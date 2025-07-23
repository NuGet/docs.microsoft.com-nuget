---
title: NuGet CLI verify command
description: Reference for the nuget.exe verify command
author: dtivel
ms.author: dtivel
ms.date: 03/06/2018
ms.topic: reference
ms.reviewer: rmpablos
---

# verify command (NuGet CLI)

**Applies to:** package consumption &bullet; **Supported versions:** 4.6+

Verifies a package and display the content hash.

Verification of signed packages is not yet supported under Mono.

## Usage

```cli
nuget verify <-All|-Signatures> <package(s)> [options]
```

where `<package(s)>` is one or more `.nupkg` files.

## nuget verify -All

Specifies that all verifications possible should be performed on the package(s).

## nuget verify -Signatures

Specifies that package signature verification should be performed.

## Options for "verify -Signatures"

- **`-CertificateFingerprint`**

  Specifies one or more SHA-256 certificate fingerprints of certificates(s) which signed packages must be signed with. A certificate SHA-256 fingerprint is a SHA-256 hash of the certificate. Multiple inputs should be semicolon separated.

## Options

- **`-ConfigFile`**

  The NuGet configuration file to apply. If not specified, `%AppData%\NuGet\NuGet.Config` (Windows), or `~/.nuget/NuGet/NuGet.Config` or `~/.config/NuGet/NuGet.Config` (Mac/Linux) is used.

- **`-ForceEnglishOutput`**

  Forces nuget.exe to run using an invariant, English-based culture.

- **`-?|-help`**

  Displays help information for the command.

- **`-NonInteractive`**

  Suppresses prompts for user input or confirmations.

- **`-Verbosity [normal|quiet|detailed]`**

  Specifies the amount of detail displayed in the output: `normal` (the default), `quiet`, or `detailed`.

## Examples

```cli
nuget verify -Signatures .\..\MyPackage.nupkg -CertificateFingerprint "CE40881FF5F0AD3E58965DA20A9F571EF1651A56933748E1BF1C99E537C4E039;5F874AAF47BCB268A19357364E7FBB09D6BF9E8A93E1229909AC5CAC865802E2" -Verbosity detailed

nuget verify -Signatures c:\packages\MyPackage.nupkg -CertificateFingerprint CE40881FF5F0AD3E58965DA20A9F571EF1651A56933748E1BF1C99E537C4E039

nuget verify -Signatures MyPackage.nupkg -Verbosity quiet

nuget verify -Signatures .\*.nupkg

nuget verify -All .\*.nupkg

```