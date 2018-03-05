---
title: NuGet CLI verify command | Microsoft Docs
author: dtivel
ms.author: dtivel
manager: doronm
ms.date: 03/02/2018
ms.topic: reference
ms.prod: nuget
ms.technology: null
description: Reference for the nuget.exe verify command
keywords: nuget verify reference, verify command
ms.reviewer:
- karann
- rmpablos
---

# verify command (NuGet CLI)

**Applies to:** package consumption &bullet; **Supported versions:** 4.6+

Verifies a package.

## Usage 

```cli
nuget verify [options] <package(s)>
```

## Options

| Option | Description |
| --- | --- |
| All | Specifies that all verifications possible should be performed on the package(s). |
| CertificateFingerprint | Specifies one or more SHA-256 certificate fingerprints of certificates(s) which signed packages must be signed with. A certificate SHA-256 fingerprint is a SHA-256 hash of the certificate. Multiple inputs should be semicolon separated. |
| ConfigFile | The NuGet configuration file to apply. If not specified, *%AppData%\NuGet\NuGet.Config* is used. |
| ForceEnglishOutput | *(3.5+)* Forces nuget.exe to run using an invariant, English-based culture. |
| Help | Displays help information for the command. |
| NonInteractive | Suppresses prompts for user input or confirmations. |
| Signatures | Specifies that package signature verification should be performed. |
| Verbosity | Specifies the amount of detail displayed in the output: *normal*, *quiet*, *detailed*. |


## Examples

```cli
nuget verify -Signatures .\..\MyPackage.nupkg -CertificateFingerprint 86C0E8C51FF0EBDAD355315DF82AA6EC7049B542;BE36A4562FB2EE05DBB3D32323ADF445084ED656" -Verbosity detailed

nuget verify -Signatures c:\foo\MyPackage.nupkg -CertificateFingerprint BE36A4562FB2EE05DBB3D32323ADF445084ED656

nuget verify -Signatures MyPackage.nupkg -Verbosity quiet

nuget verify -Signatures .\*.nupkg
```