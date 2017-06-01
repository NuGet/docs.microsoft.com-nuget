---
# required metadata

title: NuGet Install-Package PowerShell Reference | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 6/1/2017
ms.topic: article
ms.prod: nuget
#ms.service:
ms.technology: null
ms.assetid: 879db0ef-6b72-4a4a-bb68-f9e3a00f64b8

# optional metadata

description: Reference for Install-Package PowerShell command in the NuGet Package Manager Console in Visual Studio.
keywords: NuGet package manager console, NuGet Powershell commands, NuGet Powershell reference, Install-Package
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

# Install-Package

Installs a package and its dependencies into the project.

## Usage

```ps
Install-Package [-Id] <string> [-Version <string>] [-IgnoreDependencies] [-ProjectName <string>]  [-Source <string>] [-IncludePrerelease] [-FileConflictAction] [-DependencyVersion <dependencyVersion>] [-WhatIf]
```

In NuGet 2.8+, `Install-Package` can downgrade an existing package in your project. For example, if you have Microsoft.AspNet.MVC 5.1.0-rc1 installed, the following command would downgrade it to 5.0.0:

```ps
Install-Package Microsoft.AspNet.MVC -Version 5.0.0.
```

NuGet 2.7 and earlier will give an error saying that a newer version is already installed.
  
## Parameters

|  Parameter   | Description    |
| --- | --- |
Id | Specifies the package ID of the package to install. With NuGet 3.0+, the ID can be a path or URL of a `packages.config` file or a `.nupkg` file. The -Id switch itself is optional.
Version | Specifies the version of the package to install, defaulting to the latest version.
IgnoreDependencies | Installs only this package and not its dependencies.
ProjectName | Specifies the project into which to install the package, defaulting to the default project.
Source | Specifies the URL or path to a package source. If omitted, defaults to the currently selected package source.
IncludePrerelease | Indicates that prerelease packages can be installed.
FileConflictAction | Specifies the action to take when asked to overwrite or ignore existing files referenced by the project. Possible values are *Overwrite, Ignore, None, OverwriteAll*, and *IgnoreAll* (3.0+).
DependencyVersion | Specifies the version of the dependency packages to use, which can be one of the following:
| | - Lowest (default): the lowest version
| | - HighestPatch: the version with the lowest major, lowest minor, highest patch
| | - HighestMinor: the version with the lowest major, highest minor, highest patch
| | - Highest: the highest version
| | You can set the default value using the [`dependencyVersion`](../Schema/nuget-config-file.md#config-section) setting in the Nuget.Config file.
WhatIf | Shows what would happen when running the command without actually performing the install.

## Common Parameters

`Install-Package` supports the following [common PowerShell parameters](http://go.microsoft.com/fwlink/?LinkID=113216):

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
Install-package https://raw.githubusercontent.com/json-ld.net/master/src/JsonLD/packages.config

Install-package c:\temp\packages.config

Install-package https://az320820.vo.msecnd.net/packages/microsoft.aspnet.mvc.5.2.3.nupkg

Install-package c:\temp\packages\jQuery.1.10.2.nupkg

# Installs the latest version of Elmah from the current source
Install-Package Elmah

# Installs Glimpse 1.0.0 into the MvcApplication1 project
Install-Package Glimpse -Version 1.0.0 -Project MvcApplication1

# Installs Ninject.Mvc3 but not its dependencies from c:\temp\packages
Install-Package Ninject.Mvc3 -IgnoreDependencies -Source c:\temp\packages
```
