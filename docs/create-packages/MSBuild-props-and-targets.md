---
title: MSBuild props and targets in a package
description: Describes package source mapping functionality and how to onboard
author: nkolev92
ms.author: nikolev
ms.date: 10/15/2021
ms.topic: conceptual
---

# MSBuild props and targets in a package

In additional to the more traditional assemblies, NuGet packages may sometimes add custom build targets or properties to projects that consume that package.
This can be achieved by adding a valid MSBuild file, in the form `<package_id>.targets` or `<package_id>.props` (such as `Contoso.Utility.UsefulStuff.targets`) within the build folders of the project.

## Build folders


Files in the root `\build` folder are considered suitable for all target frameworks. To provide framework-specific files, first place them within appropriate subfolders, such as the following:

```
    \build
        \netstandard1.4
            \Contoso.Utility.UsefulStuff.props
            \Contoso.Utility.UsefulStuff.targets
        \net462
            \Contoso.Utility.UsefulStuff.props
            \Contoso.Utility.UsefulStuff.targets
```

Then in the `.nuspec` file, be sure to refer to these files in the `<files>` node:

```xml
<?xml version="1.0"?>
<package >
    <metadata minClientVersion="2.5">
    <!-- ... -->
    </metadata>
    <files>
        <!-- Include everything in \build -->
        <file src="build\**" target="build" />

        <!-- Other files -->
        <!-- ... -->
    </files>
</package>
```

When NuGet installs a package with `\build` files, it adds MSBuild `<Import>` elements in the project file pointing to the `.targets` and `.props` files. (`.props` is added at the top of the project file; `.targets` is added at the bottom.) A separate conditional MSBuild `<Import>` element is added for each target framework.

MSBuild `.props` and `.targets` files for cross-framework targeting can be placed in the `\buildMultiTargeting` folder. During package installation, NuGet adds the corresponding `<Import>` elements to the project file with the condition, that the target framework is not set (the MSBuild property `$(TargetFramework)` must be empty).

With NuGet 3.x, targets are not added to the project but are instead made available through `{projectName}.nuget.g.targets` and `{projectName}.nuget.g.props`.

[introduced with NuGet 2.5](../release-notes/NuGet-2.5.md#automatic-import-of-msbuild-targets-and-props-files)