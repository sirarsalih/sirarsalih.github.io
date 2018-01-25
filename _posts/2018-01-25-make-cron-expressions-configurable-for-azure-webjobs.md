---
layout: post
title: Make cron expressions configurable for Azure WebJobs
tags: [dotnet, tech]
---
[Azure WebJobs](https://docs.microsoft.com/en-us/azure/app-service/web-sites-create-web-jobs) are great for running background tasks in the cloud. You can schedule a WebJob to execute different functions, such as updating a database, fetching data from an API, polling a service, etc. A WebJob will typically call a scheduled job to execute a function. The scheduling is done with a <code>TimerTrigger</code> that takes in a cron expression string as an argument:

```csharp
public class ScheduledJobs
{        
    public static void StartJob([TimerTrigger("0 0 2 * * *", RunOnStartup = false)] TimerInfo timerInfo)
    {
        // Do something
    }
}
```

This job runs every day at 02:00 AM UTC ([check this](https://docs.microsoft.com/en-us/azure/app-service/web-sites-create-web-jobs) for more on cron expressions). This is all well and good, but suppose that we want the job to run at different time intervals on different servers, or that we want to simply make the cron expression a configurable string in order to change it easily. How can we achieve this when the cron expression is a constant string? Well, say hello to <code>INameResolver</code> (which follows with the Azure WebJobs SDK). Implementing <code>INameResolver</code>, we can dynamically resolve a string name. Suppose that we create a configuration variable for the cron expression, fetching it using the <code>ConfigurationManager</code>, here is how the resolver would look like:

```csharp
public class ScheduleExpressionResolver : INameResolver
{
    public string Resolve(string name)
    {
        return ConfigurationManager.AppSettings[name];
    }
}
```

The App.config entry:

```xml
<appSettings>
    <add key="WorkerRole.Cron.Job.Schedule.Expression" value="0 0 2 * * *" />
</appsettings>
```

We have to then set the <code>NameResolver</code> property on the <code>JobHostConfiguration</code> object to an instance of the expression resolver when creating the WebJob:

```csharp
public class WebJob
{
    private JobHost _webJobHost;   

    public bool Start()
    {
        var config = new JobHostConfiguration();
        if (config.IsDevelopment) {
            config.UseDevelopmentSettings();
        }
        config.NameResolver = new ScheduleExpressionResolver();
        config.UseTimers();
        _webJobHost = new JobHost(config);
        _webJobHost.Start();
        return true;
    }

    public bool Stop()
    {
        _webJobHost.Stop();
        return true;
    }
}
```

Finally, we can now substitute the cron expression with the configuration variable name surrounded by percentages:

```csharp
public class ScheduledJobs
{        
    public static void StartJob([TimerTrigger("%WorkerRole.Cron.Job.Schedule.Expression%", RunOnStartup = false)] TimerInfo timerInfo)
    {
        // Do something
    }
}
```

<code>ScheduleExpressionResolver</code> will take in <code>WorkerRole.Cron.Job.Schedule.Expression</code> as a name and successfully return the cron expression. We can now tweak the cron expression in any way we like. For instance, we can change this value when deploying to different servers, so that we can apply different time intervals.