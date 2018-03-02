---
title: Signed Packages Reference | Microsoft Docs
author: rido-min
ms.author: rido-min
manager: unniravindranathan
ms.date: 02/26/2018
ms.topic: reference
ms.prod: nuget
ms.technology: null
description: Signed Packages feature description.
keywords: NuGet package sign, signature, certificate
ms.reviewer:
- ananguar
- karann-msft
- unniravindranathan
---

# Signed Packages

NuGet packages can include a digital signature which provides protection against tampered content. This signature is produced from an x509 certificate that also adds authenticity proofs to the actual origin of the package. 

This feature has been introduced in nuget.exe 4.6, while signature verification requires Visual Studio 2017 Update 15.6.

## Benefits of signed packages

Packages that have been signed with a  certificate provide the strongest end to end validation. An author signature guarantees that the package has not been modified since it left the author computer, no matter from which repository or transport method has been used .

Consumers who demand a locked-down environment can require packages signed with an specific author certificate. 

Additionally, author-signed packages provide an extra authentication mechanism to the publishing mechanism to nuget.org, because the certificate used to sign must be registered ahead of time.

## How to sign a package

[Singing Packages](../create-packages/Sign-a-package) explains how to sign a package using the [sign command](../tools/cli-ref-sign).

## Certificate Requirements

Authors require a code signing certificate, which are a special type of certificate that are valid for the `id-kp-codeSigning` purpose [[RFC 5280 section 4.2.1.12]](https://tools.ietf.org/html/rfc5280#section-4.2.1.12), and have an RSA public key length of 2048 bits or higher. 

## Get a code signing certificate

These certificates can be obtained from a public certificate authorities like:

- [Symmantec](https://trustcenter.websecurity.symantec.com/process/trust/productOptions?productType=SoftwareValidationClass3)
- [DigiCert](https://www.digicert.com/code-signing/)
- [Go Daddy](https://www.godaddy.com/web-security/code-signing-certificate)
- [Global Sign](https://www.globalsign.com/en/code-signing-certificate/)
- [Comodo](https://www.comodo.com/e-commerce/code-signing/code-signing-certificate.php)
- [Certum](https://www.certum.eu/certum/cert,offer_en_open_source_cs.xml) 

The complete list of certification authorities trusted by Windows can be obtained from [http://aka.ms/trustcertpartners](http://aka.ms/trustcertpartners).

## Create a Test certficate

You can use Self-Signed certificates for testing purposes. To create a self signed certificate you can use the powershell commmand [New-SelfSignedCertificate](https://docs.microsoft.com/en-us/powershell/module/pkiclient/new-selfsignedcertificate)

```ps
New-SelfSignedCertificate	-Subject "CN=NuGet Test Developer, OU=Use for testing purposes ONLY" `
							-FriendlyName "NuGetTestDeveloper" `
							-Type CodeSigning `
   			    			-KeyUsage DigitalSignature `
							-KeyLength 2048 `
							-KeyAlgorithm RSA `
                            -HashAlgorithm SHA256 `
							-Provider "Microsoft Enhanced RSA and AES Cryptographic Provider" `
							-CertStoreLocation "Cert:\CurrentUser\My" 
```
This command creates a testing certificate available in the personal certificate store. You can open the certificate store by running `certmgr.msc` to see the new created certificate.


## Timestamp Requirements

Signed packages should include a timestamp to ensure the validity period beyond the expiration date of the certificate. The certificate used to sign the timestamp must be valid for the `id-kp-timeStamping` purpose [[RFC 5280 section 4.2.1.12](https://tools.ietf.org/html/rfc5280#section-4.2.1.12)] and have an RSA public key length of 2048 bits or higher.

Additional technical details can be found in the [technical spec](https://github.com/NuGet/Home/wiki/Package-Signatures-Technical-Details) in GitHub.
