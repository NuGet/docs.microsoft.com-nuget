---
title: NuGet CLI search command
description: Reference for the nuget.exe search command
author: JonDouglas
ms.author: jodou
ms.date: 08/17/2020
ms.topic: reference
---

# search command (NuGet CLI)

**Applies to:** package consumption &bullet; **Supported versions:** 5.8+

Searches a given source using the query string provided. If no sources are specified, all sources defined in %AppData%\NuGet\NuGet.Config are used.

## Usage

```cli
nuget search [search terms] [options]
```

where the search terms are applied to the names of packages, tags, and package descriptions just as they are when using them on nuget.org.

## Options

| Name | Description | Usage |
| ---  |     ---     |  :-:  |
| PreRelease | Pre-release packages are not included by default, but can be included by using this argument | -PreRelease |
| Source | Specific package source(s) to search instead of querying the default sources in __nuget.config__ | -Source `<Source URL>`|
| Take | The number of results to return. The default value is 20. | -Take `<positive integer>` |
| Verbosity | The level of detail to display in the output. The default is _normal_. (See the note below)  | -Verbosity `<quiet|normal|detailed>` |
| Help | Displays help information for the command | -Help |

Also see [Environment variables](cli-ref-environment-variables.md)

> [!NOTE] 
> Verbosity Levels:
> * _quiet_ - Package ID, Version
> * _normal_ - Package ID, Version, Downloads, Preview of Description
> * _detailed_ - Package ID, Version, Downloads, Full Description, Other information such as the query URL

## Examples

Search for *logging*-related packages from default sources:
```
nuget search logging
```
Search for *logging*-related packages with detailed verbosity:
```
nuget search logging -Verbosity detailed
```
Search for *logging*-related packages, and only show the top 5 results:
```
nuget search logging -Take 5
```
Search for *JSON*-related packages, including pre-release versions, from specified source/feed:
```
nuget search JSON -PreRelease -Source "https://api.nuget.org/v3/index.json"
```
Search for *JSON*-related packages from multiple sources/feeds:
```
nuget search JSON -Source "https://api.nuget.org/v3/index.json" -Source "https://other-feed-url-goes-here"
```
