---
title: Install and manage NuGet packages with the dotnet CLI
description: See how to use the dotnet CLI to install, list, remove, and update NuGet packages.
author: mikejo5000
ms.author: mikejo
ms.date: 08/17/2022
ms.topic: conceptual
---

# Install and manage NuGet packages with the dotnet CLI

You can use the dotnet CLI tool on Windows, MacOS, or Linux to easily install, uninstall, and update NuGet packages in .NET projects and solutions. This article describes the most common dotnet CLI commands for managing NuGet packages.

The dotnet CLI runs on .NET, .NET Core, .NET Standard SDK-style projects, and any other SDK-style projects, for example those that target .NET Framework. For more information, see [.NET project SDKs](/dotnet/core/project-sdk/overview).

For most commands, the CLI tool looks for a project file in the current directory, unless a different project file is specified as an optional switch in the command. For a complete list of commands and their arguments, see [dotnet CLI commands](../reference/dotnet-commands.md).

## Prerequisites

- The [.NET Core SDK](https://www.microsoft.com/net/download/), which provides the `dotnet` command-line tool. Starting in Visual Studio 2017, the dotnet CLI automatically installs with all .NET and .NET Core related workloads.

## Install or update a package

The [dotnet add package](/dotnet/core/tools/dotnet-add-package) command adds a package reference to the project file, and then runs `dotnet restore` to install the package.

1. Open a command line and switch to the directory that contains your project file.

1. Use the following command to install a NuGet package:

    ```dotnetcli
    dotnet add package <PackageName>
    ```

    For example, to install the `Newtonsoft.Json` package, use the following command

    ```dotnetcli
    dotnet add package Newtonsoft.Json
    ```

1. After the command completes and you save the project, you can open the project file to see the package reference.

   For example, open the *.csproj* file to see the added `Newtonsoft.Json` package reference:

    ```xml
    <ItemGroup>
      <PackageReference Include="Newtonsoft.Json" Version="13.0.1" />
    </ItemGroup>
    ```

## Install a specific version of a package

The `dotnet add package` command installs the latest version of the package, unless you specify a different version.

To install a specific version of a NuGet package, use the optional `-v` or `--version` switch:

```dotnetcli
dotnet add package <PackageName> -v <version>
```

For example, to add version 12.0.1 of the `Newtonsoft.Json` package, use this command:

```dotnetcli
dotnet add package Newtonsoft.Json --version 12.0.1
```

## List package references

List the package references and versions for your project by using the [dotnet list package](/dotnet/core/tools/dotnet-list-package) command:

```dotnetcli
dotnet list package
```

## Remove a package

Use the [dotnet remove package](/dotnet/core/tools/dotnet-remove-package) command to remove a package reference from the project file.

```dotnetcli
dotnet remove package <PackageName>
```

For example, to remove the `Newtonsoft.Json` package, use the following command:

```dotnetcli
dotnet remove package Newtonsoft.Json
```

## Restore packages

[!INCLUDE [restore-dotnet-cli](includes/restore-dotnet-cli.md)]

## Next steps

- [Manage packages using the nuget.exe CLI](install-use-packages-nuget-cli.md)
- [Install and manage packages with the Package Manager Console](install-use-packages-powershell.md)
- [Install and manage packages in Visual Studio using the NuGet Package Manager](install-use-packages-visual-studio.md)
