---
title: dotnet NuGet commands
description: A short reference for NuGet-related commands using the dotnet command-line interface.
author: karann-msft
ms.author: karann
ms.date: 01/23/2018
ms.topic: conceptual
---

# dotnet commands

The `dotnet` command-line interface provides several NuGet commands and supports Windows, Linux, and macOS.

For complete information on `dotnet`, see [.NET Core command-line interface (CLI) tools](/dotnet/core/tools/?tabs=netcore2x).

## Package consumption

- [**dotnet add package**](/dotnet/core/tools/dotnet-add-package): Adds a package reference to the project file, then runs `dotnet restore` to install the package.
- [**dotnet remove package**](/dotnet/core/tools/dotnet-remove-package): Removes a package reference from the project file.
- [**dotnet restore**](/dotnet/core/tools/dotnet-restore?tabs=netcore2x): Restores the dependencies and tools of a project. As of NuGet 4.0, this runs the same code as `nuget restore`.
- [**dotnet nuget locals**](/dotnet/core/tools/dotnet-nuget-locals): Lists locations of the *global-packages*, *http-cache*, and *temp* folders and clears the contents of those folders.
- [**dotnet new nugetconfig**](/dotnet/core/tools/dotnet-new): Creates a [`nuget.config`](https://docs.microsoft.com/en-us/nuget/reference/nuget-config-file) file to configure NuGet's behavior.

## Package creation

- [**dotnet pack**](/dotnet/core/tools/dotnet-pack?tabs=netcore2x): Packs the code into a NuGet package.
- [**dotnet nuget push**](/dotnet/core/tools/dotnet-nuget-push): Publishes a package to a NuGet server. Applicable to nuget.org, Azure Artifacts, and [third-party NuGet servers](../hosting-packages/overview.md).
- [**dotnet nuget delete**](/dotnet/core/tools/dotnet-nuget-delete): Deletes or unlists a package from a NuGet server. Applicable to nuget.org, Azure Artifacts, and [third-party NuGet servers](../hosting-packages/overview.md).
