---
title: Install and manage NuGet packages using the dotnet CLI
description: Instructions for using the dotnet CLI to work with NuGet packages.
author: mikejo5000
ms.author: mikejo
ms.date: 06/03/2019
ms.topic: conceptual
---

# Install and manage packages using the dotnet CLI

The CLI tool allows you to easily install, uninstall, and update NuGet packages in projects and solutions. It runs on Windows, Mac OS X, and Linux.

The dotnet CLI is for use in your .NET Core and .NET Standard project (SDK-style project types), and for any other SDK-style projects (for example, an SDK-style project that targets .NET Framework). For more information, see [SDK attribute](/dotnet/core/tools/csproj#additions).

This article shows you basic usage for a few of the most common dotnet CLI commands. For most of these commands, the CLI tool looks for a project file in the current directory, unless a project file is specified in the command (the project file is an optional switch). For a complete list of commands and the arguments you may use, see the [.NET Core command-line interface (CLI) tools](../reference/dotnet-commands.md).

## Prerequisites

- The [.NET Core SDK](https://www.microsoft.com/net/download/), which provides the `dotnet` command-line tool. Starting in Visual Studio 2017, the dotnet CLI is automatically installed with any .NET Core related workloads.

## Install a package

[dotnet add package](/dotnet/core/tools/dotnet-add-package?tabs=netcore2x) adds a package reference to the project file, then runs `dotnet restore` to install the package.

1. Open a command line and switch to the directory that contains your project file.

2. Use the following command to install a Nuget package:

    ```dotnetcli
    dotnet add package <PACKAGE_NAME>
    ```

    For example, to install the `Newtonsoft.Json` package, use the following command

    ```dotnetcli
    dotnet add package Newtonsoft.Json
    ```

3. After the command completes, look at the project file to make sure the package was installed.

   You can open the `.csproj` file to see the added reference:

    ```xml
    <ItemGroup>
      <PackageReference Include="Newtonsoft.Json" Version="12.0.1" />
    </ItemGroup>
    ```

## Install a specific version of a package

If the version is not specified, NuGet installs the latest version of the package. You can also use the [dotnet add package](/dotnet/core/tools/dotnet-add-package?tabs=netcore2x) command to install a specific version of a Nuget package:

```dotnetcli
dotnet add package <PACKAGE_NAME> --version <VERSION>
```

For example, to add version 12.0.1 of the `Newtonsoft.Json` package, use this command:

```dotnetcli
dotnet add package Newtonsoft.Json --version 12.0.1
```

## List package references

You can list the package references for your project using the [dotnet list package](/dotnet/core/tools/dotnet-list-package?tabs=netcore2x) command.

```dotnetcli
dotnet list package
```

## Remove a package

Use the [dotnet remove package](/dotnet/core/tools/dotnet-remove-package?tabs=netcore2x) command to remove a package reference from the project file.

```dotnetcli
dotnet remove package <PACKAGE_NAME>
```

For example, to remove the `Newtonsoft.Json` package, use the following command

```dotnetcli
dotnet remove package Newtonsoft.Json
```

## Update a package

NuGet installs the latest version of the package when you use the `dotnet add package` command unless you specify the package version (`-v` switch).

## Restore packages

[!INCLUDE [restore-dotnet-cli](includes/restore-dotnet-cli.md)]
