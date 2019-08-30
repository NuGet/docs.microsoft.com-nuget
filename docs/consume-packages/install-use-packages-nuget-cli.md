---
title: Manage NuGet packages using the nuget.exe CLI
description: Instructions for using the nuget.exe CLI to work with NuGet packages.
author: mikejo5000
ms.author: mikejo
ms.date: 06/03/2019
ms.topic: conceptual
---

# Manage packages using the nuget.exe CLI

The CLI tool allows you to easily update and restore NuGet packages in projects and solutions. This tool provides all NuGet capabilities on Windows, and also provides most features on Mac and Linux when running under Mono.

The `nuget.exe` CLI is for your .NET Framework project and non-SDK-style projects (for example, a non-SDK style project that targets .NET Standard libraries). If you are using a non-SDK-style project that has been migrated to `PackageReference`, use the `dotnet` CLI instead. The `nuget.exe` CLI requires a [packages.config](../reference/packages-config.md) file for package references.

> [!NOTE]
> In most scenarios, we recommend [migrating non-SDK-style projects](../consume-packages/migrate-packages-config-to-package-reference.md) that use `packages.config` to PackageReference, and then you can use the `dotnet` CLI instead of the `nuget.exe` CLI. Migration is not currently available for C++ and ASP.NET projects.

This article shows you basic usage for a few of the most common `nuget.exe` CLI commands. For most of these commands, the CLI tool looks for a project file in the current directory, unless a project file is specified in the command. For a complete list of commands and the arguments you may use, see the [nuget.exe CLI reference](../reference/nuget-exe-cli-reference.md).

## Prerequisites

- Install the `nuget.exe` CLI by downloading it from [nuget.org](https://dist.nuget.org/win-x86-commandline/latest/nuget.exe), saving that `.exe` file to a suitable folder, and adding that folder to your PATH environment variable.

## Install a package

The [install](../reference/cli-reference/cli-ref-install.md) command downloads and installs a package into a project, defaulting to the current folder, using specified package sources. Install new packages into the *packages* folder in your project root directory.

> [!IMPORTANT]
> The `install`command does not modify a project file or *packages.config*; in this way it's similar to `restore` in that it only adds packages to disk but does not change a project's dependencies. To add a dependency, either add a package through the Package Manager UI or Console in Visual Studio, or modify *packages.config* and then run either `install` or `restore`.

1. Open a command line and switch to the directory that contains your project file.

2. Use the following command to install a NuGet package to the *packages* folder.

    ```cli
    nuget install <packageID> -OutputDirectory packages
    ```

    To install the `Newtonsoft.json` package to the *packages* folder, use the following command:

    ```cli
    nuget install Newtonsoft.Json -OutputDirectory packages
    ```

Alternatively, you can use the following command to install a NuGet package using an existing `packages.config` file to the *packages* folder. This does not add the package to your project dependencies, but installs it locally.

```cli
nuget install packages.config -OutputDirectory packages
```

## Install a specific version of a package

If the version is not specified when you use the [install](../reference/cli-reference/cli-ref-install.md) command, NuGet installs the latest version of the package. You can also install a specific version of a Nuget package:

```cli
nuget install <packageID | configFilePath> -Version <version>
```

For example, to add version 12.0.1 of the `Newtonsoft.json` package, use this command:

```cli
nuget install Newtonsoft.Json -Version 12.0.1
```

For more information on the limitations and behavior of `install`, see [Install a package](#install-a-package).

## Remove a package

To delete one or more packages, delete the packages you want to remove from the *packages* folder.

If you want to reinstall packages, use the `restore` or `install` command.

## List packages

You can display a list of packages from a given source using the [list](../reference/cli-reference/cli-ref-list.md) command. Use the `-Source` option to restrict the search.

```cli
nuget list -Source <source>
```

For example, list packages in the *packages* folder.

```cli
nuget list -Source C:\Users\username\source\repos\MyProject\packages
```

If you use a search term, the search includes names of packages, tags, and package descriptions.

```cli
nuget list <search term>
```

## Update an individual package

NuGet installs the latest version of the package when you use the `install` command unless you specify the package version.

## Update all packages

Use the [update](../reference/cli-reference/cli-ref-update.md) command to update all packages. Updates all packages in a project (using `packages.config`) to their latest available versions. It is recommended to run `restore` before running `update`.

```cli
nuget update
```

## Restore packages

[!INCLUDE [restore-nuget-exe-cli](includes/restore-nuget-exe-cli.md)]

## Get the CLI version

Use this command:

```cli
nuget help
```

The first line in the help output shows the version. To avoid scrolling up, use `nuget help | more` instead.