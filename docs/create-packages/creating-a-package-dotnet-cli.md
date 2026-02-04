---
title: Create a NuGet package with the dotnet CLI
description: Read a detailed guide about the process of designing and creating a NuGet package, including key decision points like files and versioning.
author: JonDouglas
ms.author: jodou
ms.date: 03/03/2025
ms.topic: how-to
---

# Create a NuGet package with the dotnet CLI

NuGet packages contain code that developers can reuse in their projects. No matter what your code does or contains, you use a command-line tool, either `nuget.exe` or `dotnet.exe`, to create the NuGet package.

This article describes how to create a package by using the [dotnet CLI](). Starting in Visual Studio 2017, the dotnet CLI is included with all .NET and .NET Core workloads. If you need to install the dotnet CLI or other NuGet client tools, see [Install NuGet client tools](../install-nuget-client-tools.md).

This topic applies only to .NET and other projects that use the [SDK-style format](../resources/check-project-format.md). For these projects, NuGet uses information from the project file to create a package. For quickstart tutorials, see [Create packages with the dotnet CLI](../quickstart/create-and-publish-a-package-using-the-dotnet-cli.md) or [Create packages with Visual Studio](../quickstart/create-and-publish-a-package-using-visual-studio.md).

The MSBuild [msbuild -t:pack](creating-a-package-msbuild.md#run-the-msbuild--tpack-command) command is functionally equivalent to [dotnet pack](/dotnet/core/tools/dotnet-pack). For more information about creating a package with MSBuild, see [Create a NuGet package using MSBuild](creating-a-package-msbuild.md).

> [!NOTE]
> - To create and publish packages for non-SDK-style projects, typically .NET Framework projects, see [Create a package using the nuget.exe CLI](Creating-a-Package.md) or [Create and publish a package using Visual Studio (.NET Framework)](../quickstart/create-and-publish-a-package-using-visual-studio-net-framework.md).
> 
> - For projects migrated from *packages.config* to [PackageReference](../consume-packages/package-references-in-project-files.md), use `msbuild -t:pack`. For more information, see [Create a package after migration](../consume-packages/migrate-packages-config-to-package-reference.md#create-a-package-after-migration).

## Set properties

You can create an example class library project by using the `dotnet new classlib` command, and package the project by using `dotnet pack`. The `dotnet pack` command uses the following properties. If you don't specify values in the project file, the command uses default values.

- `PackageId`, the package identifier, must be unique across nuget.org and any other targets that host the package. If you don't specify a value, the command uses the `AssemblyName`.
- `Version` is a specific version number in the form `Major.Minor.Patch[-Suffix]`, where `-Suffix` identifies [prerelease versions](prerelease-packages.md). If not specified, the default value is `1.0.0`.
- `Authors` are the authors of the package. If not specified, the default value is the `AssemblyName`.
- `Company` is company information. If not specified, the default value is the `Authors` value.
- `Product` is product information. If not specified, the default value is the `AssemblyName`.

In Visual Studio, you can set these values in the project properties. Right-click the project in **Solution Explorer**, select **Properties**, and then select the **Package** section. You can also add the properties directly to the *.csproj* or other project file.

The following example shows a project file with package properties added.

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <TargetFramework>netstandard2.0</TargetFramework>
    <PackageId>UniqueID</PackageId>
    <Version>1.0.0</Version>
    <Authors>Author Name</Authors>
    <Company>Company Name</Company>
    <Product>Product Name</Product>
  </PropertyGroup>
</Project>
```

You can add other optional properties, such as `Title`, `PackageDescription`, and `PackageTags`.

>[!NOTE]
> For packages you build for public consumption, pay special attention to the `PackageTags` property. Tags help others find your package and understand what it does.

The `dotnet pack` command automatically converts `PackageReference`s  in your project files to dependencies in the created package. You can control which assets to include through the `IncludeAssets`, `ExcludeAssets` and `PrivateAssets` tags. For more information, see [Controlling dependency assets](../consume-packages/package-references-in-project-files.md#controlling-dependency-assets).

For more information about dependencies, optional properties, and versioning, see:

- [Package references in project files](../consume-packages/package-references-in-project-files.md)
- [Package versioning](../concepts/package-versioning.md)
- [NuGet metadata properties](/dotnet/core/tools/csproj#nuget-metadata-properties)
- [MSBuild pack targets](../reference/msbuild-targets.md#pack-target)

### Choose a unique package identifier and set the version number

[!INCLUDE [choose-package-id](includes/choose-package-id.md)]

### Add an optional description field

[!INCLUDE [add description to package](includes/add-description.md)]

## Run the pack command

To build the NuGet package or *.nupkg* file, run the [dotnet pack](/dotnet/core/tools/dotnet-pack) command from the project folder, which also builds the project automatically.

```dotnetcli
dotnet pack
```

The output shows the path to the *.nupkg* file:

```output
MSBuild version 17.3.0+92e077650 for .NET
  Determining projects to restore...
  Restored D:\proj\AppLoggerNet\AppLogger\AppLogger.csproj (in 97 ms).
  Successfully created package 'D:\proj\AppLoggerNet\AppLogger\bin\Debug\AppLogger.1.0.0.nupkg'.
```

### Automatically generate package on build

To automatically run `dotnet pack` whenever you run `dotnet build`, add the following line to your project file in the `<PropertyGroup>` tag:

```xml
<GeneratePackageOnBuild>true</GeneratePackageOnBuild>
```

> [!NOTE]
> When you automatically generate the package, packing increases the build time for your project.

Running `dotnet pack` on a solution packs all the projects in the solution that are packable, that is, have the `IsPackable` property set to `true`.

### Test package installation

Before you publish a package, you should test installing the package into a project. Testing ensures that the necessary files end up in their correct places in the project.

Test the installation manually in Visual Studio or on the command line by using the normal [package installation process](../consume-packages/overview-and-workflow.md#ways-to-install-a-nuget-package).

> [!IMPORTANT]
> - You can't change packages once created. If you correct a problem, change the package contents and repack.
> - After you recreate the package, retesting still uses the old version of the package until you [clear your global packages directory](../consume-packages/managing-the-global-packages-and-cache-folders.md#clearing-local-directories). Clearing the directory is especially important for packages that don't use a unique prerelease label on every build.

## Next steps

Once you create the package, you can publish the *.nupkg* file to the host of your choice.

> [!div class="nextstepaction"]
> [Publish a package](../nuget-org/publish-a-package.md)

See the following articles for ways to extend the capabilities of your package or support other scenarios:

- [Package versioning](../concepts/package-versioning.md)
- [Support multiple target frameworks](../create-packages/multiple-target-frameworks-project-file.md)
- [Add a package icon](../reference/nuspec.md#icon)
- [Transformations of source and configuration files](../create-packages/source-and-config-file-transformations.md)
- [Localization](../create-packages/creating-localized-packages.md)
- [Prerelease versions](../create-packages/prerelease-packages.md)
- [Set package type](../create-packages/set-package-type.md)
- [MSBuild props and targets](../concepts/MSBuild-props-and-targets.md)
- [Create packages with COM interop assemblies](../create-packages/author-packages-with-COM-interop-assemblies.md)
- [Create native packages](../guides/native-packages.md)
- [Create symbol packages (.snupkg)](symbol-packages-snupkg.md)
