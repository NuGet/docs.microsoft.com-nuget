---
title: project.json impact on NuGet package authors
description: Details on how the implementation of project.json in NuGet 3.x affects package authors, such as unsupported features, content, and package format.
author: JonDouglas
ms.author: jodou
ms.date: 01/18/2018
ms.topic: conceptual
---

# Impact of project.json when creating packages

> [!Important]
> This content is deprecated. Projects should use the PackageReference formats.
> Learn how to [migrate your project.json project to PackageReference](#migrate-projectjson-to-packagereference).
> Visual Studio 2026 automatically migrates project.json at solution load time.
> [.NET 10 SDK & NuGet.exe 7.0](../release-notes/NuGet-7.0.md) do not support project.json projects.

The `project.json` system used in NuGet 3+ affects package authors in several ways as described in the following sections.

## Changes affecting existing packages usage

Traditional NuGet packages support a set of features that are not carried over to the transitive world.

### Install and uninstall scripts are ignored

The transitive restore model, described in [Dependency resolution](../concepts/dependency-resolution.md#dependency-resolution-with-packagereference), does not have a concept of "package install time". A package is either present or not present, but there is no consistent process that occurs when a package is installed.

Also, install scripts were supported only in Visual Studio. Other IDEs had to mock the Visual Studio extensibility API to attempt to support such scripts, and no support was available in common editors and command-line tools.

### Content transforms are not supported

Similar to install scripts, transforms run on package install and are typically not idempotent. Since there is no install time anymore, XDT Transform and similar features are not supported, and are ignored if such a package is used in a transitive scenario.

### Content

Traditional NuGet packages are shipping content files such as source code and configuration files. There are used typically in two scenarios:

1. Initial files dropped into the project so the user can edit them at a later time. The common example is default configuration files.

1. Content files used as companions to the assemblies installed in the project. The example here would be a logo image used by an assembly.

Support for content is currently disabled for similar reasons for scripts and transforms, but we are in the process of designing support for content.

Content files can still be carried inside the packages, and are ignored currently, however the end user can still copy them into the right spot.

You can see one of the proposals for bringing back content files, and follow its progress, here: [https://github.com/NuGet/Home/issues/627](https://github.com/NuGet/Home/issues/627).

## Impact for package authors

Packages using the above features would have to use a different mechanism. The most commonly useful mechanism for this would be the MSBuild targets/props that continues to get fully supported. The build system can choose to pick up other conventions in the package. This is how MSBuild targets are supported as well as Roslyn analyzers. It is possible to build packages that supports targets and analyzers for `packages.config` and `project.json` scenarios.

Packages that attempt to modify the project to ease startup typically work in a very limited set of scenarios, and should instead provide a readme, or guidance on how to use the package.

Most existing packages should not need to use the package format described below.

The format enables native content as a first class scenario. This means that managed assemblies depend on close to hardware implementations to ship binary implementations alongside the managed assemblies based on the target platform. For example System.IO.Compression package is utilizing this technology. [https://www.nuget.org/packages/System.IO.Compression](https://www.nuget.org/packages/System.IO.Compression)

In summary if the functionality above is not absolutely necessary, we recommend sticking with the existing package format, as the format described here is supported only by NuGet 3.x+.

It would be possible to build packages to work for both `packages.config` and `project.json` scenarios through shimming, however it's often simpler to just structure the packages the traditional way, without the deprecated features mentioned above.

## 3.x package format

The 3.x package format allows for several additional features beyond NuGet 2.x:

1. Defining a reference assembly used for compilation and a set of implementation assemblies used for runtime on different platforms/devices. Which allows you to take advantage of platform specific APIs while providing a common surface area for your consumers. Specifically this makes writing intermediate portable libraries easier.

1. Allows packages to pivot on platforms e.g. operating systems or CPU architecture.

1. Allows for separation of platform specific implementations to companion packages.

1. Support Native dependencies as a first class citizen.
