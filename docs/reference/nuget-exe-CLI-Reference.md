---
title: NuGet Command-Line Interface (CLI) Reference
description: Command-line reference index for the nuget.exe CLI
author: JonDouglas
ms.author: jodou
ms.date: 01/23/2018
ms.topic: reference
---

# NuGet CLI reference

The NuGet Command Line Interface (CLI), `nuget.exe`, provides the full extent of NuGet functionality to install, create, publish, and manage packages without making any changes to project files.

To use any command, open a command window or bash shell, then run `nuget` followed by the command and appropriate options, such as `nuget help pack` (to view help on the pack command).

This documentation reflects the latest version of the NuGet CLI. For exact details for any given version that you're using,  run `nuget help` for the desired command.

To learn how to use basic commands with the `nuget.exe` CLI, see [Install and use packages using the nuget.exe CLI](../consume-packages/install-use-packages-nuget-cli.md).

## Installing nuget.exe

[!INCLUDE [install-cli](../includes/install-cli.md)]

> [!Tip]
> To make the NuGet CLI available within the Package Manager Console in Visual Studio, see [Using the nuget.exe CLI in the console](../consume-packages/install-use-packages-powershell.md#use-the-nugetexe-cli-in-the-console).

## Availability

See [feature availability](../install-nuget-client-tools.md#feature-availability) for exact details.

- All commands are available on Windows.
- All commands work with nuget.exe running on Mono except where indicated for `pack`, `restore`, and `update`.
- The `pack`, `restore`, `delete`, `locals`, and `push` commands are also available on Mac and Linux through the dotnet CLI.

## Commands and applicability

Available commands and applicability to package creation, package consumption, and/or publishing a package to a host:

| Common Commands | Applicable Roles | NuGet Version | Description |
| --- | --- | --- | --- |
| [pack](cli-reference/cli-ref-pack.md) | Creation | 2.7+ | Creates a NuGet package from a `.nuspec` or project file. When running on Mono, creating a package from a project file is not supported. |
| [push](cli-reference/cli-ref-push.md) | Publishing | All | Publishes a package to a package source. |
| [config](cli-reference/cli-ref-config.md) | All | All | Gets or sets NuGet configuration values. |
| [help or ?](cli-reference/cli-ref-help.md) | All | All | Displays help information or help for a command. |
| [locals](cli-reference/cli-ref-locals.md) | Consumption | 3.3+ | Lists locations of the *global-packages*, *http-cache*, and *temp* folders and clears the contents of those folders. |
| [restore](cli-reference/cli-ref-restore.md) | Consumption | 2.7+ | Restores all packages referenced by the package management format in use. When running on Mono, restoring packages using the PackageReference format is not supported. |
| [setapikey](cli-reference/cli-ref-setapikey.md) | Publishing, Consumption | All | Saves an API key for a given package source when that package source requires a key for access. |
| [spec](cli-reference/cli-ref-spec.md) | Creation | All | Generates a `.nuspec` file, using tokens if generating the file from a Visual Studio project. |

| Secondary Commands | Applicable Roles | NuGet Version | Description |
| --- | --- | --- | --- |
| [add](cli-reference/cli-ref-add.md) | Publishing | 3.3+ | Adds a package to a non-HTTP package source using hierarchical layout. For HTTP sources, use *push*. |
| [delete](cli-reference/cli-ref-delete.md) | Publishing | All | Removes or unlists a package from a package source. |
| [init](cli-reference/cli-ref-init.md) | Creation | 3.3+ | Adds packages from a folder to a package source using hierarchical layout. |
| [install](cli-reference/cli-ref-install.md) | Consumption | All | Installs a package into the current project but does not modify projects or reference files. |
| [list](cli-reference/cli-ref-list.md) | Consumption, perhaps Publishing | All | Displays packages from a given source. |
| [mirror](cli-reference/cli-ref-mirror.md) | Publishing | Deprecated in 3.2+ | Mirrors a package and its dependencies from a source to a target repository. |
| [search](cli-reference/cli-ref-search.md) | Consumption | 5.8+ | Searches a given source using the query string provided. |
| [sources](cli-reference/cli-ref-sources.md) | Consumption, Publishing | All | Manages package sources in configuration files. |
| [update](cli-reference/cli-ref-update.md) | Consumption | All | Updates a project's packages to the latest available versions. Not supported when running on Mono. |

Different commands make use of various [Environment variables](cli-reference/cli-ref-environment-variables.md).

NuGet CLI commands by applicable roles:

| Role | Commands |
| --- | --- |
| Consumption | `config`, `help`, `install`, `list`, `locals`, `restore`, `search`, `setapikey`, `sources`, `update` |
| Creation | `config`, `help`, `init`, `pack`, `spec` |
| Publishing | `add`, `config`, `delete`, `help`, `list`, `push`, `setapikey`, `sources` |

Developers concerned only with consuming packages, for example, need only understand that subset of NuGet commands.

> [!Note]
> Command option names are case-insensitive. Options that are deprecated are not included in this reference, such as `NoPrompt` (replaced by `NonInteractive`) and `Verbose` (replaced by `Verbosity`).

## Localization

NuGet.exe's progress, warning and error messages are  translated in the same locales as Visual Studio.
NuGet.exe ships as a single exe, and due to size considerations, only the most commonly surface messages are translated in all languages.
