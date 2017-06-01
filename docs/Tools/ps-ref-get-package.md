---
# required metadata

title: NuGet Get-Package PowerShell Reference | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 6/1/2017
ms.topic: article
ms.prod: nuget
#ms.service:
ms.technology: null
ms.assetid: 42476008-64b3-480e-a966-98b2fa38b681

# optional metadata

description: Reference for Get-Package PowerShell command in the NuGet Package Manager Console in Visual Studio.
keywords: NuGet package manager console, NuGet Powershell commands, NuGet Powershell reference, Get-Package
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

# Get-Package

Retrieves the list of packages installed in the local repository, or lists packages available from a package source when used with the `-ListAvailable` switch.

## Usage

```ps
Get-Package [-Source <string>] [-ListAvailable] [-Updates] [-ProjectName <string>] [-Filter <string>] [-First <int>] [-Skip <int>] [-AllVersions] [-IncludePrerelease] [-PageSize <int>]
```

With no parameters, `Get-Package` displays the list of packages installed in the default project.

## Parameters

|  Parameter   | Description    |
| --- | --- |
Source | Specifies the URL or path of a package source. When used with -ListAvailable, defaults to nuget.org.
ListAvailable | Lists packages available from a package source, defaulting to nuget.org. If -PageSize and/or -First are not specified, this will show 50 packages.
Updates | Lists packages that have an update available from the package source.
ProjectName | Specifies the project from which to get installed packages.
Filter | Specifies a filter string used to narrow down the list of packages returned. The filter is searched for in the package ID, the description and tags.
IncludePrerelease | Indicates that prerelease packages should be included in the list.
First | Specifies the number of packages to return from the beginning of the list. If not specified, defaults to 50.
Skip | Skips the specified number of packages, counting from the beginning of the list.
AllVersions | Displays all available versions of each package instead of only the latest version.
PageSize | *(3.0+)* When used with -ListAvailable, specifies the number of packages to list before giving a prompt to continue. The command will continue listing packages as long as there are more to return. Without -PageSize, at most 50 packages are listed by default.

## Common Parameters

`Get-Package` supports the following [common PowerShell parameters](http://go.microsoft.com/fwlink/?LinkID=113216):

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
# Lists the packages installed in the current solution
Get-Package

# Lists packages available in the current package source
Get-Package -ListAvailable

# Lists all packages in the current source in pages of 20
Get-Package -ListAvailable -PageSize 20

# Lists packages with the Ninject keyword in the current source, up to 50
Get-Package -ListAvailable -Filter Ninject

# Lists packages installed in the solution that have available updates
Get-Package -Updates
```

