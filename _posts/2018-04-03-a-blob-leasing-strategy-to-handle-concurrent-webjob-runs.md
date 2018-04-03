---
layout: post
title: An Azure Table Storage Blob Leasing Strategy to Handle Concurrent Azure WebJob Runs
tags: [dotnet, azure, tech]
---
[I've talked about Azure Table Storage](https://sirarsalih.com/2017/11/16/playing-around-with-azure-table-storage/) and [touched upon Azure WebJobs](https://sirarsalih.com/2018/01/25/make-cron-expressions-configurable-for-azure-webjobs/) in the past. I thought that I would write a blog entry on how to use both, with a specific focus on handling concurrency problems that occur if you are doing atomic table storage operations at the same time as a result of running the same webjob(s) (that execute said table storage operations) concurrently.

The main question is; why would you want to run the same webjob multiple times at the same time? Well, one of the answers that I've learned is that when you run a webjob - say, especially in production - you'd want to run it on several nodes so that you can assure redundancy in the case that the webjob fails. Redundancy is great, but in the world of Azure Table Storage it comes at a cost; we simply cannot run singular atomic operations more than once at the same time.

<img src="{{ site.url }}/public/img/we_simply_cannot.jpg">

If we do that, our database logic will be flawed; queries where we are expecting single entities, delete operations and other things will start throwing runtime exceptions. We want our logic to work properly and we definitely do *not* want runtime exceptions.

So how can we achieve redundancy and at the same time avoid concurrency problems? That's where *blob leasing* comes into the picture. Blob leasing is a straightforward process: 

1. Create a blob container (one time operation)
2. Create a blob (one time operation)
3. Lease the blob
4. Do table storage operations
5. Renew the lease on blob if not yet done, or release the lease on blob if done
6. Go to step 3

Following these steps, we make sure that all table storage operations are done sequentially once, effectively terminating concurrency issues. The implementation details are entirely up to you, I will explain how I implemented this in the next parts.

**Blob Lease Manager**

A blob lease manager class is created which handles all leasing logic. It implements the following interface:

```csharp
public interface IBlobLeaseManager
{
    void Init(ILogger log);
    void Dispose();
    bool HasLease(CloudBlockBlob blob);
    string GetLeaseId(CloudBlockBlob blob);
    void ReleaseLease(CloudBlockBlob blob);
    bool TryAcquireLease(CloudBlockBlob blob, double leaseTimeInSeconds);
}
```

With the following implementation:

```csharp
public class BlobLeaseManager : IBlobLeaseManager, IDisposable
{
    private readonly Dictionary<string, Lease> acquiredLeases = new Dictionary<string, Lease>();
    private static ILogger _log;
    public void Init(ILogger log)
    {
        _log = log;
    }

    public void Dispose()
    {
        acquiredLeases.ForEach(pair => pair.Value.KeepAlive?.Dispose());
    }

    public bool HasLease(CloudBlockBlob blob)
    {
        return acquiredLeases.ContainsKey(blob.Name);
    }

    public string GetLeaseId(CloudBlockBlob blob)
    {
        return HasLease(blob) ? acquiredLeases[blob.Name].LeaseId : string.Empty;
    }

    public void ReleaseLease(CloudBlockBlob blob)
    {
        if (!HasLease(blob)) return;
        var leaseId = GetLeaseId(blob);
        ClearLease(blob);
        try
        {
            blob.ReleaseLease(new AccessCondition
            {
                LeaseId = leaseId
            });
        }
        catch (StorageException ex)
        {
            _log.Information("Failed attempt at releasing blob lock on {blob.Name} using lease id {leaseId}. Unhandled exception: {ex.Message}.", blob.Name, leaseId, ex.Message);
        }
    }

    public bool TryAcquireLease(CloudBlockBlob blob, double leaseTimeInSeconds)
    {
        // Lease time limit: https://docs.microsoft.com/en-us/rest/api/storageservices/Lease-Blob?redirectedfrom=MSDN
        if (DoesNotMeetLeaseTimeLimit(leaseTimeInSeconds))
            throw new ArgumentException("Value must be between 15 and 60.");
        try
        {
            var proposedLeaseId = Guid.NewGuid().ToString();
            var leaseTime = TimeSpan.FromSeconds(leaseTimeInSeconds);
            var leaseId = blob.AcquireLease(leaseTime, proposedLeaseId);
            UpdateAcquiredLease(blob, leaseId, leaseTimeInSeconds);
            return true;
        }
        catch (StorageException ex)
        {
            _log.Information("Failed to acquire lease {blob.Name}. Unhandled exception: {ex.Message}.", blob.Name, ex.Message);
            return false;
        }
    }

    private bool DoesNotMeetLeaseTimeLimit(double leaseTimeInSeconds)
    {
        return leaseTimeInSeconds < 15 || leaseTimeInSeconds > 60;
    }

    private void UpdateAcquiredLease(CloudBlockBlob blob, string leaseId, double lockTimeInSeconds)
    {
        var name = blob.Name;
        if (IsAcquiredLeaseMissMatched(name, leaseId)) {
            ClearLease(blob);
        }
        else {
            acquiredLeases.Add(name, MakeLease(blob, leaseId, lockTimeInSeconds));
        }
    }

    private bool IsAcquiredLeaseMissMatched(string name, string leaseId)
    {
        return acquiredLeases.ContainsKey(name) && acquiredLeases[name].LeaseId != leaseId;
    }

    private void ClearLease(CloudBlockBlob blob)
    {
        var name = blob.Name;
        var lease = acquiredLeases[name];
        lease.KeepAlive?.Dispose();
        acquiredLeases.Remove(name);
    }

    private Lease MakeLease(CloudBlockBlob blob, string leaseId, double lockTimeInSeconds)
    {
        var interval = TimeSpan.FromSeconds(lockTimeInSeconds - 1);
        return new Lease
        {
            LeaseId = leaseId,
            KeepAlive = Observable.Interval(interval).Subscribe(l => RenewLease(blob))
        };
    }

    private bool RenewLease(CloudBlockBlob blob)
    {
        if (!HasLease(blob)) return false;
        var name = blob.Name;
        try
        {
            blob.RenewLease(new AccessCondition
            {
                LeaseId = acquiredLeases[name].LeaseId
            });

            _log.Information("Renewed lease for {name}.", name);

            return true;
        }
        catch (StorageException ex)
        {
            acquiredLeases.Remove(name);
            _log.Information("Removed lease from acquired leases for blob {name}. Unhandled exception: {ex.Message}.", name, ex.Message);
            return false;
        }
    }

    private struct Lease
    {
        internal string LeaseId { get; set; }
        internal IDisposable KeepAlive { get; set; }
    }
}
```

We use a lease struct with a lease id and an IDisposable which keeps track of the lease life cycle. Make special note of the <code>TryAcquireLease</code>, <code>UpdateAcquiredLease</code> and <code>ReleaseLease</code> methods.

**Job Reservation Service**

A job reservation service class is created which abstracts some of the *BlobLeaseManager* methods in addition to handling the job reservations. This class also calls a private class, <code>JobReservationLog</code>, which keeps track of the job reservations. It implements the following interface:

```csharp
public interface IJobReservationService
{
    void Init(string connectionString, ILogger log);
    bool TryReserveJob(string jobName, double jobReservationInSeconds);
    bool HasReservation(string jobName);
    void CancelReservation(string jobName);
    void Dispose();
}
```

With the following implementation:

```csharp
public class JobReservationService : IJobReservationService, IDisposable
{
    private const string JobsContainerName = "jobs";
    private string _reserverName;
    private readonly IBlobLeaseManager _blobLeaseManager;
    private CloudBlobContainer _cloudBlobContainer;
    private ILogger _log;
    public JobReservationService(IBlobLeaseManager blobLeaseManager)
    {
        _blobLeaseManager = blobLeaseManager;
    }

    public void Init(string connectionString, ILogger log)
    {
        try
        {
            _log = log;
            _reserverName = $"Reserver-{Guid.NewGuid()}";
            _blobLeaseManager.Init(_log);

            _log.Information("Initializing job reservation service with reserver name {_reserverName}.", _reserverName);

            var cloudStorageAccount = CloudStorageAccount.Parse(connectionString);
            var cloudBlobClient = cloudStorageAccount.CreateCloudBlobClient();
            var deltaBackoff = new TimeSpan(0, 0, 0, 2);
            cloudBlobClient.RetryPolicy = new ExponentialRetry(deltaBackoff, 10);
            _cloudBlobContainer = cloudBlobClient.GetContainerReference(JobsContainerName);
            _cloudBlobContainer.CreateIfNotExists();
        }
        catch (Exception ex)
        {
            _log.Error(ex, $"Error initializing job reservation service. Unhandled exception: {ex.Message}.");
            throw;
        }
    }

    public bool TryReserveJob(string jobName, double jobReservationInSeconds)
    {
        var blobReference = GetJobReservationBlob(jobName);
        if (!blobReference.Exists()) InitializeLeaseBlob(blobReference);
        var acquireLease = _blobLeaseManager.TryAcquireLease(blobReference, jobReservationInSeconds);
        if (acquireLease) UpdateReservationLog(jobName);
        return acquireLease;
    }

    public bool HasReservation(string jobName)
    {
        return _blobLeaseManager.HasLease(GetJobReservationBlob(jobName));
    }

    public void CancelReservation(string jobName)
    {
        _blobLeaseManager.ReleaseLease(GetJobReservationBlob(jobName));
    }

    private CloudBlockBlob GetJobReservationBlob(string jobName)
    {
        var blobReference = _cloudBlobContainer.GetBlockBlobReference(jobName);
        return blobReference;
    }

    private void InitializeLeaseBlob(CloudBlockBlob blobReference)
    {
        var log = new JobReservationLog();
        UpdateBlobContent(log, blobReference);
    }

    private void UpdateBlobContent(JobReservationLog jobReservationLog, CloudBlockBlob jobReservationBlob)
    {
        jobReservationLog.Add(MakeJobReservation());
        var leaseId = _blobLeaseManager.GetLeaseId(jobReservationBlob);
        var accessCondition = string.IsNullOrWhiteSpace(leaseId) ? null : new AccessCondition { LeaseId = leaseId };
        jobReservationBlob.UploadText(jobReservationLog.ToJson(), null, accessCondition);
    }

    private void UpdateReservationLog(string jobName)
    {
        var blobReference = GetJobReservationBlob(jobName);
        var jobReservationLog = JobReservationLog.Make(blobReference.DownloadText());
        var lastReservation = jobReservationLog.LastReservation;
        if (lastReservation.Reserver == _reserverName) return;
        UpdateBlobContent(jobReservationLog, blobReference);
    }

    private JobReservation MakeJobReservation()
    {
        return new JobReservation
        {
            Obtained = DateTime.UtcNow,
            Reserver = _reserverName
        };
    }

    public void Dispose()
    {
        _blobLeaseManager.Dispose();
    }

    public struct JobReservation
    {
        public string Reserver { get; set; }
        public DateTime Obtained { get; set; }
    }

    private class JobReservationLog
    {
        private readonly List<JobReservation> _reservations = new List<JobReservation>();

        public JobReservationLog()
        {
        }

        private JobReservationLog(List<JobReservation> lockReservations)
        {
            _reservations = lockReservations;
        }

        internal JobReservation LastReservation => _reservations.FirstOrDefault();

        internal static JobReservationLog Make(string json)
        {
            var list = JsonConvert.DeserializeObject<List<JobReservation>>(json);
            return new JobReservationLog(list);
        }

        internal void Add(JobReservation jobReservation)
        {
            _reservations.Insert(0, jobReservation);
            if (_reservations.Count > 10) _reservations.Remove(_reservations.Last());
        }

        internal string ToJson()
        {
            return JsonConvert.SerializeObject(_reservations);
        }
    }
}
```

Make special note of the <code>Init</code>, <code>TryReserveJob</code>, <code>HasReservation</code> and <code>CancelReservation</code> methods.

**The Job**

Now onto the job itself. Let's just assume for simplicity's sake that we have a job, called <code>Job</code>, that connects to a <code>Customers</code> table and then simply logs to console without doing anything else:

```csharp
public class Job : IJob
{
    private const string CustomersTableName = "Customers";    
    private static CloudTable _customersTable;
    private static WorkerRole _workerRole;
    private static ILogger _log;

    public Job(string connectionString, ILogger log)
    {
        var cloudStorageAccount = CloudStorageAccount.Parse(connectionString);
        var cloudTableClient = cloudStorageAccount.CreateCloudTableClient();    
        _customersTable = cloudTableClient.GetTableReference(CustomersTableName);       
        _workerRole = null;
        _log = log;
    }

    public void Execute(WorkerRole workerRole = null)
    {
        _workerRole = workerRole;

        _log.Information("Starting job.");

        Update();

        _log.Information("Finished job.");

        // Remove reservation/release lease from job
        _workerRole?.OnStop();
    }

    private void Update()
    {
        _log.Information("Updating...");        
    }
}
```

Make special note of the <code>WorkerRole</code> object which we pass to the <code>Execute</code> method. More on that next.

**Worker Role**

A worker role class acts like a puppeteer, calling the job(s) and the job reservation service to handle the job executions:

```csharp
public class WorkerRole : RoleEntryPoint
{
    private static string _connectionString;
    private static IJobReservationService _jobReservationService;    
    private static ILogger _log;
    private string _job;
    private bool _isStopped;

    public WorkerRole(string connectionString, IJobReservationService jobReservationService, ILogger log)
    {
        _connectionString = connectionString;
        _log = log;
        _jobReservationService = jobReservationService;
        _jobReservationService.Init(connectionString, _log);
    }

    private readonly Dictionary<string, Func<IJob>> _jobs = new Dictionary<string, Func<IJob>>
    {
        { nameof(Job), () => new Job(_connectionString, _log) }
    };

    public override void Run()
    {
        try
        {
            _log.Information("Worker role entry point called.");
            if (!string.IsNullOrWhiteSpace(_job)) Task.Run(() => _jobs[_job].Invoke().Execute(this));
            while (true) {
                if (_isStopped) break;
                Thread.Sleep(15000);
                TraceJobExecution();
            }
        }
        catch (Exception ex)
        {
            _log.Error($"Unhandled exception: {ex.Message}", ex);
            throw;
        }
    }

    private void TraceJobExecution()
    {
        if (string.IsNullOrWhiteSpace(_job)) return;
        _log.Information("Reservation is currently made on job {Job}.", _job);
    }

    public override bool OnStart()
    {
        try
        {
            _log.Information("Starting worker role.");
            ServicePointManager.DefaultConnectionLimit = 12;
            _job = SelectJob();
            return base.OnStart();
        }
        catch (Exception ex)
        {
            _log.Error($"Unhandled exception: {ex.Message}", ex);
            throw;
        }
    }

    private string SelectJob()
    {
        foreach (var j in _jobs) {
            if (!_jobReservationService.TryReserveJob(j.Key, 60)) continue;
            return j.Key;
        }
        return string.Empty;
    }

    public override void OnStop()
    {
        try
        {
            _log.Information("Stopping worker role.");
            if (!string.IsNullOrWhiteSpace(_job)) {
                _log.Information("Removing reservation from job {Job}.", _job);
                _jobReservationService.CancelReservation(_job);
                _job = string.Empty;
            }
            _isStopped = true;
            base.OnStop();
        }
        catch (Exception ex)
        {
            Log.Error("Unhandled exception: {message}", ex.Message);
            _log.Error($"Unhandled exception: {ex.Message}", ex);
            throw;
        }
    }
}
```

Make special note of the <code>Run</code>, <code>OnStart</code> and <code>OnStop</code> methods. As you can see from the <code>Run</code> method, the job is executed in a thread and then we wait until the job execution is complete before breaking out of the while loop. We are blocking the UI thread for 15 seconds, as the lease time of a blob is anyway [between 15 and 60 seconds](https://docs.microsoft.com/en-us/rest/api/storageservices/Lease-Blob?redirectedfrom=MSDN).

**Scheduled Jobs**

We finally arrive at the last bit, this is where it all starts. A webjob class is created which we call <code>ScheduledJobs</code>, executes the worker role which starts the whole process:

```csharp
public class ScheduledJobs
{
    // Cron job: {second} {minute} {hour} {day} {month} {day of the week}
    // Every minute: 0 0/1 * * * *
    // Every hour: 0 0 */1 * * *
    // Every day at 2:00 AM UTC: 0 0 2 * * *
    public static void StartWorkerRole([TimerTrigger("0 0 2 * * *", RunOnStartup = false)] TimerInfo timerInfo, ILogger log)
    {
        var workerRole = new WorkerRole(WebJob.StorageConnectionString, IoC.Resolve<IJobReservationService>(), log);
        if (workerRole.OnStart()) {
            workerRole.Run();
        } else {
            workerRole.OnStop();
        }
    }
}
```

And that pretty much wraps up everything. Let me know in the comments if you have any questions, recommendations or any other inquiries. Good luck with your implementation!