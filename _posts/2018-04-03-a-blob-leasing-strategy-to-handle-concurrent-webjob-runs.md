---
layout: post
title: An Azure Table Storage Blob Leasing Strategy to Handle Concurrent Azure WebJob Runs
tags: [dotnet, azure, tech]
---
[I've talked about Azure Table Storage](https://sirarsalih.com/2017/11/16/playing-around-with-azure-table-storage/) and [touched upon Azure WebJobs](https://sirarsalih.com/2018/01/25/make-cron-expressions-configurable-for-azure-webjobs/) in the past. I thought that I would write a blog post on how to use both, with a specific focus on handling concurrency problems that occur if you are doing atomic table storage operations at the same time as a result of running the same webjob(s) (that execute said table storage operations) concurrently.

The main question is; why would you run the same webjob multiple times at the same time? Well, one of the answers that I've learned is that when you run a webjob - say, especially in production - you'd want to run it on several nodes so that you can assure redundancy in the case that the webjob fails to run. Redundancy is great, but in the world of Azure Table Storage it comes at a cost; we simply cannot run singular atomic operations more than once at the same time.