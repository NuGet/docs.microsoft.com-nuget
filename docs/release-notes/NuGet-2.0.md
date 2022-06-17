---
title: NuGet 2.0 Release Notes
description: Release notes for NuGet 2.0 including known issues, bug fixes, added features, and DCRs.
author: JonDouglas
ms.author: jodou
ms.date: 11/11/2016
ms.topic: conceptual
---

# NuGet 2.0 Release Notes

[NuGet 1.8 Release Notes](../release-notes/nuget-1.8.md) | [NuGet 2.1 Release Notes](../release-notes/nuget-2.1.md)

NuGet 2.0 was released on June 19, 2012.

## Known Installation Issue
If you are running VS 2010 SP1, you might run into an installation error when attempting to upgrade
NuGet if you have an older version installed.

The workaround is to simply uninstall NuGet and then install it from the VS Extension Gallery.  See
<https://support.microsoft.com/kb/2581019> for more information, or [go directly to the VS hotfix](http://bit.ly/vsixcertfix).

Note: If Visual Studio won't allow you to uninstall the extension (the Uninstall button is disabled),
then you likely need to restart Visual Studio using "Run as Administrator."

## Package restore consent is now active

As described in this [post on package restore consent](http://blog.nuget.org/20120518/package-restore-and-consent.html), NuGet 2.0 will now require that consent be given to enable package restore to go online and download packages. Please ensure that you have provided consent via either the package manager configuration dialog or the EnableNuGetPackageRestore environment variable.

## Group dependencies by target frameworks

Starting with version 2.0, package dependencies can vary based on the framework profile of the target project. This is accomplished using an updated `.nuspec` schema. The `<dependencies>` element can now contain a set of `<group>` elements. Each group contains zero or more `<dependency>` elements and a `targetFramework` attribute. All dependencies inside a group are installed together if the target framework is compatible with the target project framework profile. For example:

```xml
<dependencies>
    <group>
        <dependency id="RouteMagic" version="1.1.0" />
    </group>

    <group targetFramework="net40">
        <dependency id="jQuery" />
        <dependency id="WebActivator" />
    </group>

    <group targetFramework="sl30">
    </group>
</dependencies>
```

Note that a group can contain **zero** dependencies. In the example above, if the package is installed into a project that targets Silverlight 3.0 or later, no dependencies will be installed. If the package is installed into a project that targets .NET 4.0 or later, two dependencies, jQuery and WebActivator, will be installed.  If the package is installed into a project that targets an early version of these 2 frameworks, or any other framework, RouteMagic 1.1.0 will be installed. There is no inheritance between groups. If a project's target framework matches the `targetFramework` attribute of a group, only the dependencies within that group will be installed.

A package can specify package dependencies in either of two formats: the old format of a flat list of `<dependency>` elements, or groups. If the `<group>` format is used, the package cannot be installed into versions of NuGet earlier than 2.0.

Note that mixing the two formats is not allowed. For example, the following snippet is **invalid** and will be rejected by NuGet.

```xml
<dependencies>
    <dependency id="jQuery" />
    <dependency id="WebActivator" />

    <group>
        <dependency id="RouteMagic" version="1.1.0" />
    </group>
</dependencies>
```

## Grouping content files and PowerShell scripts by target framework

In addition to assembly references, content files and PowerShell scripts can also be grouped by target framework. The same folder structure found in the `lib` folder for specifying target framework can  now be applied in the same way to the `content` and `tools` folders. For example:

```
\content
    \net11
        \MyContent.txt
    \net20
        \MyContent20.txt
    \net40
    \sl40
        \MySilverlightContent.html

\tools
    \init.ps1
    \net40
        \install.ps1
        \uninstall.ps1
    \sl40
        \install.ps1
        \uninstall.ps1
```

**Note**: Because `init.ps1` is executed at the solution level and is not dependent on any individual project, it must be placed directly under the `tools` folder. If placed within a framework-specific folder, it will be ignored.

Also, a new feature in NuGet 2.0 is that a framework folder can be *empty*, in which case, NuGet will not add assembly references, add content files or run  PowerShell scripts for the particular framework version. In the example above, the folder `content\net40` is empty.

## Improved tab completion performance
The tab completion feature in the NuGet Package Manager Console has been updated to significantly improve performance. There will be much less delay from the time the tab key is pressed until the suggestion dropdown appears.

## Bug Fixes
NuGet 2.0 includes many bug fixes with an emphasis on package restore consent and performance.
