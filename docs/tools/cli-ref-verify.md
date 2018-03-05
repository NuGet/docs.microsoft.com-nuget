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
nuget verify -Signatures .\..\MyPackage.nupkg -CertificateFingerprint "CE40881FF5F0AD3E58965DA20A9F571EF1651A56933748E1BF1C99E537C4E039;5F874AAF47BCB268A19357364E7FBB09D6BF9E8A93E1229909AC5CAC865802E2" -Verbosity detailed

nuget verify -Signatures c:\foo\MyPackage.nupkg -CertificateFingerprint CE40881FF5F0AD3E58965DA20A9F571EF1651A56933748E1BF1C99E537C4E039

nuget verify -Signatures MyPackage.nupkg -Verbosity quiet

nuget verify -Signatures .\*.nupkg
```