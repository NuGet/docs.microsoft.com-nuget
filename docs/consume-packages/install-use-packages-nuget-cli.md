---
title: Manage NuGet packages with the NuGet CLI
description: Instructions for using the NuGet CLI, nuget.exe, to install, list, update, remove, and restore NuGet packages.
author: mikejo5000
ms.author: mikejo
ms.date: 08/25/2022
ms.topic: conceptual
---

# Manage NuGet packages by using the NuGet CLI

You can use the `nuget.exe` CLI tool to easily install, list, update, remove, and restore NuGet packages in Visual Studio projects and solutions. The NuGet CLI provides all NuGet capabilities on Windows, and provides most NuGet capabilities on Mac and on Linux with Mono. This article describes the most common NuGet CLI commands for managing NuGet packages.

The NuGet CLI runs on .NET Framework and non-SDK-style projects, for example non-SDK style projects that target .NET Standard libraries. The NuGet CLI requires a [packages.config](../reference/packages-config.md) file for package references. For non-SDK-style projects that use `PackageReference` for package references, use the [dotnet CLI](install-use-packages-dotnet-cli.md) instead.

> [!NOTE]
> For most non-SDK-style projects that use *packages.config*, it's best to [migrate packages.config to PackageReference](../consume-packages/migrate-packages-config-to-package-reference.md), and then use the dotnet CLI instead of the NuGet CLI. You can't migrate C++ or ASP.NET projects.

For most commands, the CLI tool looks for a project file in the current directory, unless you specify a different project file in the command. To run the following NuGet CLI commands, open a command line and switch to the directory that contains your project file.

For a complete list of commands and their arguments, see the [NuGet CLI reference](../reference/nuget-exe-cli-reference.md).

## Prerequisites

Download the NuGet CLI from [nuget.org](https://dist.nuget.org/win-x86-commandline/latest/nuget.exe). Save the *.exe* file to a folder, and add the folder to your PATH environment variable.

To find out the NuGet CLI version, open a command line and run `nuget help`. The first line in the help output shows the version. To avoid scrolling up, you can use `nuget help | more`.

## Install a package

The NuGet [install](../reference/cli-reference/cli-ref-install.md) command downloads the package you specify and installs it into the *packages* folder in your project root directory.

> [!IMPORTANT]
> The `install` command doesn't modify the project file or *packages.config* file. The `install` and `restore` commands only add the packages to disk, but don't change the project dependencies. To add a dependency, you can either:
> 
> - Add the package through the [Visual Studio Package Manager UI](install-use-packages-visual-studio.md) or [Package Manager Console](install-use-packages-powershell.md).
>   or
> - Modify *packages.config* manually, and then run `install` or `restore`.

Use the following command to install a NuGet package to the *packages* folder.

```cli
nuget install <packageID> -OutputDirectory packages
```

For example, to install the `Newtonsoft.json` package to the *packages* folder, use the following command:

```cli
nuget install Newtonsoft.Json -OutputDirectory packages
```

You can also use the following command to install an existing *packages.config* file to a project *packages* folder.

```cli
nuget install packages.config -OutputDirectory packages
```

## Install a specific version of a package

The `install` command installs the latest version of the package, unless you specify a different version. To install a specific version of a package, use the `-Version` switch:

```cli
nuget install <packageID | configFilePath> -Version <version>
```

For example, to add version 12.0.1 of the `Newtonsoft.json` package, use this command:

```cli
nuget install Newtonsoft.Json -Version 12.0.1
```

## List packages

Use the [list](../reference/cli-reference/cli-ref-list.md) command to display a list of packages. Use the `-Source` option to restrict the search.

```cli
nuget list -Source <source>
```

For example, to list packages in the *packages* folder, use:

```cli
nuget list -Source C:\Users\<username>\<projectFolder>\packages
```

If you don't specify a source, `list` uses all sources defined in the global configuration file *%AppData%\NuGet\NuGet.Config* (Windows) or *~/.nuget/NuGet/NuGet.Config*. If *NuGet.Config* specifies no sources, `list` uses the default feed, for example nuget.org.

You can also use a search term to specify names of packages, tags, and package descriptions:

```cli
nuget list <search term>
```

## Update all packages

Use the [update](../reference/cli-reference/cli-ref-update.md) command to update all packages in a project *packages.config* to their latest available versions. It's best to run `restore` before you run `update`.

```cli
nuget update
```

## Remove a package

To remove a package, delete that package from the *packages* folder. To reinstall packages, use the `restore` or `install` commands.

## Restore packages

[!INCLUDE [restore-nuget-exe-cli](includes/restore-nuget-exe-cli.md)]

## Next steps

- [Manage packages with the dotnet CLI](install-use-packages-dotnet-cli.md)
- [Install and manage packages with the Package Manager Console](install-use-packages-powershell.md)
- [Install and manage packages in Visual Studio using the NuGet Package Manager](install-use-packages-visual-studio.md)
