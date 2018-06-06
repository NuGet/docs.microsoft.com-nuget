---
title: NuGet project.json archive content
description: Miscellaneous bits of project.json content removed from other areas of the NuGet documentation.
author: karann-msft
ms.author: karann
manager: unnir
ms.date: 01/17/2018
ms.topic: conceptual
---

# project.json archive

The `project.json` management format was introduced with NuGet 3.x and used for certain project types. It was deprecated with the introduction of the PackageReference format, in which dependencies are listed directly in a project file.

Also see:

- [project.json schema](project-json.md)
- [project.json impact on package authors](project-json-impact.md)
- [project.json and UWP](project-json-and-uwp.md)

## project.json management format

*Originally in [Package restore](../what-is-nuget.md).*

In the list of management formats:

- [`project.json`](project-json.md): *(deprecated)* A JSON file that maintains a list of the project's dependencies with an overall package graph in an associated file, `project.lock.json`. This format is deprecated in favor of PackageReference.

## nuget restore on Mono

*Originally in [Install NuGet client tools](../install-nuget-client-tools.md).*

Works with `project.json`.

## Constraining package versions with restore

*Originally in [Package restore](../consume-packages/package-restore.md#constraining-package-versions-with-restore).*

- `project.json`: Specify a version range directly with the dependency's version number. For example:

    ```json
    "Newtonsoft.json": "[6, 7)"
    ```

## NuGet CLI commands

- `nuget install` does not work with `project.json`.
- `nuget restore`: with projects using `project.json`, generates a `project.lock.json` file and a `<project>.nuget.props` file, if needed. (Both files can be omitted from source control.) The `<projectPath>` argument can point a `project.json` file and has the same behavior as pointing to a `packages.config` or project file. In the priority order for package folders, `%userprofile%\.nuget\packages` is searched first when using `project.json`.
- `nuget update`: On Mono, this command does not work with projects using `project.json`.

## Dependency resolution with PackageReference

*Originally in [Dependency resolution](../consume-packages/dependency-resolution.md#dependency-resolution-with-packagereference).*

The behavior of PackageReference applies also to `project.json`. NuGet restore writes the dependency graph into a file named `project.lock.json` alongside `project.json`.

## Managing dependency assets

*Originally in [Dependency resolution](../consume-packages/dependency-resolution.md#managing-dependency-assets).*

When using the `project.json` format, you can control which assets from dependencies flow into the top-level project. For details, see [project.json](project-json.md).

## Excluding references

*Originally in [Dependency resolution](../consume-packages/dependency-resolution.md#excluding-references).*

- `project.json`: add `"exclude" : "all"` in the dependency for PackageC:

    ```json
    {
        "dependencies": {
            "PackageC": {
            "version": "1.0.0",
            "exclude": "all"
            }
        }
    }
    ```

## Resolving incompatible package errors

*Originally in [Dependency resolution](../consume-packages/dependency-resolution.md#resolving-incompatible-package-errors).*

An added means of resolving errors:

- **Not recommended**: as a temporary solution while you work with the package author, projects targeting `netcore`, `netstandard`, and `netcoreapp` can denote other frameworks as being compatible, thereby allowing packages targeting those other frameworks to be used. See [project.json imports](project-json.md#imports) and [MSBuild restore target PackageTargetFallback](../reference/msbuild-targets.md#packagetargetfallback). This can cause unexpected behaviors, so again, it's best to resolve package incompatibilities by working with the package author on an update.

## Target frameworks

*Originally in [Target frameworks](../reference/target-frameworks.md).*

- [project.json](project-json.md): The `frameworks` node specifies the framework versions that the project can be compiled against.

## Creating a package

*Originally in [Creating a package](../create-packages/creating-a-package.md)*

### Setting a package type

With .NET Core 1.x, when a DotnetCliTool package is installed, Visual Studio places the package in the `project.json` `tools` node instead of the `dependencies` node.

Package types are set in `project.json`.

- `project.json`: Indicate the package type within a `packOptions.packageType` property json:

    ```json
    {
        // ...
        "packOptions": {
        "packageType": "DotnetCliTool"
        }
    }
    ```

### Adding targets and props for MSBuild

*Originally in [Create .NET Standard NuGet Packages with Visual Studio 2015](../guides/create-net-standard-packages-vs2015.md).*

When using `project.json`, targets are not added to the project but are made available through the `project.lock.json`.

### Package versioning

*Originally in [Package versioning](../reference/package-versioning.md).*

When using the `project.json` format, NuGet also supports using a wildcard notation, \*, for Major, Minor, Patch, and pre-release suffix parts of the number.

### NuGet.Config reference

*Originally in [NuGet.Config reference](../reference/nuget-config-file.md).*

`globalPackagesFolder` applies only to `project.json`. (Added note: also applies to PackageReference.)

### nuspec file reference

*Originally in [nuspec reference](../reference/nuspec.md).*

The `<contentFiles>` element is used instead of `<files>` with `project.json`.

### Package manager options control

*Originally in [Package Manager UI reference](../tools/package-manager-ui.md).*

Projects using `project.json` management format show only the **Show preview window** option.

### Visual Studio Templates

*Originally in [NuGet Packages in Visual Studio templates](../visual-studio-extensibility/visual-studio-templates.md).*

Best practices: templates do not include a `project.json` file, and do not include or any references or content that would be added when NuGet packages are installed.