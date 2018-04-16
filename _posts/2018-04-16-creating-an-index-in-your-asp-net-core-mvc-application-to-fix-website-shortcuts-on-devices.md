---
layout: post
title: Creating an index.html in Your ASP.NET Core MVC Application to Fix Website Shortcuts on Devices
tags: [dotnet, tech]
---
While working on an ASP.NET Core MVC PWA application, I noticed that if users created a shortcut to the application in their home screens on their devices, an <code>/index.html</code> was added to the application URL.

So if your application URL looks like this:

<code>https://{YourApplicationName}.azurewebsites.net</code>

Then if users create a shortcut to your application on their devices and add it to the home screen, it will look like this:

<code>https://{YourApplicationName}.azurewebsites.net/index.html</code>

The latter URL will then either return a blank page, or return a <code>404 Not found</code> error.

It's possible to fix this problem by redirecting from <code>/index.html</code> to the actual index of the application. In your ASP.NET Core MVC application, create an empty index.html file and add it to the root folder:

[<img src="{{ site.url }}/public/img/index_solution.png">]({{ site.url }}/public/img/index_solution.png)

To ensure that the application will use this file, go to <code>Startup.cs</code> and add the following method call:

```csharp
app.UseDefaultFiles();
```
*Before* calling the following method:

```csharp
app.UseStaticFiles();
```

Now go to index.html and write the following JavaScript code which force redirects from <code>/index.html</code> to the actual, default application home index:

```javascript
<script type="text/javascript">
    window.onload = function () {
        var url = window.location.href;
        if (url.indexOf("index.html") >= 0) {
            url = url.replace("index.html", "");
            redirectPage(url);
        } else {
            redirectPage(url);
        }
    };

    function redirectPage(baseUrl) {
        var url = baseUrl + "Home/Index";
        window.location.href = url;
    };
</script>
```

And that solves the problem, now users will be able to access your application from their home screen shortcuts.