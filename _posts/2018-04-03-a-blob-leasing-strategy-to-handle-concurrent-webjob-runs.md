---
layout: post
title: An Azure Table Storage Blob Leasing Strategy to Handle Concurrent Azure WebJob Runs
tags: [dotnet, azure, tech]
---
[I've talked about Azure Table Storage](https://sirarsalih.com/2017/11/16/playing-around-with-azure-table-storage/) and [touched upon Azure WebJobs](https://sirarsalih.com/2018/01/25/make-cron-expressions-configurable-for-azure-webjobs/) in the past. I thought that I would write a blog post on how to use both, with a specific focus on handling concurrency problems that occur if you are doing atomic table storage operations at the same time as a result of running the same webjob(s) (that execute said table storage operations) concurrently.