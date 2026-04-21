---
title: Install and Manage NuGet Packages with the dotnet CLI
description: Find out how to use the dotnet command-line interface (CLI) to install, list, remove, and update NuGet packages in .NET projects.
author: mikejo5000
ms.author: mikejo
ms.date: 04/17/2026
ms.topic: how-to
# customer intent: As a developer, I want to find out how to use the dotnet CLI to manage NuGet packages so that I can use the packages in .NET projects.
---

# Install and manage NuGet packages with the dotnet CLI

You can use the dotnet command-line interface (CLI) tool on Windows, macOS, or Linux to easily install, uninstall, and update NuGet packages in .NET projects and solutions. This article describes the most common dotnet CLI commands for managing NuGet packages.

The dotnet CLI runs on .NET, .NET Core, .NET Standard SDK-style projects, and any other SDK-style projects, for example, those that target .NET Framework. For more information, see [.NET project SDKs](/dotnet/core/project-sdk/overview).

For most commands, the CLI tool looks for a project file in the current directory, unless a different project file is specified as an optional switch in the command. For a complete list of commands and their arguments, see [dotnet CLI commands](../reference/dotnet-Commands.md).

## Prerequisites

The [.NET SDK](https://dotnet.microsoft.com/download), which provides the dotnet CLI. In Visual Studio, the dotnet CLI automatically installs with all .NET-related workloads.

## Install or update a package

The [dotnet package add](/dotnet/core/tools/dotnet-package-add) command adds a package reference to the project file, and then runs `dotnet restore` to install the package.

1. Open a command-line window and go to the directory that contains your project file.

1. Use the following command to install a NuGet package:

   ```dotnetcli
   dotnet package add <package-name>
   ```

   For example, to install the `Newtonsoft.Json` package, use the following command:

   ```dotnetcli
   dotnet package add Newtonsoft.Json
   ```

   If you're using .NET 9 or earlier, use the verb-first form of the command instead:

   ```dotnetcli
   dotnet add package <package-name>
   ```

1. After the command finishes, open the project file to check for the package reference.

   For example, open the *.csproj* file and check for the added `Newtonsoft.Json` package reference:

   ```xml
   <ItemGroup>
     <PackageReference Include="Newtonsoft.Json" Version="13.0.4" />
   </ItemGroup>
   ```

## Install a specific version of a package

The `dotnet package add` command installs the latest version of the package unless you specify a different version.

To install a specific version of a NuGet package, use the optional `-v` or `--version` switch:

```dotnetcli
dotnet package add <package-name> -v <version>
```

For example, to add version 13.0.1 of the `Newtonsoft.Json` package, use this command:

```dotnetcli
dotnet package add Newtonsoft.Json --version 13.0.1
```

## List package references

You can use the [dotnet package list](/dotnet/core/tools/dotnet-package-list) command to list the package references and versions for your project. From the directory that contains your project file, run the following command:

```dotnetcli
dotnet package list
```

If you're using .NET 9 or earlier, use the verb-first form instead:

```dotnetcli
dotnet list package
```

## Remove a package

You can use the [dotnet package remove](/dotnet/core/tools/dotnet-package-remove) command to remove a package reference from the project file. From the directory that contains your project file, run the following command:

```dotnetcli
dotnet package remove <package-name>
```

For example, to remove the `Newtonsoft.Json` package, use the following command:

```dotnetcli
dotnet package remove Newtonsoft.Json
```

If you're using .NET 9 or earlier, use the verb-first form instead:

```dotnetcli
dotnet remove package <package-name>
```

## Restore packages

[!INCLUDE [restore-dotnet-cli](includes/restore-dotnet-cli.md)]

## Related content

- [.NET CLI overview](/dotnet/core/tools)
- [Install and manage packages in Visual Studio using the NuGet Package Manager](install-use-packages-visual-studio.md)
- [Manage packages with the Visual Studio Package Manager Console (PowerShell)](install-use-packages-powershell.md)
