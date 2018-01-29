---
layout: post
title: Fetching an installed certificate programmatically from the Windows Local Machine Store
tags: [dotnet, tech]
---
This is quite a relevant scenario in the world of .NET security, where we sometimes need to fetch a certificate programmatically in order to validate it to some given condition(s). There is quite an amount of boiler plate code in order to achieve this, let's jump right into it. A certificate is fetched using a <code>friendly name</code>, with that in mind, let's assume that we use a <code>CertificateService</code> to fetch the certificate, then the service would look like this: