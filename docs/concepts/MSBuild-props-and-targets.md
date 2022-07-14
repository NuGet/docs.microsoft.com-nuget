---
title: MSBuild props and targets in a package
description: Describes MSBuild props and targets in NuGet packages
author: nkolev92
ms.author: nikolev
ms.date: 07/13/2022
ms.topic: conceptual
---

# MSBuild .props and .targets in a package

In additional to the more traditional assemblies, NuGet packages may sometimes add custom build targets or properties to projects that consume that package.
This can be achieved by adding a valid MSBuild file, in the form `<package_id>.targets` or `<package_id>.props` (such as `Contoso.Utility.UsefulStuff.targets`) within the build folders of the project.

## Build folders

As NuGet has evolved, various different folders for build `.props` and `.targets` have been added.

| Folder | NuGet Version | Use |
|--------|---------------|-----|
| build | 2.5+ | Build logic for every framework of a project. |
| buildMultiTargetting | 4.0+ | Build logic for the `outer build` for projects that target multiple frameworks. PackageReference only. |
| buildTransitive | 5.0+ | Build logic for assets that flow transitively to any consuming project. See the [feature](https://github.com/NuGet/Home/wiki/Allow-package--authors-to-define-build-assets-transitive-behavior) page. PackageReference only. |

## Framework specific build folder

All 3 build folder follow the same pattern for deciding the most suitable file based on the project target framework.

Files in the root build folder are considered suitable for all target frameworks.
To provide framework-specific files, first place them within appropriate subfolders, such as the following:

```text
    \build
        \netstandard1.4
            \Contoso.Utility.UsefulStuff.props
            \Contoso.Utility.UsefulStuff.targets
        \net462
            \Contoso.Utility.UsefulStuff.props
            \Contoso.Utility.UsefulStuff.targets
```

Note that if a package does not have any files in the `lib` or `ref` folders and only files under a framework specific build folder, that package will be considered compatible with all projects. Up to date versions of the pack tooling, raise the  [NU5127](..\reference\errors-and-warnings\NU5127.md) warning when such packages are created.


## Projects consuming packages with build files

### PackageReference projects

`.props` and `.targets` are not added to the project file but are instead made available through `{projectName}.nuget.g.targets` and `{projectName}.nuget.g.props`. These files are automatically generated when restore is run.

When a project targets more than one framework, the imports to these files are conditioned on the target framework name.

MSBuild `.props` and `.targets` files for multi-framework targeting can be placed in the `\buildMultiTargeting` folder.
When the imports are generated, a condition that the MSBuild property `$(TargetFramework)` is empty is set.

### packages.config projects

When NuGet installs a package with `\build` files, it adds MSBuild `<Import>` elements in the project file pointing to the `.targets` and `.props` files. (`.props` is added at the top of the project file; `.targets` is added at the bottom.) A separate conditional MSBuild `<Import>` element is added for each target framework.

## Authoring packages with MSBuild props and targets

You can use any of the following tools to include MSBuild `.props` and `.targets` in your package.

- [NuGet.exe pack](..\create-packages\Creating-a-Package.md#include-msbuild-props-and-targets-in-a-package)
- [dotnet.exe pack](..\create-packages\creating-a-package-dotnet-cli.md)
- [MSBuild.exe pack](..\create-packages\creating-a-package-msbuild.md)

### Guidance for the content of MSBuild props and targets

NuGet does not limit how you author `.props` and `.targets` as they will vary based on the need of the package author and the target projects themselves.

There are a few things that must not be done in packages' `.props` and `.targets`, such as not specifying properties and items that affect restore, as those will be automatically excluded.

- Some examples of properties that must not be added or updated: TargetFramework, TargetFrameworkMoniker, TargetPlatformMoniker, AssetTargetFallback etc.

- Some examples of items that must not be added or updated: PackageReference, PackageVersion, PackageDownload, etc.
