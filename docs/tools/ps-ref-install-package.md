---
title: NuGet Install-Package PowerShell Reference | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 06/01/2017
ms.topic: reference
ms.prod: nuget
ms.technology: null
ms.assetid: 879db0ef-6b72-4a4a-bb68-f9e3a00f64b8
description: Reference for Install-Package PowerShell command in the NuGet Package Manager Console in Visual Studio.
keywords: NuGet package manager console, NuGet Powershell commands, NuGet Powershell reference, Install-Package
ms.reviewer:
- karann-msft
- unniravindranathan
---

# Install-Package (Package Manager Console in Visual Studio)

*This topic describes the command within the [NuGet Package Manager Console](package-manager-console.md) in Visual Studio on Windows. For the generic PowerShell Install-Package command, see the [PowerShell PackageManagement reference](/powershell/module/packagemanagement/?view=powershell-6).*

Installs a package and its dependencies into a project.

## Syntax

```ps
Install-Package [-Id] <string> [-IgnoreDependencies] [-ProjectName <string>] [[-Source] <string>] 
    [[-Version] <string>] [-IncludePrerelease] [-FileConflictAction] [-DependencyVersion]
    [-WhatIf] [<CommonParameters>]
```

In NuGet 2.8+, `Install-Package` can downgrade an existing package in your project. For example, if you have Microsoft.AspNet.MVC 5.1.0-rc1 installed, the following command would downgrade it to 5.0.0:

```ps
Install-Package Microsoft.AspNet.MVC -Version 5.0.0.
```

## Parameters

| Parameter | Description |
| --- | --- |
| Id | (Required) The identifier of the package to install. (*3.0+*) The identifier can be a path or URL of a `packages.config` file or a `.nupkg` file. The -Id switch itself is optional. |
| IgnoreDependencies | Install only this package and not its dependencies. |
| ProjectName | The project into which to install the package, defaulting to the default project. |
| Source | The URL or folder path for the package source to search. Local folder paths can be absolute, or relative to the current folder. If omitted, `Install-Package` searches the currently selected package source. |
| Version | The version of the package to install, defaulting to the latest version. |
| IncludePrerelease | Considers prerelease packages for the install. If omitted, only stable packages are considered. |
| FileConflictAction | The action to take when asked to overwrite or ignore existing files referenced by the project. Possible values are *Overwrite, Ignore, None, OverwriteAll*, and *(3.0+)* *IgnoreAll*. |
| DependencyVersion | The version of the dependency packages to use, which can be one of the following:<br/><ul><li>*Lowest* (default): the lowest version</li><li>*HighestPatch*: the version with the lowest major, lowest minor, highest patch</li><li>*HighestMinor*: the version with the lowest major, highest minor, highest patch</li><li>*Highest* (default for Update-Package with no parameters): the highest version</li></ul>You can set the default value using the [`dependencyVersion`](../reference/nuget-config-file.md#config-section) setting in the `Nuget.Config` file. |
| WhatIf | Shows what would happen when running the command without actually performing the install. |

None of these parameters accept pipeline input or wildcard characters.

## Common Parameters

`Install-Package` supports the following [common PowerShell parameters](http://go.microsoft.com/fwlink/?LinkID=113216): Debug, Error Action, ErrorVariable, OutBuffer, OutVariable, PipelineVariable, Verbose, WarningAction, and WarningVariable.

## Examples

```ps
# Installs the latest version of Elmah from the current source into the default project
Install-Package Elmah

# Installs Glimpse 1.0.0 into the MvcApplication1 project
Install-Package Glimpse -Version 1.0.0 -Project MvcApplication1

# Installs Ninject.Mvc3 but not its dependencies from c:\temp\packages
Install-Package Ninject.Mvc3 -IgnoreDependencies -Source c:\temp\packages

# Installs the package listed on the online packages.config into the current project
Install-package https://raw.githubusercontent.com/json-ld.net/master/src/JsonLD/packages.config

# Installs jquery 1.10.2 package, using the .nupkg file under local path of c:\temp\packages
Install-package c:\temp\packages\jQuery.1.10.2.nupkg

# Installs the specific online package
Install-package https://az320820.vo.msecnd.net/packages/microsoft.aspnet.mvc.5.2.3.nupkg
```
