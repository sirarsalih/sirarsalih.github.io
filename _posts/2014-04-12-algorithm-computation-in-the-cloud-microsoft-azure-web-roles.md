---
layout: post
title: 'Algorithm Computation in the Cloud: Microsoft Azure Web Roles'
date: 2014-04-12 18:25:46.000000000 +02:00
type: post
published: true
status: publish
categories:
tags: [dotnet]
meta:
  _edit_last: '54045106'
  _publicize_pending: '1'
  _oembed_4117bd2ed79c63dd8e40bf04f5c78ad5: "{{unknown}}"
author:
  login: sirars
  email: sirars@gmail.com
  display_name: Sirar Salih
  first_name: ''
  last_name: ''
---
<p>Worker and Web Roles are some of the great features that <a href="http://azure.microsoft.com/" title="Microsoft Azure">Microsoft Azure</a> has to offer. These two features are designed to do computation for you in the cloud. A worker role is basically a virtual machine that can be used as a back-end application server in the cloud. Similarly, a web role is a virtual machine hosted in the cloud, but the difference is that this one is used as a front-end server that requires Internet Information Services (IIS). So you can use a web role if you want to have an interface exposed to the client - for example an ASP.NET web site - that makes interaction from the outside possible.</p>
<p>In the following tutorial, I will focus on web roles. I will show you how to create a web role and host it on an Azure web site. The web role's task will be to launch a console application - which is a path finder algorithm that I wrote in C++ - that reads input from the client, computes and then returns results. The reason for choosing a web role for this, is to make communication possible through REST, so the client can use a web site or simply do a GET request to fire up the console application in the cloud and get back the results as JSON.</p>
<p><strong>Creating the Azure Web Role</strong></p>
<p>The first step is to download the <a href="http://azure.microsoft.com/en-us/downloads/" title="Microsoft Azure SDK">Microsoft Azure SDK</a>. Since I'm a .NET developer, I downloaded the Visual Studio 2013 version. The SDK includes an Azure emulator, that we will be using locally. The web role has to be hosted somewhere, we will choose a cloud service for that. So, start up Visual Studio and create a Windows Azure Cloud Service project:</p>
<p><a href="https://sirars.files.wordpress.com/2014/04/cloud.png"><img src="https://sirars.files.wordpress.com/2014/04/cloud.png" alt="cloud" width="440" height="304" class="alignnone size-full wp-image-404" /></a></p>
<p>Choose ASP.NET Web Role:</p>
<p><a href="https://sirars.files.wordpress.com/2014/04/cloud2.png"><img src="https://sirars.files.wordpress.com/2014/04/cloud2.png" alt="cloud2" width="440" height="253" class="alignnone size-full wp-image-408" /></a></p>
<p>Choosing the front-end stack type is up to you, I choose an MVC Web API without authentication:</p>
<p><a href="https://sirars.files.wordpress.com/2014/04/cloud3.png"><img src="https://sirars.files.wordpress.com/2014/04/cloud3.png" alt="cloud3" width="440" height="308" class="alignnone size-full wp-image-409" /></a></p>
<p>Go to the <code>WebRole</code> class in the WebRole project and set a break point inside the <code>OnStart()</code> method. Now build and run your cloud service, you will see that an Azure Compute Emulator is started up and the break point is reached:</p>
<p><a href="https://sirars.files.wordpress.com/2014/04/cloud4.png"><img src="https://sirars.files.wordpress.com/2014/04/cloud4.png" alt="cloud4" width="440" height="275" class="alignnone size-full wp-image-411" /></a></p>
<p>Press <code>F5</code> to continue. Your web role web site is now up and running. Right click the Azure Compute Emulator icon in the task bar, and choose "Show Compute Emulator UI". A new window shows up, click on your web role in the left column:</p>
<p><a href="https://sirars.files.wordpress.com/2014/04/cloud5.png"><img src="https://sirars.files.wordpress.com/2014/04/cloud5.png" alt="cloud5" width="440" height="327" class="alignnone size-full wp-image-413" /></a></p>
<p>This shows you the status of the web role. A web role inherits the abstract class <code>RoleEntryPoint</code> that has three virtual methods; <code>OnStart()</code>, <code>Run()</code> and <code>OnStop()</code>. These methods are called as their name suggests and can be overridden. We already overrode the <code>OnStart()</code> method as we saw earlier. Now, the next step is to launch the console application as a process from the web role.</p>
<p><strong>Starting a Console Application Process from the Azure Web Role</strong></p>
<p>Delete the default override of <code>OnStart()</code> in the <code>WebRole</code> class. We want to call a custom method from our web role independently. Create an interface <code>IWebRole</code> that looks like this:</p>

```csharp
public interface IWebRole
{
   string RunInternalProcess(string stringParams);
}
```

<p>Make <code>WebRole</code> implement this interface. <code>RunInternalProcess(string stringParams)</code> is a custom method that we will call from the client. The method will then launch a console application process and return results back to the client as JSON. We want the process to do the operation asynchronously. Here is part of how the implementation looks like:</p>

```csharp
public string RunInternalProcess(string stringParams)
{
    var path = HttpContext.Current.Server.MapPath("..\\TSP_Genetic_Algorithm.exe");
    var result = RunProcessAndGetOutputAsync(stringParams, path).Result;
    return result.Replace(" ", "\n");
}

private static async Task<string> RunProcessAndGetOutputAsync(string stringParams, string path)
{
    return await RunProcessAndGetOutput(stringParams, path);
}

private static Task<string> RunProcessAndGetOutput(string stringParams, string path)
{
    var process = CreateProcess(stringParams, path);
    process.Start();
    var result = process.StandardOutput.ReadToEnd();
    process.WaitForExit();
    var taskCompletionSource = CreateTaskCompletionSourceAndSet(result);
    return taskCompletionSource.Task;
}
```

<p>As you can see, the method starts a process called <code>TSP_Genetic_Algorithm.exe</code> which is included in the project. It's important to set the "Copy to Output Directory" property of this executable file to "Copy always", such that it's always copied to the project directory. You can do this by right clicking the executable, and choosing "Properties":</p>
<p><a href="https://sirars.files.wordpress.com/2014/04/cloud15.png"><img src="https://sirars.files.wordpress.com/2014/04/cloud15.png" alt="cloud15" width="440" height="558" class="alignnone size-full wp-image-446" /></a></p>
<p>The next step is to make the client call up the web role through an HTTP GET request.</p>
<p><strong>Calling the Azure Web Role from the Client</strong></p>
<p>We need to make it possible for the client to call the web role, we will do this by creating an <code>HttpGet ActionResult</code>. Go to <code>HomeController</code> and inject the <code>WebRole</code> interface there:</p>

```csharp
private readonly IWebRole _webRole;

public HomeController(IWebRole webRole)
{
    _webRole = webRole;
}
```

Create an ```HttpGet ActionResult```, it can look like this:

```csharp
public ActionResult Solve(string[] c)
{
    var coordinates = c.Aggregate(string.Empty, (current, t) =&gt; current + (t + " "));
    _results = _webRole.RunInternalProcess(coordinates);
    return Json(new { results = _results }, JsonRequestBehavior.AllowGet);
}
```

<p>This GET request takes in an array of string coordinates, calls the web role with these coordinates, which in turn launches up the process with the input and then finally return the results back to the client as JSON. Beautiful, isn't it? Build and run your cloud service. Now type this in the browser address field:</p>
<pre>http://127.0.0.1:81/Home/Solve?c=200,300&amp;c=400,300&amp;c=400,400&amp;c=400,200&amp;c=500,200&amp;c=200,400&amp;c=400,300</pre>
<p>And here are the results:</p>
<p><a href="https://sirars.files.wordpress.com/2014/04/cloud11.png"><img src="https://sirars.files.wordpress.com/2014/04/cloud11.png" alt="cloud11" width="440" height="53" class="alignnone size-full wp-image-432" /></a></p>
<p><strong>Publishing the Azure Cloud Service</strong></p>
<p>The final step is to publish our Azure cloud service so it goes online. This process is pretty straightforward, but it assumes that you have a <a href="http://blogs.msdn.com/b/arunrakwal/archive/2012/04/09/what-is-azure-subscription-and-how-to-setup-a-windows-azure-subscription.aspx" title="Subscription">Microsoft Azure subscription</a> and a target profile. Once you have registered a subscription, right click the cloud service project and choose "Publish...":</p>
<p><a href="https://sirars.files.wordpress.com/2014/04/cloud7.png"><img src="https://sirars.files.wordpress.com/2014/04/cloud7.png" alt="cloud7" width="440" height="343" class="alignnone size-full wp-image-426" /></a></p>
<p>Go through the wizard to create a target profile:</p>
<p><a href="https://sirars.files.wordpress.com/2014/04/cloud8.png"><img src="https://sirars.files.wordpress.com/2014/04/cloud8.png" alt="cloud8" width="440" height="295" class="alignnone size-full wp-image-427" /></a></p>
<p>Enable Remote Desktop if you want remote access to your virtual machine in Azure, this is pretty handy. Once done, click on Publish in the last dialog:</p>
<p><a href="https://sirars.files.wordpress.com/2014/04/cloud9.png"><img src="https://sirars.files.wordpress.com/2014/04/cloud9.png" alt="cloud9" width="440" height="295" class="alignnone size-full wp-image-428" /></a></p>
<p>The publishing process will start in Visual Studio:</p>
<p><a href="https://sirars.files.wordpress.com/2014/04/cloud10.png"><img src="https://sirars.files.wordpress.com/2014/04/cloud10.png" alt="cloud10" width="440" height="157" class="alignnone size-full wp-image-429" /></a></p>
<p>And then complete:</p>
<p><a href="https://sirars.files.wordpress.com/2014/04/cloud12.png"><img src="https://sirars.files.wordpress.com/2014/04/cloud12.png" alt="cloud12" width="440" height="157" class="alignnone size-full wp-image-434" /></a></p>
<p>That's it! Your Azure web role web site is now online, and you can now do the GET request through the web:</p>
<p><a href="https://sirars.files.wordpress.com/2014/04/cloud14.png"><img src="https://sirars.files.wordpress.com/2014/04/cloud14.png" alt="cloud14" width="440" height="44" class="alignnone size-full wp-image-443" /></a> </p>
<p>You can go to the <a href="https://manage.windowsazure.com" title="Microsoft Azure Portal">Azure Portal</a> to view statistics and maintain your web role there:</p>
<p><a href="https://sirars.files.wordpress.com/2014/04/cloud13.png"><img src="https://sirars.files.wordpress.com/2014/04/cloud13.png" alt="cloud13" width="440" height="410" class="alignnone size-full wp-image-436" /></a></p>
<p>Hope you enjoyed this tutorial!</p>
