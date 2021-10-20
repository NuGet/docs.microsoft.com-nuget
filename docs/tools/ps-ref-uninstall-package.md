---
title: NuGet Uninstall-Package PowerShell Reference | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 06/01/2017
ms.topic: reference
ms.prod: nuget
ms.technology: null
description: Reference for Uninstall-Package PowerShell command in the NuGet Package Manager Console in Visual Studio.
keywords: NuGet package manager console, NuGet Powershell commands, NuGet Powershell reference, Uninstall-Package
ms.reviewer:
- karann-msft
- unniravindranathan
---

# Uninstall-Package (Package Manager Console in Visual Studio)

*This topic describes the command within the [NuGet Package Manager Console](package-manager-console.md) in Visual Studio on Windows. For the generic PowerShell Uninstall-Package command, see the [PowerShell PackageManagement reference](/powershell/module/packagemanagement/?view=powershell-6).*

Removes a package from a project, optionally removing its dependencies. If other packages depend on this package, the command will fail unless the –Force option is specified.

## Syntax

```ps
Uninstall-Package [-Id] <string> [-RemoveDependencies] [-ProjectName <string>] [-Force]
    [-Version <string>] [-WhatIf] [<CommonParameters>]
```

If other packages depend on this package, the command will fail unless the –Force option is specified.

## Parameters

| Parameter | Description |
| --- | --- |
| Id | (Required) The identifier of the package to uninstall. The -Id switch itself is optional. |
| Version | The version of the package to uninstall, defaulting to the currently installed version. |
| RemoveDependencies | Uninstall the package and its unused dependencies. That is, if any dependency has another package that depends on it, it's skipped. |
| ProjectName | The project from which to uninstall the package, defaulting to the default project. |
| Force | Forces a package to be uninstalled, even if other packages depend on it. |
| WhatIf | Shows what would happen when running the command without actually performing the uninstall. |

None of these parameters accept pipeline input or wildcard characters.

## Common Parameters

`Uninstall-Package` supports the following [common PowerShell parameters](/powershell/module/microsoft.powershell.core/about/about_commonparameters): Debug, Error Action, ErrorVariable, OutBuffer, OutVariable, PipelineVariable, Verbose, WarningAction, and WarningVariable.

## Examples

```ps
# Uninstalls the Elmah package from the default project
Uninstall-Package Elmah

# Uninstalls the Elmah package and all its unused dependencies
Uninstall-Package Elmah -RemoveDependencies 

# Uninstalls the Elmah package even if another package depends on it
Uninstall-Package Elmah -Force
```