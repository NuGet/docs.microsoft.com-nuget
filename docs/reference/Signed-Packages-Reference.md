---
title: Signed NuGet Packages Reference
description: Requirements for NuGet package signing.
author: rido-min
ms.author: rido-min
manager: unnir
ms.date: 04/24/2018
ms.topic: reference
ms.reviewer: ananguar
---

# Signed packages

*NuGet 4.6.0+ and Visual Studio 2017 version 15.6 and later*

NuGet packages can include a digital signature that provides protection against tampered content. This signature is produced from an X.509 certificate that also adds authenticity proofs to the actual origin of the package.

Signed packages provide the strongest end-to-end validation. An author signature guarantees that the package has not been modified since the author signed the package, no matter from which repository or what transport method the package is delivered.

Consumers who demand a locked-down environment can require packages signed with a specific author certificate.

Additionally, author-signed packages provide an extra authentication mechanism to the nuget.org publishing pipeline because the signing certificate must be registered ahead of time. For more information, see [Register certificates](register-certificates.md).

For details on creating a signed package, see [Signing Packages](../create-packages/Sign-a-package.md) and the [nuget sign command](../tools/cli-ref-sign.md).

> [!Important]
> Package signing is currently supported only when using nuget.exe on Windows. Verification of signed packages is currently supported only when using nuget.exe or Visual Studio on Windows.

## Certificate requirements

Package signing requires a code signing certificate, which is a special type of certificate that is valid for the `id-kp-codeSigning` purpose [[RFC 5280 section 4.2.1.12](https://tools.ietf.org/html/rfc5280#section-4.2.1.12)]. Additionally, the certificate must have an RSA public key length of 2048 bits or higher.

## Get a code signing certificate

Valid certificates may be obtained from public certificate authorities like:

- [Symantec](https://trustcenter.websecurity.symantec.com/process/trust/productOptions?productType=SoftwareValidationClass3)
- [DigiCert](https://www.digicert.com/code-signing/)
- [Go Daddy](https://www.godaddy.com/web-security/code-signing-certificate)
- [Global Sign](https://www.globalsign.com/en/code-signing-certificate/)
- [Comodo](https://www.comodo.com/e-commerce/code-signing/code-signing-certificate.php)
- [Certum](https://www.certum.eu/certum/cert,offer_en_open_source_cs.xml) 

The complete list of certification authorities trusted by Windows can be obtained from [http://aka.ms/trustcertpartners](http://aka.ms/trustcertpartners).

## Create a test certificate

You can use self-issued certificates for testing purposes. To create a self-issued certificate, use the [New-SelfSignedCertificate](https://docs.microsoft.com/en-us/powershell/module/pkiclient/new-selfsignedcertificate) PowerShell command.

```ps
New-SelfSignedCertificate -Subject "CN=NuGet Test Developer, OU=Use for testing purposes ONLY" `
                          -FriendlyName "NuGetTestDeveloper" `
                          -Type CodeSigning `
                          -KeyUsage DigitalSignature `
                          -KeyLength 2048 `
                          -KeyAlgorithm RSA `
                          -HashAlgorithm SHA256 `
                          -Provider "Microsoft Enhanced RSA and AES Cryptographic Provider" `
                          -CertStoreLocation "Cert:\CurrentUser\My" 
```

This command creates a testing certificate available in the current user's personal certificate store. You can open the certificate store by running `certmgr.msc` to see the newly created certificate.

## Timestamp requirements

Signed packages should include an RFC 3161 timestamp to ensure signature validity beyond the package signing certificate's validity period. The certificate used to sign the timestamp must be valid for the `id-kp-timeStamping` purpose [[RFC 5280 section 4.2.1.12](https://tools.ietf.org/html/rfc5280#section-4.2.1.12)]. Additionally, the certificate must have an RSA public key length of 2048 bits or higher.

Additional technical details can be found in the [package signature technical specs](https://github.com/NuGet/Home/wiki/Package-Signatures-Technical-Details) (GitHub).
