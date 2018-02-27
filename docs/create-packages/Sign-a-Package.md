---
title: How to Sign a NuGet Package | Microsoft Docs
author: rido-min
ms.author: rido-min
manager: unniravindranathan
ms.date: 02/26/2018
ms.topic: article
ms.prod: nuget
ms.technology: null
description: Explains how signed packages can be used to enable content integrity verification
keywords: NuGet package signing, NuGet security, creating signed packages
ms.reviewer:
- karann-msft
- anangaur
---

# How to create a signed NuGet Package

Signing a package is a simple process that will ensure the package has not been modified since creation to consumption.

## Pre-requisites

1. The .nupkg to sign. See [Creating a package](creating-a-package.md).
2. NuGet.exe CLI tools. See how to [Install NuGet CLI](../install-nuget-client-tools.md#nugetexe-cli). *Singing is available from version 4.6 or later*.
3. Get a code signing certificate. These are certificates issued for that purpose. For packages published to NuGet.org the certificate must be issued by a trusted certificate authority [http://aka.ms/trustcertpartners](http://aka.ms/trustcertpartners). For private package consumption you can use any other certificate as long it is trusted in the consumer machine.  


> [!Warning] 
> NuGet.org does not accept signed packages yet. You can use signed packages for packages distributed in private feeds.


## How to sign a package

Using `nuget.exe` you can use the [sign command](../tools/cli-ref-sign) to sign a package:

```
nuget sign <MyPackage.nupkg> -CertificateSubjectName <MyCertSubjectName> -Timestamper <TimestampServiceURL>
```

As described in the command reference you can use a certificate available in the certificate store or use a certificate from a file.

### Common problems signing a package

There are some cases where the sign operation can fail, here are the most common cases:
- The certificate is not valid for code signing. You must ensure the certificate specified has the appropiate extended key usage (EKU=1.3.6.1.5.5.7.3.3).
- The certificate does not satisfy the basic requirements such as the 2048 minimum key length, or the SHA256 hash signature.
- The certificate has expired or has been revoked.
- The timestamp server does not satisfy the certificate requirements.

> [!Note]
> Signed packages should include a timestamp to make sure the signature will remain valid when the signed certificate has expired. The sign operation will produce a warning when signing without a timestamp.


## How to verify a signed package

You can use the `nuget.exe` [verify command](../tools/cli-ref-verify) to see the signature details of a given package.

```
nuget verify -signature <MyPackage.nupkg>
```

## How to install a signed package

Signed packages don't require any specific action to be installed, however if the content has been modified since it was signed, the installation will be blocked and will produce an error message.

> [!Warning]
> Packages signed with untrusted certificates will be considered as unsigned, and will be installed without any warning or issue as any other unsigned package.


