---
title: Signing NuGet Packages
description: Explains how signed packages can be used to enable content integrity verification.
author: rido-min
ms.author: rmpablos
ms.date: 03/06/2018
ms.topic: conceptual
ms.reviewer: anangaur
---

# Signing NuGet Packages

Signed packages allows for content integrity verification checks which provides protection against content tampering. The package signature also serves as the single source of truth about the actual origin of the package and bolsters package authenticity for the consumer. This guide assumes you have already [created a package](creating-a-package.md).

## Get a code signing certificate

Valid certificates may be obtained from a public certificate authority like such as [Symantec](https://trustcenter.websecurity.symantec.com/process/trust/productOptions?productType=SoftwareValidationClass3), [DigiCert](https://www.digicert.com/code-signing/), [Go Daddy](https://www.godaddy.com/web-security/code-signing-certificate), [Global Sign](https://www.globalsign.com/en/code-signing-certificate/), [Comodo](https://www.comodo.com/e-commerce/code-signing/code-signing-certificate.php), [Certum](https://www.certum.eu/certum/cert,offer_en_open_source_cs.xml), etc. The complete list of certification authorities trusted by Windows can be obtained from [http://aka.ms/trustcertpartners](http://aka.ms/trustcertpartners).

You can use self-issued certificates for testing purposes. However, packages signed using self-issued certificates are not accepted by NuGet.org. Learn more about [creating a test certificate]()

1. nuget.exe 4.6.0 or later. See how to [Install NuGet CLI](../install-nuget-client-tools.md#nugetexe-cli).

1. [A code signing certificate](../reference/signed-packages-reference.md#get-a-code-signing-certificate).

## Sign a package

To sign a package, use [nuget sign](../tools/cli-ref-sign.md):

```cli
nuget sign MyPackage.nupkg -CertificateSubjectName <MyCertSubjectName> -Timestamper <TimestampServiceURL>
```

As described in the command reference, you can use a certificate available in the certificate store or use a certificate from a file.

### Common problems when signing a package

- The certificate is not valid for code signing. You must ensure the certificate specified has the appropriate extended key usage (EKU 1.3.6.1.5.5.7.3.3).
- The certificate does not satisfy the basic requirements such as the RSA SHA-256 signature algorithm or a public key 2048 bits or greater.
- The certificate has expired or has been revoked.
- The timestamp server does not satisfy the certificate requirements.

> [!Note]
> Signed packages should include a timestamp to make sure the signature remains valid when the signing certificate has expired. The sign operation produce a [warning NU3002](../reference/errors-and-warnings/NU3002.md) when signing without a timestamp.

## Verify a signed package

Use [nuget verify](../tools/cli-ref-verify.md) to see the signature details of a given package:

```cli
nuget verify -signature MyPackage.nupkg
```

## Install a signed package

Signed packages don't require any specific action to be installed; however, if the content has been modified since it was signed, the installation is blocked and produces an [error NU3008](../reference/errors-and-warnings/NU3008.md).

> [!Warning]
> Packages signed with untrusted certificates are considered as unsigned and are installed without any warnings or errors like any other unsigned package.

## Create a test certificate

You can use self-issued certificates for testing purposes. To create a self-issued certificate, use the [New-SelfSignedCertificate PowerShell command](/powershell/module/pkiclient/new-selfsignedcertificate.md).

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

> [!Warning]
> nuget.org does not accept packages signed with self-issued certificates.

## See also

[Signed Packages Reference](../reference/Signed-Packages-Reference.md)
