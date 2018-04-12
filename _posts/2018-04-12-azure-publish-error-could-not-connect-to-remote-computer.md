---
layout: post
title: Azure Publish Error; Web deployment task failed. (Could not connect to the remote computer [...])
tags: [dotnet, azure, tech]
---
I recently encountered a connection problem while publishing an application service to Azure from behind my employer's firewall. Here is the complete error message:

<blockquote><em>Error 1 Web deployment task failed. (Could not connect to the remote computer [...]). On the remote computer, make sure that Web Deploy is installed and that the required process ("Web Management Service") is started. Learn more at: http://go.microsoft.com/fwlink/?LinkId=221672#ERROR_DESTINATION_NOT_REACHABLE.) </em></blockquote>

This is obviously a showstopper that hinders you from getting your site published to the public cloud from your employer's company domain. Fear not though, as there is a solution to this; by disabling SCM. The solution is based on [an answer from Patrick McCurley at StackOverflow](https://stackoverflow.com/a/29440749/1392202):

1. Go to the [Azure Portal](https://portal.azure.com).
2. Click on App Services.
3. Click on your application service.
4. Click on Application settings.
5. Scroll down to Application settings.
6. Click Add new setting, and add the following entry:

Name: **WEBSITE_WEBDEPLOY_USE_SCM**, Value: **false**

7. Click on Save and close the Application settings window.
8. Click on your application service.
9. Download the publish profile by clicking on Get publish profile.
10. Import the publish profile when publishing your application service to Azure from Visual Studio.

This effectively disables SCM and enables you to publish your application service to Azure.
