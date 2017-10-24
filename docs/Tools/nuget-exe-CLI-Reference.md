---
# required metadata

title: NuGet Command-Line Interface (CLI) Reference | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 10/24/2017
ms.topic: reference
ms.prod: nuget
ms.technology: null
ms.assetid: d777c424-0cf3-4bc0-8abd-7ca16c22192b

# optional metadata

description: Command-line reference index for the nuget.exe CLI
keywords: nuget.exe reference index, nuget.exe command-line interface, nuget.exe CLI, nuget command
ms.reviewer:
- karann-msft
- unniravindranathan

---

# NuGet CLI reference

The NuGet Command Line Interface (CLI), `nuget.exe`, provides the full extent of NuGet functionality to install, create, publish, and manage packages without making any changes to project files.

To use any command, open a command window or bash shell, then run `nuget` followed by the command and appropriate options, such as `nuget install elmah` (to install the elmah package).

## Installing nuget.exe

[!INCLUDE[install-cli](../includes/install-cli.md)]

## Availability

- All commands are available on Windows.
- All commands work with [nuget.exe running on Mono](../guides/install-nuget.md#mac-osx-and-linux) except where indicated for `pack`, `restore`, and `update`.
- The `pack`, `restore`, `delete`, `locals`, and `push` commands are also available on Mac and Linux through the [dotnet CLI](dotnet-Commands.md). 

## Commands and applicability

Available commands and applicability to package creation, package consumption, and/or publishing a package to a host:

| Common Commands | Applicable Roles | NuGet Version | Description | 
| --- | --- | --- | --- |
| [pack](cli-ref-pack.md) | Creation | 2.7+ | Creates a NuGet package from a `.nuspec` or project file. When running on Mono, creating a package from a project file is not supported. |
| [push](cli-ref-push.md) | Publishing | All | Publishes a package to a package source. |
| [config](cli-ref-config.md) | All | All | Gets or sets NuGet configuration values. |
| [help or ?](cli-ref-help.md) | All | All | Displays help information or help for a command. |
| [locals](cli-ref-locals.md) | Consumption | 3.3+ | Clears or lists packages in various caches or the global packages folder, or identifies those folders. |
| [restore](cli-ref-restore.md) | Consumption | 2.7+ | Restores all packages referenced by the package reference format in use. When running on Mono, restoring packages using the PackageReference format is not supported. | 
| [setapikey](cli-ref-setapikey.md) | Publishing, Consumption | All | Saves an API key for a given package source when that package source requires a key for access. |
| [spec](cli-ref-spec.md) | Creation | All | Generates a `.nuspec` file, using tokens if generating the file from a Visual Studio project. |


| Secondary Commands | Applicable Roles | NuGet Version | Description | 
| --- | --- | --- | --- |
| [add](cli-ref-add.md) | Publishing | 3.3+ | Adds a package to a non-HTTP package source using hierarchical layout. For HTTP sources, use *push*. |
| [delete](cli-ref-delete.md) | Publishing | All | Removes or unlists a package from a package source. |
| [init](cli-ref-init.md) | Creation | 3.3+ | Adds packages from a folder to a package source using hierarchical layout. |
| [install](cli-ref-install.md) | Consumption | All | Installs a package into the current project but does not modify projects or reference files. |
| [list](cli-ref-list.md) | Consumption, perhaps Publishing | All | Displays packages from a given source. |
| [mirror](cli-ref-mirror.md) | Publishing | Deprecated in 3.2+ | Mirrors a package and its dependencies from a source to a target repository. |
| [sources](cli-ref-sources.md) | Consumption, Publishing | All | Manages package sources in configuration files. |
| [update](cli-ref-update.md) | Consumption | All | Updates a project's packages to the latest available versions. Not supported when running on Mono. |

Different commands make use of various [Environment variables](cli-ref-environment-variables.md).

NuGet CLI commands by applicable roles:

| Role | Commands |
| --- | --- |
| Consumption | `config`, `help`, `install`, `list`, `locals`, `restore`, `setapikey`, `sources`, `update` | 
| Creation | `config`, `help`, `init`, `pack`, `spec` |
| Publishing | `add`, `config`, `delete`, `help`, `list`, `push`, `setapikey`, `sources` |

Developers concerned only with consuming packages, for example, need only understand that subset of NuGet commands.

> [!Note]
> Command option names are case-insensitive. Options that are deprecated are not included in this reference, such as `NoPrompt` (replaced by `NonInteractive`) and `Verbose` (replaced by `Verbosity`).
