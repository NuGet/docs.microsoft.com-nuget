---
# required metadata

title: NuGet Uninstall-Package PowerShell Reference | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 6/1/2017
ms.topic: article
ms.prod: nuget
#ms.service:
ms.technology: null
ms.assetid: f4f5dc79-8e8e-4012-8986-873a5d9283d9

# optional metadata

description: Reference for Uninstall-Package PowerShell command in the NuGet Package Manager Console in Visual Studio.
keywords: NuGet package manager console, NuGet Powershell commands, NuGet Powershell reference, Uninstall-Package
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

# Uninstall-Package

Removes a package from a project, optionally removing its dependencies.

## Usage

```ps
Uninstall-Package [-Id] <string> [-Version <string>] [-RemoveDependencies] [-ProjectName <string>] [-Force] [-WhatIf]
```

If other packages depend on this package, the command will fail unless the –Force option is specified.


## Parameters

|  Parameter   | Description    |
| --- | --- |
Id | Specifies the identifier of the package to uninstall. The -Id switch itself is optional.
Version | Specifies the version of the package to uninstall, defaulting to the currently installed version.
RemoveDependencies | Uninstalls the package and its unused dependencies. That is, if any dependency has another package that depends on it, it is skipped.
ProjectName | Specifies the project from which to uninstall the package, defaulting to the default project.
Force | Forces a package to be uninstalled, even if there are dependencies on it.
WhatIf | Shows what would happen when running the command without actually performing the uninstall.

## Common Parameters

`Uninstall-Package` supports the following [common PowerShell parameters](http://go.microsoft.com/fwlink/?LinkID=113216):

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
# Uninstalls the Elmah package from the default project
Uninstall-Package Elmah

# Uninstalls the Elmah package and all its unused dependencies
Uninstall-Package Elmah -RemoveDependencies 

# Uninstalls the Elmah package even if another package depends on it.
Uninstall-Package Elmah -Force
```
