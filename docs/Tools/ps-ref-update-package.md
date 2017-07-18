---
# required metadata

title: NuGet Update-Package PowerShell Reference | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 7/17/2017
ms.topic: article
ms.prod: nuget
#ms.service:
ms.technology: null
ms.assetid: 90f4dcb0-6e5a-4948-8ea9-62e0d061d95a

# optional metadata

description: Reference for Update-Package PowerShell command in the NuGet Package Manager Console in Visual Studio.
keywords: NuGet package manager console, NuGet Powershell commands, NuGet Powershell reference, Update-Package
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

# Update-Package

Updates a package and its dependencies, or all packages in a project, to a newer version.

## Syntax

```ps
Update-Package [-Id] <string> [-IgnoreDependencies] [-ProjectName <string>] [-Version <string>]
    [-Safe] [-Source <string>] [-IncludePrerelease] [-Reinstall] [-FileConflictAction]
    [-DependencyVersion] [-ToHighestPatch] [-ToHighestMinor] [-WhatIf] [<CommonParameters>]
```

In NuGet 2.8+, `Update-Package` can be used to downgrade an existing package in your project. For example, if you have Microsoft.AspNet.MVC 5.1.0-rc1 installed, the following command would downgrade it to 5.0.0:

```ps
Update-Package Microsoft.AspNet.MVC -Version 5.0.0.
```

NuGet 2.7 and earlier gives an error saying that a newer version is already installed.

## Parameters

|  Parameter | Description |
| --- | --- |
| Id | The identifier of the package to update. If omitted, updates all packages. The -Id switch itself is optional. |
| IgnoreDependencies | Skips updating the package's dependencies. |
| ProjectName | The name of the project containing the packages to update, defaulting to all projects. |
| Version | The version to use for the upgrade, defaulting to the latest version. In NuGet 3.0+, the version value must be one of *Lowest, Highest, HighestMinor*, or *HighestPatch* (equivalent to -Safe). |
| Safe | Constrains upgrades to only versions with the same Major and Minor version as the currently installed package. |
| Source | The URL or folder path for the package source to search. Local folder paths can be absolute, or relative to the current folder. If omitted, `Uninstall-Package` searches the currently selected package source. |
| IncludePrerelease | Includes prerelease packages for updates. |
| Reinstall | Resintalls packages using their currently installed versions. See [Reinstalling and updating packages](../consume-packages/reinstalling-and-updating-packages.md). |
| FileConflictAction | The action to take when asked to overwrite or ignore existing files referenced by the project. Possible values are *Overwrite, Ignore, None, OverwriteAll*, and *IgnoreAll* (3.0+). |
| DependencyVersion | The version of the dependency packages to use, which can be one of the following:<br/><ul><li>*Lowest* (default): the lowest version</li><li>*HighestPatch*: the version with the lowest major, lowest minor, highest patch</li><li>*HighestMinor*: the version with the lowest major, highest minor, highest patch</li><li>*Highest* (default for Update-Package with no parameters): the highest version</li></ul>You can set the default value using the [`dependencyVersion`](../Schema/nuget-config-file.md#config-section) setting in the `Nuget.Config` file. |
| ToHighestPatch | Constrains upgrades to only versions with the same Minor version as the currently installed package. |
| ToHighestMinor | Constrains upgrades to only versions with the same Major version as the currently installed package. |
| WhatIf | Shows what would happen when running the command without actually performing the update. |

None of these parameters accept pipeline input or wildcard characters.

### Common Parameters

`Update-Package` supports the following [common PowerShell parameters](http://go.microsoft.com/fwlink/?LinkID=113216): Debug, Error Action, ErrorVariable, OutBuffer, OutVariable, PipelineVariable, Verbose, WarningAction, and WarningVariable.

### Examples

```ps
# Updates all packages in every project of the solution
Update-Package

# Updates every package in the MvcApplication1 project
Update-Package -ProjectName MvcApplication1

# Updates the Elmah package in every project to the latest version
Update-Package Elmah

# Updates the Elmah package to version 1.1.0 in every project showing optional -Id usage
Update-Package -Id Elmah -Version 1.1.0

# Updates the Elmah package within the MvcApplication1 project to the highest "safe" version.
# For example, if Elmah version 1.0.0 of a package is installed, and versions 1.0.1, 1.0.2,
# and 1.1 are available in the feed, the -Safe parameter updates the package to 1.0.2 instead
# of 1.1 as it would otherwise.
Update-Package Elmah -Project MvcApplication1 -Safe
```