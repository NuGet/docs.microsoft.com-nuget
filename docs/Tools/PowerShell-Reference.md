---
# required metadata

title: NuGet PowerShell Reference | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 7/25/2017
ms.topic: article
ms.prod: nuget
#ms.service:
ms.technology: null
ms.assetid: cd08b869-44c6-480e-90f7-494a6d08e6d0

# optional metadata

description: The complete reference to PowerShell commands available in the NuGet Package Manager Console in Visual Studio.
keywords: NuGet package manager console, NuGet Powershell commands, NuGet Powershell reference
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

# PowerShell reference

The Package Manager Console provides a PowerShell interface within Visual Studio on Windows to interact with NuGet through the specific commands listed below. (The console is not presently available in Visual Studio for Mac.) For a guide to using the console, see the [Package Manager Console](../tools/package-manager-console.md) topic.

> [!Tip]
> All PowerShell commands relate only to package consumption. No PowerShell commands relate to creating and publishing packages except to the extent that a package can also be a consumer of other packages.

> ![Important]
> The commands listed here are specific to the Package Manager Console in Visual Studio, and differ from the [Package Management module commands](https://msdn.microsoft.com/powershell/reference/6/packagemanagement/packagemanagement) that are available in a general PowerShell environment. Specifically, each environment has commands that are not available in the other, and commands with the same name may also differ in their specific arguments. When using the Package Management Console in Visual Studio, the commands and arguments documented in this present topic apply.

| Command | Description | NuGet Version |
| --- | --- | --- |
| [Add-BindingRedirect](ps-ref-add-bindingredirect.md) | Examines all assemblies within the output path for a project and adds binding redirects to the `app.config` or `web.config` where necessary. | All |
| [Find-Package](ps-ref-find-package.md) | Searches a package source using a package ID or keywords. | 3.0+ |
| [Get-Package](ps-ref-get-package.md) | Retrieves the list of packages installed in the local repository, or lists packages available from a package source. | All |
| [Get-Project](ps-ref-get-project.md) | Displays information about the default or specified project. | 3.0+ |
| [Install-Package](ps-ref-install-package.md) | Installs a package and its dependencies into the project. | All |
| [Open-PackagePage](ps-ref-open-packagepage.md) | Launches the default browser with the project, license, or report abuse URL for the specified package. | Deprecated in 3.0+ |
| [Register-TabExpansion](ps-ref-register-tabexpansion.md) | Registers a tab expansion for the parameters of a command, allowing you to create customized expansions for commonly-used parameter values. | All |
| [Sync-Package](ps-ref-sync-package.md) | Get the version of installed package from specified project and syncs the version to the rest of projects in the solution. | 3.0+ |
| [Uninstall-Package](ps-ref-uninstall-package.md) | Removes a package from a project, optionally removing its dependencies. | All |
| [Update-Package](ps-ref-update-package.md) | Updates a package and its dependencies, or all packages in a project. | All |

For complete, detailed help on any of these commands within the console, just run the following with the command name in question:

```ps
Get-Help <command> -full
```

Note that all Package Manager Console commands support the following [common PowerShell parameters](http://go.microsoft.com/fwlink/?LinkID=113216):

- Debug
- ErrorAction
- ErrorVariable
- OutBuffer
- OutVariable
- PipelineVariable
- Verbose
- WarningAction
- WarningVariable

For details, refer to [about_CommonParameters](http://go.microsoft.com/fwlink/?LinkID=113216) in the PowerShell documentation.
