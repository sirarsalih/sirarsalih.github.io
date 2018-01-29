---
layout: post
title: Fetching an installed certificate programmatically from the Windows Local Machine Store
tags: [dotnet, tech]
---
Nowadays we do things in Azure but this is quite a relevant scenario in the world of .NET security, where we sometimes need to fetch a certificate programmatically in order to validate it to some given condition(s). There is quite an amount of boiler plate code in order to achieve this, so let's jump right into it. 

A certificate is fetched using a public <code>friendly name</code>, let's assume that we use a <code>CertificateService</code> to fetch the certificate, then the service would look like this:

```csharp
using System.Linq;
using System.Security.Cryptography.X509Certificates;

public class CertificateService : ICertificateService
{
    public byte[] GetCertificateFromLocalMachineStore(string friendlyName)
    {
        var store = GetLocalMachineCertificates();
        X509Certificate2 certificate = null;
        foreach (var cert in store.Cast<X509Certificate2>().Where(cert => cert.FriendlyName.Equals(friendlyName))) {
            certificate = cert;
        }
        return certificate != null ? certificate.Export(X509ContentType.Pkcs12) : null;    
    }

    private static X509Certificate2Collection GetLocalMachineCertificates()
    {
        var localMachineStore = new X509Store(StoreLocation.LocalMachine);
        localMachineStore.Open(OpenFlags.ReadOnly);
        var certificates = localMachineStore.Certificates;
        localMachineStore.Close();
        return certificates;
    }
}
```

We first fetch all the local machine certificates, then we loop through them and fetch the certificate that we're looking for using the <code>friendly name</code>.

If you're lucky enough and get to work with WCF (pun intended), and would like to fetch the certificate through WCF, you can encapsulate the certificate as a property of a response object:

```csharp
using System;
using System.Runtime.Serialization;

[Serializable]
[DataContract(Namespace = Constants.DATA_NAMESPACE)]
public class CertificateResponse
{
    [DataMember]
    public byte[] Certificate { get; set; }
]
```

Then you'd need to change the method that fetches the certificate to return the response object:

```csharp
public CertificateResponse GetCertificateFromLocalMachineStore(string friendlyName)
{
    var store = GetLocalMachineCertificates();
    X509Certificate2 certificate = null;
    foreach (var cert in store.Cast<X509Certificate2>().Where(cert => cert.FriendlyName.Equals(friendlyName))) {
        certificate = cert;
    }
    var certBytes = certificate != null ? certificate.Export(X509ContentType.Pkcs12) : null;
    return new CertificateResponse() { Certificate = certBytes };    
}
```