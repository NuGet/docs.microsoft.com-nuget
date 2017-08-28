---
# required metadata

title: NuGet Find-Package PowerShell Reference | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 6/1/2017
ms.topic: reference
ms.prod: nuget
#ms.service:
ms.technology: null
ms.assetid: 2f7b8847-8259-4366-98c0-13cab88d6e1b

# optional metadata

description: Reference for Find-Package PowerShell command in the NuGet Package Manager Console in Visual Studio.
keywords: NuGet package manager console, NuGet Powershell commands, NuGet Powershell reference, Find-Package
#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer:
- karann
- unnir
#ms.suite:
#ms.tgt_pltfrm:
#ms.custom:

---

# Find-Package

*Version 3.0+*

Gets the set of remote packages with specified ID or keywords from the package source.

## Syntax

```ps
Find-Package [-Id] <keywords> -Source <string> [-AllVersions] [-First [<int>]]
    [-Skip <int>] [-IncludePrerelease] [-ExactMatch] [-StartWith] [<CommonParameters>]
```

## Parameters

| Parameter | Description |
| --- | --- |
| Id &lt;keywords&gt; | (Required) Keywords to use when searching the package source. Use -ExactMatch to return only those packages whose package ID matches the keywords. If no keywords are given, `Find-Package` returns a list of the top 20 packages by downloads, or the number specified by -First. Note that -Id is optional and a no-op. |
| Source | The URL or folder path for the package source to search. Local folder paths can be absolute, or relative to the current folder. If omitted, `Find-Package` searches the currently selected package source. |
| AllVersions | Displays all available versions of each package instead of only the latest version. |
| First | The number of packages to return from the beginning of the list; the default is 20. |
| Skip | Omits the first &lt;int&gt; packages from the displayed list.  |
| IncludePrerelease | Includes prerelease packages in the results. |
| ExactMatch | Specified to use &lt;keywords&gt; as a case-sensitive package ID. |
| StartWith | Returns packages whose package ID begins with &lt;keywords&gt;. |

None of these parameters accept pipeline input or wildcard characters.

## Common Parameters

`Find-Package` supports the following [common PowerShell parameters](http://go.microsoft.com/fwlink/?LinkID=113216): Debug, Error Action, ErrorVariable, OutBuffer, OutVariable, PipelineVariable, Verbose, WarningAction, and WarningVariable.

## Examples

```ps
# List available packages with the keyword Elmah
Find-Package Elmah

# List packages whose ID begins with Elmah
Find-Package Elmah -StartWith

# List all versions of the jquery package
Find-Package jquery -AllVersions -ExactMatch

# List packages with the keyword EntityFramework and version 6.1.1
Find-Package EntityFramework -version 6.1.1
```
