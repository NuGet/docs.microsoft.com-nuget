---
title: Signing NuGet Packages
description: Explains how signed packages can be used to enable content integrity verification.
author: rido-min
ms.author: rmpablos
manager: unnir
ms.date: 03/06/2018
ms.topic: conceptual
ms.reviewer: anangaur
---

# Signing NuGet Packages

Signing a package is a process that makes sure the package has not been modified since its creation.

## Prerequisites

1. The package (a `.nupkg` file) to sign. See [Creating a package](creating-a-package.md).

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

Signed packages don't require any specific action to be installed; however, if the content has been modified since it was signed, the installation be blocked and produces a [error NU3008](../reference/errors-and-warnings/NU3008.md).

> [!Warning]
> Packages signed with untrusted certificates are considered as unsigned and are installed without any warnings or errors like any other unsigned package.

## See also

[Signed Packages Reference](../reference/Signed-Packages-Reference.md)
