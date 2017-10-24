---
# required metadata

title: NuGet CLI list command | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 10/24/2017
ms.topic: reference
ms.prod: nuget
ms.technology: null
ms.assetid: 728c8452-0457-4bb8-bfdc-de77fe1570bd

# optional metadata

description: Reference for the nuget.exe list command
keywords: nuget list reference, list packages command
ms.reviewer:
- karann-msft
- unniravindranathan

---

# list command (NuGet CLI)

**Applicable roles**: Package consumption, publishing
**Supported versions**: All

Displays a list of packages from a given source. If no sources are specified, all sources defined in the global configuration file, `%AppData%\NuGet\NuGet.Config`, are used. If `NuGet.Config` specifies no sources, then `list` uses the default feed (nuget.org).

## Usage

```
nuget list [search terms] [options]
```

where the optional search terms will filter the displayed list. Search terms are applied to the names of packages, tags, and package descriptions.

## Options
| Option | Description |
| --- | --- |
| AllVersions | List all versions of a package. By default, only the latest package version is displayed. |
| ConfigFile | *(2.5+)* The NuGet configuration file to apply. If not specified, *%AppData%\NuGet\NuGet.Config* is used. |
| ForceEnglishOutput | *(3.5+)* Forces nuget.exe to run using an invariant, English-based culture. |
| Help | Displays help information for the command. |
| IncludeDelisted | *(3.2+)* Display unlisted packages. |
| NonInteractive | Suppresses prompts for user input or confirmations. |
| PreRelease | Includes prerelease packages in the list. |
| Source | Specifies a list of packages sources to search. |
| Verbosity | Specifies the amount of detail displayed in the output: *normal*, *quiet*, *detailed (2.5+)*. |

Also see [Environment variables](cli-ref-environment-variables.md)

## Examples

```
nuget list

nuget list -Verbosity detailed -AllVersions
```
