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

Retrieves the list of packages installed in the local repository, lists packages available from a package source when used with the -ListAvailable switch, or lists available updates when used with the -Update switch.

## Syntax

```ps
Get-Package -Source <string> [-ListAvailable] [-Updates] [-ProjectName <string>]
    [-Filter <string>] [-First <int>] [-Skip <int>] [-AllVersions] [-IncludePrerelease]
    [-PageSize] [<CommonParameters>]
```

With no parameters, `Get-Package` displays the list of packages installed in the default project.

## Parameters

| Parameter | Description |
| --- | --- |
| Source | The URL or folder path for the package . Local folder paths can be absolute, or relative to the current folder. If omitted, `Get-Package` searches the currently selected package source. When used with -ListAvailable, defaults to nuget.org. |
| ListAvailable | Lists packages available from a package source, defaulting to nuget.org. Shows a default of 50 packages unless -PageSize and/or -First are specified. |
| Updates | Lists packages that have an update available from the package source. |
| ProjectName | The project from which to get installed packages. If omitted, returns installed projects for the entire solution. |
| Filter | A filter string used to narrow down the list of packages by applying it to the package ID, description, and tags. |
| First | The number of packages to return from the beginning of the list. If not specified, defaults to 50. |
| Skip | Omits the first &lt;int&gt; packages from the displayed list.  |
| AllVersions | Displays all available versions of each package instead of only the latest version. |
| IncludePrerelease | Includes prerelease packages in the results. |
| PageSize | *(3.0+)* When used with -ListAvailable (required), the number of packages to list before giving a prompt to continue. |

None of these parameters accept pipeline input or wildcard characters.

## Common Parameters

`Get-Package` supports the following [common PowerShell parameters](http://go.microsoft.com/fwlink/?LinkID=113216): Debug, Error Action, ErrorVariable, OutBuffer, OutVariable, PipelineVariable, Verbose, WarningAction, and WarningVariable.

## Examples

```ps
# Lists the packages installed in the current solution
Get-Package

# Lists the packages installed in a project
Get-Package -ProjectName MyProject

# Lists packages available in the current package source
Get-Package -ListAvailable

# Lists 30 packages at a time from the current source, and prompts to continue if more are available
Get-Package -ListAvailable -PageSize 30

# Lists packages with the Ninject keyword in the current source, up to 50
Get-Package -ListAvailable -Filter Ninject

# Lists packages installed in the solution that have available updates
Get-Package -Updates

# Lists packages installed in a specific project that have available updates
Get-Package -Updates -ProjectName MyProject
```

