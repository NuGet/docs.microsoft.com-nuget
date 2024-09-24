---
title: NuGet CLI sign command
description: Reference for the nuget.exe sign command
author: dtivel
ms.author: dtivel
ms.date: 03/06/2018
ms.topic: reference
ms.reviewer: rmpablos
---

# sign command (NuGet CLI)

**Applies to:** package creation &bullet; **Supported versions:** 4.6+

Signs all the packages matching the first argument with a certificate. The certificate with the private key can be obtained from a file or from a certificate installed in a certificate store by providing a subject name or a thumbprint.

> [!Note]
> Package signing is not yet supported in .NET Core, under Mono, or on non-Windows platforms.

## Usage

```cli
nuget sign <package(s)> [options]
```

where `<package(s)>` is one or more `.nupkg` files.

## Options

- **`-CertificateFingerprint`**

  Specifies the SHA-1, SHA-256, SHA-384, or SHA-512 fingerprint of the certificate used to search a local certificate store for the certificate.

  > [!NOTE]
  > Starting with `NuGet.exe 6.12`, a `NU3043` warning is raised when a SHA-1 certificate fingerprint is passed.
  > SHA-1 is considered insecure and should no longer be used.

- **`-CertificatePassword`**

  Specifies the certificate password, if needed. If a certificate is password protected but no password is provided, the command will prompt for a password at run time, unless the `-NonInteractive` option is passed.

- **`-CertificatePath`**

  Specifies the file path to the certificate to be used in signing the package.

- **`-CertificateStoreLocation`**

  Specifies the name of the X.509 certificate store use to search for the certificate. Defaults to "CurrentUser", the X.509 certificate store used by the current user. This option should be used when specifying the certificate via `-CertificateSubjectName` or `-CertificateFingerprint` options.

- **`-CertificateStoreName`**

  Specifies the name of the X.509 certificate store to use to search for the certificate. Defaults to "My", the X.509 certificate store for personal certificates. This option should be used when specifying the certificate via `-CertificateSubjectName` or `-CertificateFingerprint` options.

- **`-CertificateSubjectName`**

  Specifies the subject name of the certificate used to search a local certificate store for the certificate.  The search is a case-insensitive string comparison using the supplied value, which will find all certificates with the subject name containing that string, regardless of other subject values.  The certificate store can be specified by `-CertificateStoreName` and `-CertificateStoreLocation` options.

- **`-ConfigFile`**

  The NuGet configuration file to apply. If not specified, `%AppData%\NuGet\NuGet.Config` (Windows), or `~/.nuget/NuGet/NuGet.Config` or `~/.config/NuGet/NuGet.Config` (Mac/Linux) is used.

- **`-ForceEnglishOutput`**

  Forces nuget.exe to run using an invariant, English-based culture.

- **`-HashAlgorithm`**

  Hash algorithm to be used to sign the package. Defaults to SHA256. Possible values are SHA256, SHA384, and SHA512.

- **`-?|-help`**

  Displays help information for the command.

- **`-NonInteractive`**

  Suppresses prompts for user input or confirmations.

- **`-OutputDirectory`**

  Specifies the directory where the signed package should be saved. By default the original package is overwritten by the signed package.

- **`-Overwrite`**

  Switch to indicate if the current signature should be overwritten. By default the command will fail if the package already has a signature.

- **`-Timestamper`**

  URL to an RFC 3161 timestamping server.

- **`-TimestampHashAlgorithm`**

  Hash algorithm to be used by the RFC 3161 timestamp server. Defaults to SHA256.

- **`-Verbosity [normal|quiet|detailed]`**

  Specifies the amount of detail displayed in the output: `normal` (the default), `quiet`, or `detailed`.

## Examples

```cli
nuget sign MyPackage.nupkg -CertificatePath .\..\certificate.pfx -Timestamper http://timestamp.test

nuget sign .\..\MyPackage.nupkg -CertificateStoreLocation CurrentUser -CertificateStoreName My -CertificateSubjectName 'subject name' -Timestamper http://timestamp.test -OutputDirectory .\..\Signed
```
