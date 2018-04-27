---
title: Creating and publishing a NuGet package using the dotnet CLI
description: A walkthrough tutorial on creating and publishing a NuGet package using the .NET Core CLI, dotnet.
author: kraigb
ms.author: kraigb
manager: douge
ms.date: 01/24/2018
ms.topic: quickstart
---

# Quickstart: Create and publish a package (dotnet CLI)

It's a simple process to create a NuGet package from a .NET Class Library and publish it to nuget.org using the `dotnet` command-line interface (CLI).

## Prerequisites

1. Install the [.NET Core SDK](https://www.microsoft.com/net/download/), which includes the `dotnet` CLI.

1. [Register for a free account on nuget.org](https://www.nuget.org/users/account/LogOn?returnUrl=%2F) if you don't have one already. Creating a new account sends a confirmation email. You must confirm the account before you can upload a package.

## Create a class library project

You can use an existing .NET Class Library project for the code you want to package, or create a simple one as follows:

1. Create a folder called `AppLogger` and change into it.

1. Create the project using `dotnet new classlib`, which uses the name of the current folder for the project.

## Add package metadata to the project file

Every NuGet package needs a manifest that describes the package's contents and dependencies. In a final package, the manifest is a `.nuspec` file that is generated from the NuGet metadata properties that you include in the project file.

1. Open your project file (`.csproj`) and add the following minimal properties inside the exiting `<PropertyGroup>` tag, changing the values as appropriate:

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

```cli
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

## Related topics

- [Create a Package](../create-packages/creating-a-package.md)
- [Publish a Package](../create-packages/publish-a-package.md)
- [Pre-release Packages](../create-packages/Prerelease-Packages.md)
- [Support multiple target frameworks](../create-packages/supporting-multiple-target-frameworks.md)
- [Package versioning](../reference/package-versioning.md)
- [Creating localized packages](../create-packages/creating-localized-packages.md)
- [Signing packages](../create-packages/Sign-a-package.md)
