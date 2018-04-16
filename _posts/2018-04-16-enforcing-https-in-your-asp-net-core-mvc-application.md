---
layout: post
title: Enforcing HTTPS in Your ASP.NET Core MVC Application
tags: [dotnet, tech]
---
You'll be quite surprised to know how many websites use unsecure HTTP URLs out there. It's 2018 and this is *still* an issue, I honestly cringe whenever I visit a website with an unsecure URL. It was more common place back in the days, but today I really don't see a reason not to apply SSL to your website. It's really pretty straightforward, all you need is a certificate ([Let's Encrypt](https://letsencrypt.org/) is an excellent choice) and a domain.

So applying SSL to your website is one thing, you also need to enforce it so that all non-secure URLs redirect to the secure ones. In the following, I will explain how to enable and enforce HTTPS in your ASP.NET Core MVC application.

When you create an ASP.NET Core MVC application, you'll have a <code>Startup.cs</code> file. Go to that file, then add the following to the <code>ConfigureServices</code> method:

```csharp
services.Configure<MvcOptions>(options =>
{
    options.Filters.Add(new RequireHttpsAttribute());
});
```

This makes your website require HTTPS. Now in the <code>Startup.cs</code> file, add the following to the <code>Configure</code> method:

```csharp
var options = new RewriteOptions().AddRedirectToHttps();
app.UseRewriter(options);
```

This redirects all URLs to HTTPS, which is excellent!

I also recommend enforcing HTTPS while debugging your application locally (a self-signed certificate is created for this purpose), you can do that by going through the following steps:

1. Right click the project.
2. Click on Properties.
3. Click on Debug.
4. Tick the Enable SSL checkbox.
5. Add https://localhost:44330 to the App URL field.
6. Save project.

Now your website will use HTTPS whenever you run it locally.