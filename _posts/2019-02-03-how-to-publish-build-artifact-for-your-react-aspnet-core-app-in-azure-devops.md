---
layout: post
title: How to publish build artifact for your React ASP.NET Core web apps in Azure DevOps
tags: [azure]
---
I'd like to thank my good friend [@oddbeard](https://twitter.com/oddbeard) for a fun talk on Skype the other day where he pointed me in the right direction on this one. I use [Azure DevOps](https://azure.com/devops/) in my projects, I find it really great for working in teams, for setting up build and release pipelines. 

I'm working on a project lately where I sat up Continuous Integration (CI) and Continuous Deployment (CD) pipelines for a React ASP.NET Core web application. It seems that there is a challenge here when it comes to official documentation for Azure DevOps, as it's simply lacking the information on how to properly set up CI/CD for such web applications. I was googling for hours and trying to figure out how to properly publish build artifact in the CI, so that the artifact is picked up, read by the CD pipeline and deployed as an Azure application service. I was unable to find an answer on the Internet (that worked) and my friend [@oddbeard](https://twitter.com/oddbeard) came to the rescue. He had done something similar in the past and it worked for him. So I tried it out, and it worked!

Okay, so in Azure DevOps you need to use the "Publish" step in your CI pipeline, which uses the <code>publish</code> command to compile the dlls and client libs. Here is the necessary step, highlighted:

[<img src="{{ site.url }}/public/img/azure_devops_1.png">]({{ site.url }}/public/img/azure_devops_1.png)

Note the path to csproj and the arguments provided. The custom variables used in the arguments are defined in the variables tab:

[<img src="{{ site.url }}/public/img/azure_devops_2.png">]({{ site.url }}/public/img/azure_devops_2.png)

The artifact gets zipped, then the "Publish Artifact" step publishes the zipped artifact to a directory which is then picked up by the CD pipeline:

[<img src="{{ site.url }}/public/img/azure_devops_3.png">]({{ site.url }}/public/img/azure_devops_3.png)

Here is how the CD pipeline looks like:

[<img src="{{ site.url }}/public/img/azure_devops_4.png">]({{ site.url }}/public/img/azure_devops_4.png)

Note the package/folder path, how it's pointing to a zip. And that's pretty much it, the CD magically downloads the artifact, unpacks the dlls/client libs from the zipped file and starts the Azure application service!
