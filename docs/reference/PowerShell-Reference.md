---
title: NuGet PowerShell Reference
description: The complete reference to PowerShell commands available in the NuGet Package Manager Console in Visual Studio.
author: karann-msft
ms.author: karann
ms.date: 10/02/2017
ms.topic: reference
---

# PowerShell reference

The Package Manager Console provides a PowerShell interface within Visual Studio on Windows to interact with NuGet through the specific commands listed below. (The console is not presently available in Visual Studio for Mac.) For a guide to using the console, see [Install and manage packages using Package Manager Console](../consume-packages/install-use-packages-powershell.md) topic.

> [!Tip]
> All PowerShell commands relate only to package consumption. No PowerShell commands relate to creating and publishing packages except to the extent that a package can also be a consumer of other packages.

> [!Important]
> The commands listed here are specific to the Package Manager Console in Visual Studio, and differ from the [Package Management module commands](/powershell/module/packagemanagement/?view=powershell-6) that are available in a general PowerShell environment. Specifically, each environment has commands that are not available in the other, and commands with the same name may also differ in their specific arguments. When using the Package Management Console in Visual Studio, the commands and arguments documented in this present topic apply.

| Common Commands | Description | NuGet Version |
| --- | --- | --- |
| [Install-Package](ps-reference/ps-ref-install-package.md) | Installs a package and its dependencies into the project. | All |
| [Update-Package](ps-reference/ps-ref-update-package.md) | Updates a package and its dependencies, or all packages in a project. | All |
| [Find-Package](ps-reference/ps-ref-find-package.md) | Searches a package source using a package ID or keywords. | 3.0+ |
| [Get-Package](ps-reference/ps-ref-get-package.md) | Retrieves the list of packages installed in the local repository, or lists packages available from a package source. | All |

| Secondary Commands | Description | NuGet Version |
| --- | --- | --- |
| [Add-BindingRedirect](ps-reference/ps-ref-add-bindingredirect.md) | Examines all assemblies within the output path for a project and adds binding redirects to the `app.config` or `web.config` where necessary. | All |
| [Get-Project](ps-reference/ps-ref-get-project.md) | Displays information about the default or specified project. | 3.0+ |
| [Open-PackagePage](ps-reference/ps-ref-open-packagepage.md) | Launches the default browser with the project, license, or report abuse URL for the specified package. | Deprecated in 3.0+ |
| [Register-TabExpansion](ps-reference/ps-ref-register-tabexpansion.md) | Registers a tab expansion for the parameters of a command, allowing you to create customized expansions for commonly-used parameter values. | All |
| [Sync-Package](ps-reference/ps-ref-sync-package.md) | Get the version of installed package from specified project and syncs the version to the rest of projects in the solution. | 3.0+ |
| [Uninstall-Package](ps-reference/ps-ref-uninstall-package.md) | Removes a package from a project, optionally removing its dependencies. | All |

For complete, detailed help on any of these commands within the console, just run the following with the command name in question:

```ps
Get-Help <command> -full
```

All Package Manager Console commands support the following [common PowerShell parameters](https://go.microsoft.com/fwlink/?LinkID=113216):

- Debug
- ErrorAction
- ErrorVariable
- OutBuffer
- OutVariable
- PipelineVariable
- Verbose
- WarningAction
- WarningVariable

For details, refer to [about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216) in the PowerShell documentation.
