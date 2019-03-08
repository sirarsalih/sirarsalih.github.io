---
layout: post
title: Securing your front-end payload with RSA encryption using OpenSSL, jsencrypt and Azure Key Vault
tags: [azure]
---

Security is very important in software development, I have been working intensively on security in the context of Azure recently and I thought that I would write a blog post on encryption. Even though your network traffic is usually secured with SSL encryption, there is a high chance that a hacker will use SSL decryption tools and be able to sniff your traffic in clear text. In such cases it's very smart to protect your payload with additional encryption. In this blog post I will explain how to encrypt the payload coming from your front-end with RSA encryption and how to decrypt it in your back-end.

So in this case we need to do RSA encryption in JavaScript (front-end) and RSA decryption in C# (back-end).

In JavaScript we can use [jsencrypt](https://www.npmjs.com/package/jsencrypt) to do the RSA encryption, which I think is a good library for doing this. The typical encryption/decryption flow looks like this:

[<img src="{{ site.url }}/public/img/encrypt_decrypt.png">]({{site.url}}/public/img/encrypt_decrypt.png)

A public key, known to all users of your application, is used for encryption. A private key, known only to you - the provider - is used for decryption. So before we get started, we need to generate public/private key pairs and a certificate. [OpenSSL](https://www.openssl.org/) is an excellent tool for doing this, so with this tool we can create a private key and certificate PEM files:

```
openssl req -newkey rsa:1024 -nodes -keyout private_key.pem -x509 -days 365 -out certificate.pem
```

Make note of the arguments; we're applying RSA 1024-bit encryption. We then create a public key PEM file using the private key and certificate that we created previously:

```
openssl x509 -pubkey -noout -in certificate.pem > public_key.pem
```

Finally, we export a certificate PFX file using the private key and certificate PEM files:

```
openssl pkcs12 -export -out certificate.pfx -inkey private_key.pem -in certificate.pem
```

When exporting the certificate, you'll be asked to set a certificate password. Give it a password, then store this password somewhere safe such as in a secret in Azure Key Vault. 

Using PowerShell, we then convert the certificate PFX to a base64 string. We need to do that so we can store the base64 string in a secret in Azure Key Vault for decryption later:

```PowerShell
$fileContentBytes = get-content 'certificate.pfx' -Encoding Byte
[System.Convert]::ToBase64String($fileContentBytes) | Out-File 'certificate_base64.txt'
```

Open ```certificate_base64.txt```, copy the content and paste it in a secret in your Azure Key Vault. Open ```public_key.pem``` using a text editor then copy the content of the key and place it in a configuration file that can be accessed by your application. Return the public key from your back-end like so:

```csharp
[Route("Home/Form")]
public async Task<IActionResult> Form()
{
    //........
    return StatusCode(200, Json(_config["RSAPublicKey"]));
}
```

Using React as example for the front-end, here is how we set the public key:

```JavaScript
componentDidMount() {
    var self = this;
    fetch('Home/Form',
        {
            method: 'GET',
            headers: {
                'Accept': 'application/json',
                'Content-Type': 'application/json'
            }
        })
        .then(function (response) {
            //Handle the response
        })
        .then((json) => {
            self.setState({ rsaPublicKey: json.value });
            return json;
        })
        .catch(function (error) {
            console.log(error);
        });
}
```

Using [jsencrypt](https://www.npmjs.com/package/jsencrypt), we are then ready to encrypt the payload prior to submitting it to the back-end:

```JavaScript
handleSubmit = event => {
    var messageJson = JSON.stringify({
        Message: this.state.messageText
    });
    var encrypt = new jsencrypt.JSEncrypt();
    encrypt.setPublicKey(this.state.rsaPublicKey);
    var encryptedMessageJson = encrypt.encrypt(messageJson);
    var wrapperJson = JSON.stringify({ encryptedMessageJson });
    fetch('Home/Submit',
        {
            method: 'POST',
            headers: {
                'Accept': 'application/json',
                'Content-Type': 'application/json'
            },
            body: wrapperJson
        }).then((response) => {
            //Handle the response
        }).catch(function (error) {
            console.log(error);
        });
    event.preventDefault();
}
```