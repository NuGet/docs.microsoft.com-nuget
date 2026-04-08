---
title: Manage NuGet Packages with the NuGet CLI
description: Find out how to use the NuGet CLI, nuget.exe, to manage NuGet packages in non-SDK-style projects that use a packages.config file to list package references.
author: mikejo5000
ms.author: mikejo
ms.date: 04/08/2026
ms.topic: how-to
# customer intent: As a developer, I want to find out how to use the NuGet CLI so that I can manage NuGet packages in my projects and solutions.
---

# Manage NuGet packages with the NuGet CLI

You can use the `nuget.exe` command-line interface (CLI) to manage NuGet packages in Visual Studio projects and solutions. This article describes the most common NuGet CLI commands for managing NuGet packages. All these commands work on Windows, and most work on macOS and on Linux with Mono. 

The NuGet CLI runs on .NET Framework and non-SDK-style projects, for example non-SDK-style projects that target .NET Standard libraries. The NuGet CLI commands can use a project [packages.config](../reference/packages-config.md) file that lists package references. For non-SDK-style projects that use `PackageReference` instead of *packages.config* for package references, use the [dotnet CLI](install-use-packages-dotnet-cli.md) instead.

> [!NOTE]
> For most non-SDK-style projects that use *packages.config*, it's best to [migrate packages.config to `PackageReference`](migrate-packages-config-to-package-reference.md), and then use the dotnet CLI instead of the NuGet CLI to manage packages. However, you can't migrate C++ or ASP.NET projects.

For most commands, the NuGet CLI tool uses the current folder, unless you specify a different location in the command. To run NuGet CLI commands, open a command-line program and switch to the folder that contains your project file.

For a complete list of commands and their arguments, see [NuGet CLI reference](../reference/nuget-exe-cli-reference.md).

## Prerequisites

Download the NuGet CLI from [nuget.org](https://dist.nuget.org/win-x86-commandline/latest/nuget.exe). Save the *nuget.exe* file to a suitable folder, and make sure the folder is in your `PATH` environment variable.

> [!NOTE]
> You can also use the [`winget`](/windows/package-manager/winget) tool for Windows or [Homebrew](https://brew.sh/) for macOS.

To check your NuGet CLI version, open a command-line program and run `nuget help`. Or to avoid having to scroll up, use `nuget help | more`. The first line in the help output shows the version.

## Install a package

The NuGet CLI [`install`](../reference/cli-reference/cli-ref-install.md) command downloads and installs specified NuGet packages.

> [!IMPORTANT]
> The `install` command doesn't modify the project file or *packages.config* file. The `install` and `restore` commands only add packages to disk, but don't add dependencies to projects. To add project dependencies, add packages by using the [Visual Studio Package Manager UI](install-use-packages-visual-studio.md) or [Package Manager Console](install-use-packages-powershell.md). If you then run `install` or `restore`, perhaps in another environment, only the declared dependencies are installed or restored.

Use the `-OutputDirectory` option to install packages in a specific folder. If you don't specify an output folder, `install` uses the current folder.

```cli
nuget install <package-ID | configuration-file-path> -OutputDirectory <output-folder>
```

For example, to install the `Newtonsoft.json` package in the *packages* subfolder, use the following command:

```cli
nuget install Newtonsoft.Json -OutputDirectory packages
```

Instead of specifying a package to install, you can specify an existing *packages.config* file in the current folder or another folder. The `install` command installs all the packages listed in the *packages.config* file.

```cli
nuget install packages.config
```

For example, the following command installs all the packages listed in *packages.config* in the *config* subfolder of the current folder. The command installs the packages in the *packages* folder.

```cli
nuget install config\packages.config -OutputDirectory packages

```

## Install a specific version of a package

The `install` command installs the latest version of a package unless you specify a different version. To install a specific version of a package, use the `-Version` option:

```cli
nuget install <package-ID> -Version <version>
```

For example, to install version 12.0.1 of the `Newtonsoft.json` package, use:

```cli
nuget install Newtonsoft.Json -Version 12.0.1
```

## List packages

Use the [`search`](../reference/cli-reference/cli-ref-search.md) command or the [`list`](../reference/cli-reference/cli-ref-list.md) command to display basic information about packages at a specified source.

For example, if you use a NuGet version that's earlier than 5.8, use `list` to list packages in the *packages* subfolder of *MyProject*:

```cli
nuget list -Source C:\Users\%USERNAME%\source\repos\MyProject\packages
```

If you use NuGet 5.8+, use `search` to display basic information about packages:

```cli
nuget search -Source C:\Users\%USERNAME%\source\repos\MyProject\packages
```

For either command, you can also specify search terms to limit the results by package names, tags, or descriptions:

```cli
# Use with NuGet 5.8+.
nuget search <search-terms> -Source <source>

# Use with NuGet versions earlier than 5.8.
nuget list <search-terms> -Source <source>
```

## Update all packages

Use the [`update`](../reference/cli-reference/cli-ref-update.md) command to update all packages in a project *packages.config* file to their latest available versions. For `<configuration-file-path>`, use the path to your *packages.config* file.

```cli
nuget update <configuration-file-path>
```

It's best to run `restore` before you run `update`. Then the `update` command has information about the package versions that are in use. That information helps it resolve dependencies correctly.

## Remove a package

To remove a package, you can delete that package from the project folder. To reinstall packages, use the `restore` or `install` commands.

Deleting packages from disk doesn't update the project, *packages.config*, or *NuGet.Config* files. The best way to remove packages is by using the Visual Studio [Package Manager UI](install-use-packages-visual-studio.md) or [Package Manager Console](install-use-packages-powershell.md).

## Restore packages

[!INCLUDE [restore-nuget-exe-cli](includes/restore-nuget-exe-cli.md)]

For more information, see [Restore packages](package-restore.md).

## Related content

- [NuGet CLI reference](../reference/nuget-exe-cli-reference.md)
- [Install and manage packages in Visual Studio using the NuGet Package Manager](install-use-packages-visual-studio.md)
- [Migrate from packages.config to `PackageReference`](migrate-packages-config-to-package-reference.md)
- [Install and manage NuGet packages with the dotnet CLI](install-use-packages-dotnet-cli.md)
