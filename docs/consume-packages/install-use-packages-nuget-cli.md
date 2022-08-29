---
title: Manage NuGet packages with the NuGet CLI
description: Instructions for using the NuGet CLI, nuget.exe, to manage NuGet packages.
author: mikejo5000
ms.author: mikejo
ms.date: 08/29/2022
ms.topic: conceptual
---

# Manage NuGet packages with the NuGet CLI

You can use the `nuget.exe` CLI tool to manage NuGet packages in Visual Studio projects and solutions. This article describes the most common NuGet CLI commands for managing NuGet packages. All these commands work on Windows, and most work on Mac and on Linux with Mono. 

The NuGet CLI runs on .NET Framework and non-SDK-style projects, for example non-SDK style projects that target .NET Standard libraries. The NuGet CLI commands can use a project [packages.config](../reference/packages-config.md) file that lists package references. For non-SDK-style projects that use `PackageReference` instead of *packages.config* for package references, use the [dotnet CLI](install-use-packages-dotnet-cli.md) instead.

> [!NOTE]
> For most non-SDK-style projects that use *packages.config*, it's best to [migrate packages.config to PackageReference](migrate-packages-config-to-package-reference.md), and then use the dotnet CLI instead of the NuGet CLI to manage packages. However, you can't migrate C++ or ASP.NET projects.

For most commands, the NuGet CLI tool uses the current directory, unless you specify a different location in the command. To run NuGet CLI commands, open a command line and switch to the directory that contains your project file.

For a complete list of commands and their arguments, see the [NuGet CLI reference](../reference/nuget-exe-cli-reference.md).

## Prerequisites

Download the NuGet CLI from [nuget.org](https://dist.nuget.org/win-x86-commandline/latest/nuget.exe). Save the *nuget.exe* file to a suitable directory, and make sure the directory is in your PATH environment variable.

> [!NOTE]
> You can also use the [winget](/windows/package-manager/winget) tool for Windows or [Homebrew](https://brew.sh/) for macOS.

To find out your NuGet CLI version, open a command line and run `nuget help`, or to avoid having to scroll up, use `nuget help | more`. The first line in the help output shows the version.

## Install a package

The NuGet CLI [install](../reference/cli-reference/cli-ref-install.md) command downloads and installs specified NuGet packages.

> [!IMPORTANT]
> The `install` command doesn't modify the project file or *packages.config* file. The `install` and `restore` commands only add packages to disk, but don't add dependencies to projects. To add project dependencies, add packages through the [Visual Studio Package Manager UI](install-use-packages-visual-studio.md) or [Package Manager Console](install-use-packages-powershell.md), then run `install` or `restore`.

Use the `-OutputDirectory` option to install packages to a specific directory. If you don't specify an output directory, `install` uses the current directory.

```cli
nuget install <packageID | configFilePath> -OutputDirectory <outputDirectory>
```

For example, to install the `Newtonsoft.json` package to the *packages* subdirectory, use the following command:

```cli
nuget install Newtonsoft.Json -OutputDirectory packages
```

Instead of specifying a package to install, you can specify an existing *packages.config* file in the current or another directory. The `install` command installs all the packages listed in the *packages.config* file.

```cli
nuget install packages.config
```

For example, the following command installs all the packages listed in *packages.config* in the *config* subdirectory to the *packages* subdirectory:

```cli
nuget install config\packages.config -OutputDirectory packages

```

## Install a specific version of a package

The `install` command installs the latest version of a package unless you specify a different version. To install a specific version of a package, use the `-Version` option:

```cli
nuget install <packageID | configFilePath> -Version <version>
```

For example, to install version 12.0.1 of the `Newtonsoft.json` package, use:

```cli
nuget install Newtonsoft.Json -Version 12.0.1
```

## List packages

Use the [list](../reference/cli-reference/cli-ref-list.md) command to display a list of packages installed in the packages folders. Use the `-Source` option to restrict the list.

```cli
nuget list -Source <source>
```

For example, to list packages in the *packages* subdirectory of *MyProject*, use:

```cli
nuget list -Source C:\Users\%USERNAME%\source\repos\MyProject\packages
```

You can also use a search term to search for package names, tags, or descriptions:

```cli
nuget list <"search term"> -Source <source>
```

## Update all packages

Use the [update](../reference/cli-reference/cli-ref-update.md) command to update all packages in a project *packages.config* file to their latest available versions. It's best to run `restore` before you run `update`.

```cli
nuget update
```

## Remove a package

To remove a package, delete that package from the project folder. To reinstall packages, use the `restore` or `install` commands.

Deleting packages from disk doesn't update the project, *packages.config*, or *NuGet.Config* files. The best way to remove packages is through the Visual Studio [Package Manager UI](install-use-packages-visual-studio.md) or [Package Manager Console](install-use-packages-powershell.md).

## Restore packages

[!INCLUDE [restore-nuget-exe-cli](includes/restore-nuget-exe-cli.md)]

For more information, see [Restore packages](package-restore.md).

## Next steps

- [NuGet CLI reference](../reference/nuget-exe-cli-reference.md)
- [Install and manage packages in Visual Studio using the NuGet Package Manager](install-use-packages-visual-studio.md)
- [Migrate from packages.config to PackageReference](migrate-packages-config-to-package-reference.md)
- [Manage packages with the dotnet CLI](install-use-packages-dotnet-cli.md)
