---
title: Create and publish a NuGet package with the dotnet CLI
description: Walk through quickly creating and publishing a NuGet package by using the dotnet CLI.
author: JonDouglas
ms.author: jodou
ms.date: 03/03/2025
ms.topic: quickstart
---

# Quickstart: Create and publish a package with the dotnet CLI

This quickstart shows you how to quickly create a NuGet package from a .NET class library and publish it to nuget.org by using the .NET command-line interface, or [dotnet CLI](/dotnet/core/tools).

## Prerequisites

- The [.NET SDK](https://www.microsoft.com/net/download), which provides the dotnet command-line tool. Starting in Visual Studio 2017, the dotnet CLI automatically installs with any .NET or .NET Core related workloads.

- A free account on nuget.org. Follow the instructions at [Add a new individual account](../nuget-org/individual-accounts.md#add-a-new-individual-account).

## Create a class library project

You can use an existing .NET Class Library project for the code you want to package, or create a simple project as follows:

1. Create a folder named *AppLogger*.
1. Open a command prompt and switch to the *AppLogger* folder. All the dotnet CLI commands in this quickstart run on the current folder by default.
1. Enter `dotnet new classlib`, which creates a project with the current folder name.

For more information, see [dotnet new](/dotnet/core/tools/dotnet-new).

## Add package metadata to the project file

Every NuGet package has a manifest that describes the package's contents and dependencies. In the final package, the manifest is a *.nuspec* file, which uses the NuGet metadata properties you include in the project file.

Open the *.csproj*, *.fsproj*, or *.vbproj* project file, and add the following properties inside the existing `<PropertyGroup>` tag. Use your own values for name and company, and replace the package identifier with a unique value.

```xml
<PackageId>Contoso.08.28.22.001.Test</PackageId>
<Version>1.0.0</Version>
<Authors>your_name</Authors>
<Company>your_company</Company>
```

> [!Important]
> The package identifier must be unique across nuget.org and other package sources. Publishing makes the package publicly visible, so if you use the example AppLogger library or other test library, use a unique name that includes `Sample` or `Test`.

You can add any optional properties described in [NuGet metadata properties](../reference/msbuild-targets.md#pack-target).

> [!Note]
> For packages you build for public consumption, pay special attention to the `PackageTags` property. Tags help others find your package and understand what it does.

## Run the pack command

To build a NuGet package or *.nupkg* file from the project, run the [dotnet pack](/dotnet/core/tools/dotnet-pack) command, which also builds the project automatically.

```dotnetcli
dotnet pack
```

The output shows the path to the *.nupkg* file:

```output
MSBuild version 17.3.0+92e077650 for .NET
  Determining projects to restore...
  Restored C:\Users\myname\source\repos\AppLogger\AppLogger.csproj (in 64 ms).
  AppLogger -> C:\Users\myname\source\repos\AppLogger\bin\Debug\net6.0\AppLogger.dll
  Successfully created package 'C:\Users\myname\source\repos\AppLogger\bin\Debug\Contoso.08.28.22.001.Test.1.0.0.nupkg'.
  ```

### Automatically generate package on build

To automatically run `dotnet pack` whenever you run `dotnet build`, add the following line to your project file within `<PropertyGroup>`:

```xml
    <GeneratePackageOnBuild>true</GeneratePackageOnBuild>
```

## Publish the package

Publish your *.nupkg* file to nuget.org by using the [dotnet nuget push](/dotnet/core/tools/dotnet-nuget-push) command with an API key you get from nuget.org.

[!INCLUDE [publish-notes](includes/publish-notes.md)]

### Get your API key

[!INCLUDE [publish-api-key](includes/publish-api-key-with-link.md)]

### Publish with dotnet nuget push

[!INCLUDE [publish-dotnet](includes/publish-dotnet.md)]

### Publish errors

[!INCLUDE [publish-errors](includes/publish-errors.md)]

### Manage the published package

[!INCLUDE [publish-manage](includes/publish-manage.md)]

Congratulations on creating and publishing your first NuGet package!

## Related video

> [!Video https://learn.microsoft.com/shows/dotnet-package-management-with-nuget-for-beginners/creating-and-publishing-a-nuget-package-nuget-for-beginners/player]

Find more NuGet videos on [Channel 9](/shows/NuGet-101/) and [YouTube](https://www.youtube.com/playlist?list=PLdo4fOcmZ0oVLvfkFk8O9h6v2Dcdh2bh_).

## Next steps


See more details about how to create packages with the dotnet CLI:

> [!div class="nextstepaction"]
> [Create a NuGet package with the dotnet CLI](../create-packages/creating-a-package-dotnet-cli.md)

Get more information about creating and publishing NuGet packages:

- [Publish a package](../nuget-org/publish-a-package.md)
- [Prerelease packages](../create-packages/Prerelease-Packages.md)
- [Support multiple target frameworks](../create-packages/multiple-target-frameworks-project-file.md)
- [Package versioning](../concepts/package-versioning.md)
- [Add a license expression or file](../reference/msbuild-targets.md#packing-a-license-expression-or-a-license-file)
- [Create localized packages](../create-packages/creating-localized-packages.md)
- [Create symbol packages](../create-packages/symbol-packages-snupkg.md)
- [Sign packages](../create-packages/Sign-a-package.md)
