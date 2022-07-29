---
title: Create and publish a NuGet package using the dotnet CLI
description: A walkthrough tutorial on creating and publishing a NuGet package using the .NET Core CLI, dotnet.
author: JonDouglas
ms.author: jodou
ms.date: 05/24/2019
ms.topic: quickstart
---

# Quickstart: Create and publish a package (dotnet CLI)

It's a simple process to create a NuGet package from a .NET Class Library and publish it to nuget.org using the `dotnet` command-line interface (CLI).

## Prerequisites

1. Install the [.NET Core SDK](https://www.microsoft.com/net/download/), which includes the `dotnet` CLI. Starting in Visual Studio 2017, the dotnet CLI is automatically installed with any .NET Core related workloads.

1. [Register for a free account on nuget.org](https://www.nuget.org/users/account/LogOn?returnUrl=%2F) if you don't have one already. Creating a new account sends a confirmation email. You must confirm the account before you can upload a package.

## Create a class library project

You can use an existing .NET Class Library project for the code you want to package, or create a simple one as follows:

1. Create a folder called `AppLogger`.

1. Open a command prompt and switch to the `AppLogger` folder.

1. Type `dotnet new classlib`, which uses the name of the current folder for the project.

   This creates the new project.

## Add package metadata to the project file

Every NuGet package needs a manifest that describes the package's contents and dependencies. In a final package, the manifest is a `.nuspec` file that is generated from the NuGet metadata properties that you include in the project file.

1. Open your project file (`.csproj`, `.fsproj` or `.vbproj` depending on the language you're using) and add the following minimal properties inside the existing `<PropertyGroup>` tag, changing the values as appropriate:

    ```xml
    <PackageId>AppLogger</PackageId>
    <Version>1.0.0</Version>
    <Authors>your_name</Authors>
    <Company>your_company</Company>
    ```

    > [!Important]
    > Give the package an identifier that's unique across nuget.org or whatever host you're using. For this walkthrough we recommend including "Sample" or "Test" in the name as the later publishing step does make the package publicly visible (though it's unlikely anyone will actually use it).

1. Add any optional properties described on [NuGet metadata properties](/dotnet/core/tools/csproj#nuget-metadata-properties).

    > [!Note]
    > For packages built for public consumption, pay special attention to the **PackageTags** property, as tags help others find your package and understand what it does.

## Run the pack command

To build a NuGet package (a `.nupkg` file) from the project, run the `dotnet pack` command, which also builds the project automatically:

```dotnetcli
# Uses the project file in the current folder by default
dotnet pack
```

The output shows the path to the `.nupkg` file:

```output
Microsoft (R) Build Engine version 15.5.180.51428 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Restore completed in 29.91 ms for D:\proj\AppLoggerNet\AppLogger\AppLogger.csproj.
  AppLogger -> D:\proj\AppLoggerNet\AppLogger\bin\Debug\netstandard2.0\AppLogger.dll
  Successfully created package 'D:\proj\AppLoggerNet\AppLogger\bin\Debug\AppLogger.1.0.0.nupkg'.
```

### Automatically generate package on build

To automatically run `dotnet pack` when you run `dotnet build`, add the following line to your project file within `<PropertyGroup>`:

```xml
<GeneratePackageOnBuild>true</GeneratePackageOnBuild>
```

## Publish the package

Once you have a `.nupkg` file, you publish it to nuget.org using the `dotnet nuget push` command along with an API key acquired from nuget.org.

[!INCLUDE [publish-notes](includes/publish-notes.md)]

### Acquire your API key

[!INCLUDE [publish-api-key](includes/publish-api-key.md)]

### Publish with dotnet nuget push

[!INCLUDE [publish-dotnet](includes/publish-dotnet.md)]

### Publish errors

[!INCLUDE [publish-errors](includes/publish-errors.md)]

### Manage the published package

[!INCLUDE [publish-manage](includes/publish-manage.md)]

## Related video

> [!Video https://docs.microsoft.com/shows/NuGet-101/Create-and-Publish-a-NuGet-Package-with-the-NET-CLI-5-of-5/player]

Find more NuGet videos on [Channel 9](/shows/NuGet-101/) and [YouTube](https://www.youtube.com/playlist?list=PLdo4fOcmZ0oVLvfkFk8O9h6v2Dcdh2bh_).

## Next steps

Congratulations on creating your first NuGet package!

> [!div class="nextstepaction"]
> [Create a Package](../create-packages/creating-a-package-dotnet-cli.md)

To explore more that NuGet has to offer, select the links below.

- [Publish a Package](../nuget-org/publish-a-package.md)
- [Pre-release Packages](../create-packages/Prerelease-Packages.md)
- [Support multiple target frameworks](../create-packages/multiple-target-frameworks-project-file.md)
- [Package versioning](../concepts/package-versioning.md)
- [Adding a license expression or file](../reference/msbuild-targets.md#packing-a-license-expression-or-a-license-file)
- [Creating localized packages](../create-packages/creating-localized-packages.md)
- [Creating symbol packages](../create-packages/symbol-packages-snupkg.md)
- [Signing packages](../create-packages/Sign-a-package.md)
