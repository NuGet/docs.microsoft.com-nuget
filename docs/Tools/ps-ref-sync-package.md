---
# required metadata

title: NuGet Sync-Package PowerShell Reference | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 6/1/2017
ms.topic: article
ms.prod: nuget
#ms.service:
ms.technology: null
ms.assetid: 1b980b93-fa58-430c-b663-78ce069b1603

# optional metadata

description: Reference for Sync-Package PowerShell command in the NuGet Package Manager Console in Visual Studio.
keywords: NuGet package manager console, NuGet Powershell commands, NuGet Powershell reference, Sync-Package
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

# Sync-Package

*Version 3.0+*

Get the version of installed package from specified project and syncs the version to the rest of projects in the solution.

## Usage

```ps
Sync-Package [-Id] <string> [-Version <string>] [-IgnoreDependencies] [-ProjectName <string>] [-Source <string>] [-IncludePrerelease] [-FileConflictAction] [-DependencyVersion <dependencyVersion>] [-WhatIf]
```

## Parameters

|  Parameter   | Description    |
| --- | --- |
Id | Specifies the identifier of the package to sync. The -Id switch itself is optional.
Version | Specifies the version of the package to sync, defaulting to the currently installed version.
ProjectName | Specifies the project to sync the package from, defaulting to the default  project.
Source | Specifies the URL or path of a package source, defaulting to the current package source.
IncludePrerelease | Includes prerelease packages in the sync.
FileConflictAction | Specifies the action to take when asked to overwrite or ignore existing files referenced by the project. Possible values are *Overwrite, Ignore, None, OverwriteAll*, and *IgnoreAll* (3.0+).
DependencyVersion | Specifies the version of the dependency packages to use, which can be one of the following:
| | -Lowest (default): the lowest version
| | -HighestPatch: the version with the lowest major, lowest minor, highest patch
| | -HighestMinor: the version with the lowest major, highest minor, highest patch
| | -Highest: the highest version
| | You can set the default value using the [*dependencyVersion*](../Schema/nuget-config-file.md#config-section) setting in the Nuget.Config file.
WhatIf | Shows what would happen when running the command without actually performing the sync.

## Common Parameters

`Sync-Package` supports the following [common PowerShell parameters](http://go.microsoft.com/fwlink/?LinkID=113216):

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
# Syncs the Ninject package installed in the default project into the other projects in the solution
Sync-Package Ninject

# Syncs only Microsoft.Aspnet.package to the rest of the projects, but not its dependencies
Sync-Package Microsoft.Aspnet.Mvc -IgnoreDependencies

# Syncs jQuery.Validation and installs the highest version of jQuery (a dependency) from the package source    
Sync-Package jQuery.Validation -DependencyVersion highest
```
