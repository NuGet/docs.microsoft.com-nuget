---
title: NuGet Sync-Package PowerShell Reference
description: Reference for Sync-Package PowerShell command in the NuGet Package Manager Console in Visual Studio.
author: karann-msft
ms.author: karann
ms.date: 12/07/2017
ms.topic: reference
---

# Sync-Package (Package Manager Console in Visual Studio)

*Version 3.0+; available only within the [Package Manager Console](../../consume-packages/install-use-packages-powershell.md) in Visual Studio on Windows.*

Gets the version of installed package from specified (or default) project and synchronizes the version to the rest of projects in the solution.

## Syntax

```ps
Sync-Package [-Id] <string> [-IgnoreDependencies] [-ProjectName <string>] [[-Version] <string>]
    [[-Source] <string>] [-IncludePrerelease] [-FileConflictAction] [-DependencyVersion]
    [-WhatIf] [<CommonParameters>]
```

## Parameters

| Parameter | Description |
| --- | --- |
| Id | (Required) The identifier of the package to sync. The -Id switch itself is optional. |
| IgnoreDependencies | Install only this package and not its dependencies. |
| ProjectName | The project to sync the package from, defaulting to the default  project. |
| Version | The version of the package to sync, defaulting to the currently installed version. |
| Source | The URL or folder path for the package source to search. Local folder paths can be absolute, or relative to the current folder. If omitted, `Sync-Package` searches the currently selected package source. |
| IncludePrerelease | Includes prerelease packages in the sync. |
| FileConflictAction | The action to take when asked to overwrite or ignore existing files referenced by the project. Possible values are *Overwrite, Ignore, None, OverwriteAll*, and *(3.0+)* *IgnoreAll*. |
| DependencyVersion | The version of the dependency packages to use, which can be one of the following:<br/><ul><li>*Lowest* (default): the lowest version</li><li>*HighestPatch*: the version with the lowest major, lowest minor, highest patch</li><li>*HighestMinor*: the version with the lowest major, highest minor, highest patch</li><li>*Highest* (default for Update-Package with no parameters): the highest version</li></ul>You can set the default value using the [`dependencyVersion`](../nuget-config-file.md#config-section) setting in the `Nuget.Config` file. |
| WhatIf | Shows what would happen when running the command without actually performing the sync. |

None of these parameters accept pipeline input or wildcard characters.

## Common Parameters

`Sync-Package` supports the following [common PowerShell parameters](https://go.microsoft.com/fwlink/?LinkID=113216): Debug, Error Action, ErrorVariable, OutBuffer, OutVariable, PipelineVariable, Verbose, WarningAction, and WarningVariable.

## Examples

```ps
# Sync the Elmah package installed in the default project into the other projects in the solution
Sync-Package Elmah

# Sync the Elmah package installed in the ClassLibrary1 project into other projects in the solution
Sync-Package Elmah -ProjectName ClassLibrary1

# Sync Microsoft.Aspnet.package but not its dependencies into the other projects in the solution
Sync-Package Microsoft.Aspnet.Mvc -IgnoreDependencies

# Sync jQuery.Validation and install the highest version of jQuery (a dependency) from the package source    
Sync-Package jQuery.Validation -DependencyVersion highest
```
