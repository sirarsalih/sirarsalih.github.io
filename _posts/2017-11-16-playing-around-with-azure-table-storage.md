---
layout: post
title: Playing around with Azure Table Storage
tags: [dotnet, tech]
---
I've been using [Azure table storage](https://azure.microsoft.com/en-us/services/storage/tables/) lately at work and I thought that I'd share my experiences with you, both good and bad. What I like about it is its ease of use and set up, requiring you to do minimal work to get it up and working in your solution. It does its job and it does it well, although it loses some of its charm when it comes down to performance and its lack of basic database operations such as "like" or "contains". If you want those operations, you'd need to implement them yourself using what's available.

Setting up Azure table storage is quite straightforward. I highly recommend using [Azure Key Vault](https://azure.microsoft.com/en-us/services/key-vault/) to store secret and common keys needed to connect to and use the table storage. Keys such as <code>storageAccountKeyName</code>, <code>storageAccountName</code> and <code>storageAccountKey</code>. Let's assume that we have a <code>TableStorageService</code> class that connects to and uses table storage, make sure that you've installed [WindowsAzure.Storage](https://www.nuget.org/packages/WindowsAzure.Storage/), then the set up would look like the following:

```csharp
private const string tableName = "Customers";
private static CloudTable _cloudTable;
public TableStorageService(KeyVaultSecretProvider keyVaultSecretProvider, string storageAccountName, string storageAccountKeyName)
{
    var storageAccountKey = keyVaultSecretProvider.GetSecret(storageAccountKeyName);
    var connectionString = $"DefaultEndpointsProtocol=https;AccountName={storageAccountName};AccountKey={storageAccountKey};EndpointSuffix=core.windows.net";
    var cloudStorageAccount = CloudStorageAccount.Parse(connectionString);
    var cloudTableClient = cloudStorageAccount.CreateCloudTableClient();
    _cloudTable = cloudTableClient.GetTableReference(tableName);
}
```

Here we're defining a table called <code>Customers</code>. The next step is to create this table if it does not exist:

```csharp
_cloudTable.CreateIfNotExists();
```

And that's pretty much it. Now let's look at some of the available database operations that we can do.

### Insert, Get, Delete

In order to insert a row into our table, we'd need to insert a <code>TableEntity</code> object. Consider the following:

```csharp
public class CustomerEntity : TableEntity
{
    public CustomerEntity() { }

    public CustomerEntity(string lastName, string firstName)
    {
        PartitionKey = lastName;
        RowKey = firstName;
    }
}
```

We can insert this object as such:

```csharp
var insertOperation = TableOperation.Insert(new CustomerEntity("Snow", "Jon"));
await _cloudTable.ExecuteAsync(insertOperation);
```

We can also do a batch insert of entities, this is really efficient if you want to store 100 or more entities at a time. Here is a simple example storing 100 entities:

```csharp
var tableBatchOperation = new TableBatchOperation();
for(var i = 0; i < 100; i++){
    tableBatchOperation.Insert(new CustomerEntity("Snow", $"Jon {i}"));
    if(i == 99) {
        await _cloudTable.ExecuteBatchAsync(tableBatchOperation);
    }
}
```

To get rows from our table, we can filter using the ParitionKey or RowKey:

```csharp
var query = new TableQuery<CustomerEntity>().Where(TableQuery.GenerateFilterCondition("RowKey", QueryComparisons.Equal, "Jon"));
_cloudTable.ExecuteQuery(query);
```

As mentioned earlier, table storage does not have a "like" or "contains" operation. If you really need it, you could [implement it yourself](https://www.codeproject.com/Tips/790554/StartsWith-comparison-for-searching-in-Azure-Table), but that's really not recommended since anything custom made would very likely take its toll on performance.

Deleting an entity is straightforward; first retrieve the entity then delete it:

```csharp
var retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Snow", "Jon");
var retrievedResult = await _cloudTable.ExecuteAsync(retrieveOperation);
var deleteEntity = (CustomerEntity)retrievedResult.Result;
var deleteOperation = TableOperation.Delete(deleteEntity);
await _cloudTable.ExecuteAsync(deleteOperation);
```

Those are the basic operations which help you get up and running with Azure table storage. In addition, I recommend downloading [Azure Storage Explorer](https://azure.microsoft.com/en-us/features/storage-explorer/) which is a tool to help you manage your tables. For more information on table storage, you can always check out the [official Microsoft documentation](https://docs.microsoft.com/en-us/azure/cosmos-db/table-storage-how-to-use-dotnet).

Hope you enjoyed this!