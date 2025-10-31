---
title: dotnet CLI NuGet commands
description: A short reference for NuGet-related commands using the dotnet command-line interface.
author: JonDouglas
ms.author: jodou
ms.date: 06/24/2019
ms.topic: article
---

# dotnet CLI commands

The `dotnet` command-line interface (CLI), which runs on Windows, Mac OS X, and Linux, provides a number of essential commands such as installing, restoring, and publishing packages. If dotnet satisfies your needs, it's not necessary to use `nuget.exe`.

For examples of using these commands to consume packages, see [Install and manage packages using the dotnet CLI](../consume-packages/install-use-packages-dotnet-cli.md). For examples of using these commands to create packages, see [Create and publish a package using the dotnet CLI](../quickstart/create-and-publish-a-package-using-the-dotnet-cli.md).

For the complete command reference on `dotnet` CLI, see [.NET Core command-line interface (CLI) tools](/dotnet/core/tools/?tabs=netcore2x).

## Package consumption

- [**dotnet add package**](/dotnet/core/tools/dotnet-add-package): Adds a package reference to the project file, then runs `dotnet restore` to install the package.
- [**dotnet remove package**](/dotnet/core/tools/dotnet-remove-package): Removes a package reference from the project file.
- [**dotnet restore**](/dotnet/core/tools/dotnet-restore?tabs=netcore2x): Restores the dependencies and tools of a project. As of NuGet 4.0, this runs the same code as `nuget restore`.
- [**dotnet nuget locals**](/dotnet/core/tools/dotnet-nuget-locals): Lists locations of the *global-packages*, *http-cache*, and *temp* folders and clears the contents of those folders.
- [**dotnet new nugetconfig**](/dotnet/core/tools/dotnet-new): Creates a [`nuget.config`](../reference/nuget-config-file.md) file to configure NuGet's behavior.

## Package creation

- [**dotnet pack**](/dotnet/core/tools/dotnet-pack?tabs=netcore2x): Packs the code into a NuGet package.
- [**dotnet nuget push**](/dotnet/core/tools/dotnet-nuget-push): Publishes a package to a NuGet server. Applicable to nuget.org, Azure Artifacts, and [third-party NuGet servers](../hosting-packages/overview.md).
- [**dotnet nuget delete**](/dotnet/core/tools/dotnet-nuget-delete): Deletes or unlists a package from a NuGet server. Applicable to nuget.org, Azure Artifacts, and [third-party NuGet servers](../hosting-packages/overview.md).
- [**dotnet nuget verify**](/dotnet/core/tools/dotnet-nuget-verify): Verifies a signed NuGet package.
