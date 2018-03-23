---
title: NuGet CLI delete command | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 01/18/2018
ms.topic: reference
ms.prod: nuget
ms.technology: null
description: Reference for the nuget.exe delete command
keywords: nuget delete reference, delete package command
ms.reviewer:
- karann-msft
- unniravindranathan
ms.workload: 
 - "dotnet"
 - "aspnet"
---

# delete command (NuGet CLI)

**Applies to:** package publishing &bullet; **Supported versions:** all

Deletes or unlists a package from a package source. For nuget.org, the delete command [unlists the package](../policies/deleting-packages.md).

## Usage

```cli
nuget delete <packageID> <packageVersion> [options]
```

where `<packageID>` and `<packageVersion>` identify the exact package to delete or unlist. The exact behavior depends on the source. For local folders, for instance, the package is deleted; for nuget.org the package is unlisted.

## Options

| Option | Description |
| --- | --- |
| ApiKey | The API key for the target repository. If not present, the one specified in the config file is used. |
| ConfigFile | The NuGet configuration file to apply. If not specified, `%AppData%\NuGet\NuGet.Config` (Windows) or `~/.nuget/NuGet/NuGet.Config` (Mac/Linux) is used.|
| ForceEnglishOutput | *(3.5+)* Forces nuget.exe to run using an invariant, English-based culture. |
| Help | Displays help information for the command. |
| NonInteractive | Suppresses prompts for user input or confirmations. |
| Source | Specifies the server URL. The URL for nuget.org is `https://api.nuget.org/v3/index.json`. For private feeds, substitute the host name, for example, *%hostname%/api/v3*. |
| Verbosity | Specifies the amount of detail displayed in the output: *normal*, *quiet*, *detailed*. |

Also see [Environment variables](cli-ref-environment-variables.md)

## Examples

```cli
nuget delete MyPackage 1.0

nuget delete MyPackage 1.0 -Source http://package.contoso.com/source -apikey A1B2C3
```
