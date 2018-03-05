---
title: NuGet CLI sign command | Microsoft Docs
author: dtivel
ms.author: dtivel
manager: doronm
ms.date: 03/02/2018
ms.topic: reference
ms.prod: nuget
ms.technology: null
description: Reference for the nuget.exe sign command
keywords: nuget sign reference, sign command
ms.reviewer:
- karann
- rmpablos
---

# sign command (NuGet CLI)

**Applies to:** package creation &bullet; **Supported versions:** 4.6+

Signs all the packages matching the first argument with a certificate. The certificate with the private key can be obtained from a file or from a certificate installed in a certificate store by providing a subject name or a thumbprint.

Package signing is not yet supported under Mono or on non-Windows platforms.

## Usage 

```cli
nuget sign <package(s)> [options]
```

## Options

| Option | Description |
| --- | --- |
| CertificateFingerprint | Specifies the SHA-1 fingerprint of the certificate used to search a local certificate store for the certificate. |
| CertificatePassword | Specifies the certificate password, if needed. If a certificate is password protected but no password is provided, the command will prompt for a password at run time, unless the -NonInteractive option is passed. |
| CertificatePath | Specifies the file path to the certificate to be used in signing the package. |
| CertificateStoreLocation | Specifies the name of the X.509 certificate store use to search for the certificate. Defaults to "CurrentUser", the X.509 certificate store used by the current user.
This option should be used when specifying the certificate via -CertificateSubjectName or -CertificateFingerprint options. |
| CertificateStoreName | Specifies the name of the X.509 certificate store to use to search for the certificate. Defaults to "My", the X.509 certificate store for personal certificates.
This option should be used when specifying the certificate via -CertificateSubjectName or -CertificateFingerprint options. |
| CertificateSubjectName | Specifies the subject name of the certificate used to search a local certificate store for the certificate.
The search is a case-insensitive string comparison using the supplied value, which will find all certificates with the subject name containing that string, regardless of other subject values.
The certificate store can be specified by -CertificateStoreName and -CertificateStoreLocation options. |
| ConfigFile | The NuGet configuration file to apply. If not specified, *%AppData%\NuGet\NuGet.Config* is used. |
| ForceEnglishOutput | *(3.5+)* Forces nuget.exe to run using an invariant, English-based culture. |
| HashAlgorithm | Hash algorithm to be used to sign the package. Defaults to SHA256. |
| Help | Displays help information for the command. |
| NonInteractive | Suppresses prompts for user input or confirmations. |
| OutputDirectory | Specifies the directory where the signed package should be saved. By default the original package is overwritten by the signed package. |
| Overwrite | Switch to indicate if the current signature should be overwritten. By default the command will fail if the package already has a signature. |
| Timestamper | URL to an RFC 3161 timestamping server. |
| TimestampHashAlgorithm | Hash algorithm to be used by the RFC 3161 timestamp server. Defaults to SHA256. |
| Verbosity | Specifies the amount of detail displayed in the output: *normal*, *quiet*, *detailed*. |


## Examples

```cli
nuget sign MyPackage.nupkg -Timestamper http://timestamp.test

nuget sign .\..\MyPackage.nupkg -Timestamper http://timestamp.test -OutputDirectory .\..\Signed
```