---
title: NuGet CLI trusted-signers command
description: Reference for the nuget.exe trusted-signers command
author: patbel
ms.author: patbel
ms.date: 11/12/2018
ms.topic: reference
ms.reviewer: rmpablos
---

# trusted-signers command (NuGet CLI)

**Applies to:** package consumption &bullet; **Supported versions:** 4.9.1+

Gets or sets trusted signers to the NuGet configuration. For additional usage, see [Common NuGet configurations](../../consume-packages/configuring-nuget-behavior.md). For details on how the nuget.config schema looks like, refer to the [NuGet config file reference](../nuget-config-file.md).

## Usage

```cli
nuget trusted-signers <list|add|remove|sync> [options]
```

if none of `list|add|remove|sync` is specified, the command will default to `list`.

## nuget trusted-signers list

Lists all the trusted signers in the configuration. This option will include all the certificates (with fingerprint and fingerprint algorithm) each signer has. If a certificate has a preceding `[U]`, it means that certificate entry has `allowUntrustedRoot` set as `true`.

Below is an example output from this command:

```cli
$ nuget trusted-signers
Registered trusted signers:


 1.   nuget.org [repository]
      Service Index: https://api.nuget.org/v3/index.json
      Certificate fingerprint(s):
        SHA256 - 0E5F38F57DC1BCC806D8494F4F90FBCEDD988B46760709CBEEC6F4219AA6157D
        SHA256 - 5A2901D6ADA3D18260B9C6DFE2133C95D74B9EEF6AE0E5DC334C8454D1477DF4

 2.   microsoft [author]
      Certificate fingerprint(s):
        SHA256 - 3F9001EA83C560D712C24CF213C3D312CB3BFF51EE89435D3430BD06B5D0EECE
        SHA256 - AA12DA22A49BCE7D5C1AE64CC1F3D892F150DA76140F210ABD2CBFFCA2C18A27

 3.   myUntrustedAuthorSignature [author]
      Certificate fingerprint(s):
        [U] SHA256 - 518F9CF082C0872025EFB2587B6A6AB198208F63EA58DD54D2B9FF6735CA4434
        
```

## nuget trusted-signers add [options]

Adds a trusted signer with the given name to the config. This option has different gestures to add a trusted author or repository.

## Options for add based on a package

```cli
nuget trusted-signers add <package> -Name <name> [options]
```

where `<package>` is one signed `.nupkg` file.

- **`-Author`**

  Specifies that the author signature of the signed package should be trusted.

- **`-AllowUntrustedRoot`**

  Specifies if the certificate for the trusted signer should be allowed to chain to an untrusted root.

- **`-Owners`**

  Semi-colon separated list of trusted owners to further restrict the trust of a repository. Only valid when using the `-Repository` option.

- **`-Repository`**

  Specifies that the repository signature or countersignature of the signed package should be trusted.

Providing both `-Author` and `-Repository` at the same time is not supported.

## Options for add based on a service index

```cli
nuget trusted-signers add -Name <name> [options]
```

_Note_: This option will only add trusted repositories. 

- **`-AllowUntrustedRoot`**

  Specifies if the certificate for the trusted signer should be allowed to chain to an untrusted root.

- **`-Owners`**

  Semi-colon separated list of trusted owners to further restrict the trust of a repository.

- **`-ServiceIndex`**

  Specifies the V3 service index of the repository to be trusted. This repository has to support the repository signatures resource. If not provided, the command will look for a package source with the same `-Name` and get the service index from there.

## Options for add based on the certificate information

```cli
nuget trusted-signers add -Name <name> [options]
```

_Note_: If a trusted signer with the given name already exists, the certificate item will be added to that signer. Otherwise a trusted author will be created with a certificate item from given certificate information.


- **`-AllowUntrustedRoot`**

  Specifies if the certificate for the trusted signer should be allowed to chain to an untrusted root.

- **`-CertificateFingerprint`**

  Specifies a certificate fingerprints of a certificate which signed packages must be signed with. A certificate fingerprint is a hash of the certificate. The hash algorithm used for calculating this hash should be specifies in the `FingerprintAlgorithm` option.

- **`-FingerprintAlgorithm`**

  Specifies the hash algorithm used to calculate the certificate fingerprint. Defaults to `SHA256`. Values supported are `SHA256`, `SHA384` and `SHA512`.

## nuget trusted-signers remove -Name \<name\>

Removes any trusted signers that match the given name.

## nuget trusted-signers sync -Name \<name\>

Requests the latest list of certificates used in a currently trusted repository to update the the existing certificate list in the trusted signer.

_Note_: This gesture will delete the current list of certificates and replace them with an up-to-date list from the repository.

## Options

- **`-ConfigFile`**

  The NuGet configuration file to apply. If not specified, `%AppData%\NuGet\NuGet.Config` (Windows), or `~/.nuget/NuGet/NuGet.Config` or `~/.config/NuGet/NuGet.Config` (Mac/Linux) is used. See [On Mac/Linux, the user-level config file location varies by tooling.](../../consume-packages/configuring-nuget-behavior.md#on-maclinux-the-user-level-config-file-location-varies-by-tooling).

- **`-ForceEnglishOutput`**

  Forces nuget.exe to run using an invariant, English-based culture.

- **`-?|-help`**

  Displays help information for the command.

- **`-Name`**

  Name of the trusted signer.

- **`-NonInteractive`**

  Suppresses prompts for user input or confirmations.

- **`-Verbosity [normal|quiet|detailed]`**

  Specifies the amount of detail displayed in the output: `normal` (the default), `quiet`, or `detailed`.


## Examples

```cli
nuget trusted-signers list

nuget trusted-signers Add -Name existingSource

nuget trusted-signers Add -Name trustedRepo -ServiceIndex https://trustedRepo.test/v3ServiceIndex

nuget trusted-signers Add -Name author1 -CertificateFingerprint CE40881FF5F0AD3E58965DA20A9F571EF1651A56933748E1BF1C99E537C4E039 -FingerprintAlgorithm SHA256

nuget trusted-signers Add -Repository .\..\MyRepositorySignedPackage.nupkg -Name TrustedRepo

nuget trusted-signers Remove -Name TrustedRepo

nuget trusted-signers Sync -Name TrustedRepo
```
