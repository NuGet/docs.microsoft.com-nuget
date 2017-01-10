---
# required metadata

title: NuGet Package References in Project Files | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 1/9/2017
ms.topic: article
ms.prod: nuget
#ms.service:
ms.technology: nuget
ms.assetid: 5a554e9d-1266-48c2-92e8-6dd00b1d6810

# optional metadata

#description:
#keywords:
#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer:
- karann
- unnir
#ms.suite:
#ms.tgt_pltfrm:
#ms.custom:

---
# Package References in Project Files

Package references, using the `PackageReference` node, allow you to manage NuGet dependencies directly in .NET Core project files, without needing a separate `packages.config` or `project.json` file. This approach also allows you to use MSBuild conditions to choose package references per target framework, configuration, platform, or other groupings. It also allows for fine-grained control over dependencies and content flow. In terms of behavior and dependency resolution, it is the same as using `project.json`

> [!Note]
> Package references are currently supported for only .NET Core projects in Visual Studio 2017.

## Adding a PackageReference

You can add a dependency in your project file using the following syntax:

    <ItemGroup>
        <!-- ... -->
        <PackageReference Include="Contoso.Utility.UsefulStuff">
            <Version>3.6.0</Version>
        </PackageReference>
        <!-- ... -->
    </ItemGroup>

or, alternately:

    <ItemGroup>
        <!-- ... -->
        <PackageReference Include="Contoso.Utility.UsefulStuff" Version="3.6.0" />
        <!-- ... -->
    </ItemGroup>

## Controlling dependency version

The convention for specifying version remains unchanged:

    <ItemGroup>
        <!-- ... -->
        <PackageReference Include="Contoso.Utility.UsefulStuff">
            <Version>3.6.0</Version>
        </PackageReference>
        <!-- ... -->
    </ItemGroup>

In the example above, 3.6.0 means any version that is >=3.6.0 with preference for the lowest version, as described on [version ranges](../create-packages/dependency-versions.md#version-ranges).

## Floating Versions

[Floating versions](../consume-packages/dependency-resolution.md#floating-versions) are supported with `PackageReference`:

    <ItemGroup>
        <!-- ... -->
        <PackageReference Include="Contoso.Utility.UsefulStuff">
            <Version>3.6.*</Version>
        </PackageReference>

        <PackageReference Include="Contoso.Utility.UsefulStuff">
            <Version>3.6.0-beta*</Version>
        </PackageReference>
        <!-- ... -->
    </ItemGroup>

## Controlling dependency assets

You might be using a dependency purely as a development harness and might not want to expose that to projects that will consume your package. In this scenario, you can use the `PrivateAssets` metadata to control this behavior.

    <ItemGroup>
        <!-- ... -->

        <PackageReference Include="Contoso.Utility.UsefulStuff" Version="3.6.0">
            <PrivateAssets>All</PrivateAssets>
        </PackageReference>

        <!-- ... -->
    </ItemGroup>

The following metadata tags control dependency assets:

| Tag | Description | Default Value
| IncludeAssets | These assets will be consumed | all
| ExcludeAssetsTag | These assets will not be consumed | none
| PrivateAssets | These assets will be consumed but won't flow to the parent project | contentfiles;analyzers;build


Allowable values for these tags are as follows, with multiple values separated by a semicolon except with `all` and `none` which must appear by themselves:

| Value | Description
| compile | Contents of the `lib` folder
| runtime | Contents of the `runtime` folder
| contentFiles | Contents of the `contentfiles` folder
| build | Props and targets in the `build` folder
| anaylzers | .NET analyzers
| native | Contents of the `native` folder
| none | None of the above are used.
| all | All of the above (except `none`)

In the following example, everything except the content files from the package would be consumed by the project and everything except content files and analyzers would flow to the parent project.

    <ItemGroup>
        <!-- ... -->

        <PackageReference Include="Contoso.Utility.UsefulStuff" Version="3.6.0">
            <IncludeAssets>All</IncludeAssets>
            <ExcludeAssets>contentFiles</ExcludeAssets>
            <PrivateAssets>contentFiles;analyzers</PrivateAssets>
        </PackageReference>

        <!-- ... -->
    </ItemGroup>

Note that because `build` is not included with `PrivateAsset`, targets and props *will* flow to the parent project. Consider, for example, that the reference above is used in a project that builds a NuGet package called AppLogger. AppLogger can consume the targets and props from Contoso.Utility.UsefulStuff, as can projects that consume AppLogger.

## Adding a PackageReference condition

You can use a condition to control whether a package is included, where conditions can use any MSBuild variable or a variable defined in the targets or props file.

For example, say you're targeting `netstandard1.4` as well as `net452` but have a dependency that is applicable only for `net452`. In this case you don't want a `nestandard1.4` project that's consuming your package to add that unnecessary dependency. To prevent this, you specify a condition on the `PackageReference` as follows:

    <ItemGroup>
        <!-- ... -->
        <PackageReference Include="Newtonsoft.json" Condition="'$(TargetFramework)' == 'net452'">
            <Version>9.0.1</Version>
        </PackageReference>
        <!-- ... -->
    </ItemGroup>

A package built using this project will show that Newtonsoft.json is included as a dependency only for a `net452` target:

![The result of applying a Condition on PackageReference](media/PackageReference-Condition.png)

Conditions can also be applied at the `ItemGroup` level and will apply to all children `PackageReference` elements:

    <ItemGroup Condition = "'$(TargetFramework)' == 'net452'>
        <!-- ... -->

        <PackageReference Include="Newtonsoft.json">
            <Version>9.0.1</Version>
        </PackageReference>

        <PackageReference Include="Contoso.Utility.UsefulStuff" Version="3.6.0">

        <!-- ... -->
    </ItemGroup>
