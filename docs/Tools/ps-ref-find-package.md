---
# required metadata

title: NuGet Find-Package PowerShell Reference | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 6/1/2017
ms.topic: article
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

Searches a package source using a package ID or keywords.

## Usage

```ps
Find-Package [<keywords>] [-Source <string>] [-First <int>] [-Skip <int>] [-AllVersions] [-IncludePrerelease] [-ExactMatch]
```

## Parameters

|  Parameter   | Description    |
| --- | --- |
&lt;keywords&gt; | Searches the package source for packages with the keywords. Use the `-ExactMatch` switch to return only those packages whose package ID matches the keywords. If no keywords are given, Find-Package returns a list of the top 20 packages by downloads.
Source | Specifies the URL or path for the package source to search. If omitted, searched the currently selected package source.
First | Specifies the number of packages to return from the beginning of the list. If not specified, Find-Package lists 20 packages.
Skip | Skips the specified number of packages, counting from the beginning of the list
AllVersions | Displays all available versions of each package instead of only the latest version.
IncludePrerelease | Indicates whether to include prerelease packages in the list.
ExactMatch | Specified to use the keywords as a case-sensitive package ID.

## Common Parameters

`Find-Package` supports the following [common PowerShell parameters](http://go.microsoft.com/fwlink/?LinkID=113216):

- Debug
- ErrorAction
- ErrorVariable
- OutBuffer
- OutVariable
- PipelineVariable
- Verbose
- WarningAction
- WarningVariable

## Examples

```ps
# List packages with the keyword Elmah available
Find-Package Elmah

# List all versions of the jquery package
Find-Package jquery -AllVersions -ExactMatch

# List packages with the keyword EntityFramework and version 6.1.1
Find-Package EntityFramework -version 6.1.1
```

