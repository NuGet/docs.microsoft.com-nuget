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

# Signed packages

NuGet packages can include a digital signature which provides protection against tampered content. This signature is produced from an X.509 certificate that also adds authenticity proofs to the actual origin of the package. 

NuGet 4.6 introduces the ability to sign NuGet packages and verify signed NuGet packages. Visual Studio 2017 Update 6 introduces the ability to verify signed NuGet packages.

## Benefits of signed packages

Signed packages provide the strongest end-to-end validation. An author signature guarantees that the package has not been modified since the author signed the package, no matter from which repository or what transport method the package is delivered.

Consumers who demand a locked-down environment can require packages signed with an specific author certificate.

Additionally, author-signed packages provide an extra authentication mechanism to nuget.org's publishing pipeline because the signing certificate must be registered ahead of time.

## How to sign a package

[Signing Packages](../create-packages/Sign-a-package.md) explains how to sign a package using the [sign command](../tools/cli-ref-sign.md).

## Certificate Requirements

Package signing requires a code signing certificate, which is a special type of certificate that is valid for the `id-kp-codeSigning` purpose [[RFC 5280 section 4.2.1.12](https://tools.ietf.org/html/rfc5280#section-4.2.1.12)]. Additionally, the certificate must have an RSA public key length of 2048 bits or higher.

## Get a code signing certificate

Valid certificates may be obtained from public certificate authorities like:

- [Symmantec](https://trustcenter.websecurity.symantec.com/process/trust/productOptions?productType=SoftwareValidationClass3)
- [DigiCert](https://www.digicert.com/code-signing/)
- [Go Daddy](https://www.godaddy.com/web-security/code-signing-certificate)
- [Global Sign](https://www.globalsign.com/en/code-signing-certificate/)
- [Comodo](https://www.comodo.com/e-commerce/code-signing/code-signing-certificate.php)
- [Certum](https://www.certum.eu/certum/cert,offer_en_open_source_cs.xml) 

The complete list of certification authorities trusted by Windows can be obtained from [http://aka.ms/trustcertpartners](http://aka.ms/trustcertpartners).

## Create a test certficate

You can use self-issued certificates for testing purposes. To create a self-issued certificate you can use the PowerShell commmand [New-SelfSignedCertificate](https://docs.microsoft.com/en-us/powershell/module/pkiclient/new-selfsignedcertificate)

```ps
New-SelfSignedCertificate	-Subject "CN=NuGet Test Developer, OU=Use for testing purposes ONLY" `
							-FriendlyName "NuGetTestDeveloper" `
							-Type CodeSigning `
   			    			-KeyUsage DigitalSignature `
							-KeyLength 2048 `
							-KeyAlgorithm RSA `
							-Provider "Microsoft Enhanced RSA and AES Cryptographic Provider" `
                            -HashAlgorithm SHA256 `
							-Provider "Microsoft Enhanced RSA and AES Cryptographic Provider" `
							-CertStoreLocation "Cert:\CurrentUser\My" 
```
This command creates a testing certificate available in the current user's personal certificate store. You can open the certificate store by running `certmgr.msc` to see the newly created certificate.


## Timestamp requirements

Signed packages should include an RFC 3161 timestamp to ensure signature validity beyond the package signing certificate's validity period. The certificate used to sign the timestamp must be valid for the `id-kp-timeStamping` purpose [[RFC 5280 section 4.2.1.12](https://tools.ietf.org/html/rfc5280#section-4.2.1.12)]. Additionally, the certificate must have an RSA public key length of 2048 bits or higher.

Additional technical details can be found in the [technical spec](https://github.com/NuGet/Home/wiki/Package-Signatures-Technical-Details) in GitHub.
