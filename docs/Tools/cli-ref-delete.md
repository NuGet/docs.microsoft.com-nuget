---
# required metadata

title: NuGet CLI delete command | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 10/24/2017
ms.topic: reference
ms.prod: nuget
ms.technology: null
ms.assetid: c213a07a-711d-47e0-9ee6-1d582e6cdb69

# optional metadata

description: Reference for the nuget.exe delete command
keywords: nuget delete reference, delete package command
ms.reviewer:
- karann-msft
- unniravindranathan

---

# delete command (NuGet CLI)

**Applicable roles**: Package publishing
**Supported versions**: All

Deletes or unlists a package from a package source. For nuget.org, the delete command [unlists the package](../policies/Deleting-Packages.md).

## Usage

```
nuget delete <packageID> <packageVersion> [options]
```

where `<packageID>` and `<packageVersion>` identify the exact package to delete or unlist. The exact behavior depends on the source. For local folders, for instance, the package is deleted; for nuget.org the package is unlisted.

## Options

| Option | Description |
| --- | --- |
| ApiKey | The API key for the target repository. If not present, the one specified in *%AppData%\NuGet\NuGet.Config* is used. |
| ConfigFile | *(2.5+)* The NuGet configuration file to apply. If not specified, *%AppData%\NuGet\NuGet.Config* is used. |
| ForceEnglishOutput | *(3.5+)* Forces nuget.exe to run using an invariant, English-based culture. |
| Help | Displays help information for the command. |
| NonInteractive | Suppresses prompts for user input or confirmations. |
| Source | Specifies the server URL. Supported URLs for nuget.org include `https://www.nuget.org`, `https://www.nuget.org/api/v3`, `https://www.nuget.org/api/v2/package`. For private feeds, substitute the host name, for example, *%hostname%/api/v3*. |
| Verbosity | Specifies the amount of detail displayed in the output: *normal*, *quiet*, *detailed (2.5+)*. |

Also see [Environment variables](cli-ref-environment-variables.md)

## Examples

```
nuget delete MyPackage 1.0

nuget delete MyPackage 1.0 -Source http://package.contoso.com/source -apikey A1B2C3
```
