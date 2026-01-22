---
title: NuGet project.json file with UWP projects
description: Description of how the project.json file is used to track NuGet dependencies in Universal Windows Platform (UWP) projects.
author: JonDouglas
ms.author: jodou
ms.date: 07/17/2017
ms.topic: concept-article
---

# project.json and UWP

> [!Important]
> This content is deprecated. Projects should use the PackageReference formats.
> Learn how to [migrate your project.json project to PackageReference](./project-json.md#migrate-projectjson-to-packagereference).
> Visual Studio 2026 automatically migrates project.json at solution load time.
> [.NET 10 SDK & NuGet.exe 7.0](../release-notes/NuGet-7.0.md) do not support project.json projects.

This document describes the package structure that employs features in NuGet 3+ (Visual Studio 2015 and later). The `minClientVersion` property of your `.nuspec` can be used to state that you require the features described here by setting it to 3.1.

## Adding UWP support to an existing package

If you have an existing package and you want to add support for UWP applications, then you don’t need to adopt the  packaging format described here. You only need to adopt this format if you require the features it describes and are willing to  work only with clients that have updated to version 3+ of the NuGet client.

## I already target netcore45

If you target `netcore45` already, and you don’t need to take advantage of the features here, no action is needed. `netcore45` packages can be consumed by UWP applications.

## I want to take advantage of Windows 10 specific APIs

In this case you need to add the `uap10.0` target framework moniker (TFM or TxM) to your package. Create a new folder in your package and add the assembly that has been compiled to work with Windows 10 to that folder.

## I don’t need Windows 10 specific APIs, but want new .NET features or don’t have netcore45 already

In this case you would add the `dotnet` TxM to your package. Unlike other TxMs, `dotnet` doesn't imply a surface area or platform. It is stating that your package works on any platform that your dependencies work on. When building a package with the `dotnet` TxM, you are likely to have many more TxM-specific dependencies in your `.nuspec`, as you need to define the BCL packages you depend on, such `System.Text`, `System.Xml`, etc. The locations that those dependencies work on define where your package works.

### How do I find out my dependencies

There are two ways to figure out which dependencies to list:

1. Use the [NuSpec Dependency Generator](https://github.com/onovotny/ReferenceGenerator) **3rd party** tool. The tool automates the process and updates your `.nuspec` file with the dependant packages on build. It is available via a NuGet package, [NuSpec.ReferenceGenerator](https://www.nuget.org/packages/NuSpec.ReferenceGenerator/).

1. (The hard way) Use `ILDasm` to look at your `.dll` to see what assemblies are actually needed at runtime. Then determine which NuGet package they each come from.

See the [`project.json`](project-json.md) topic for details on features that help in the creation of a package that supports the `dotnet` TxM.

> [!Important]
> If your package is intended to work with PCL projects, we highly recommend to create a `dotnet` folder, to avoid warnings and potential compatibility issues.

## Directory structure

NuGet packages using this format have the following well-known folder and behaviors:

| Folder | Behaviors |
| --- | --- |
| Build | Contains MSBuild targets and props files in this folder are integrated differently into the project, but otherwise there is no change. |
| Tools | `install.ps1` and `uninstall.ps1` are not run. `init.ps1` works as it always has. |
| Content | Content is not copied automatically into a user's project. Support for content inclusion in the project is planned for a later release. |
| Lib | For many packages the `lib` works the same way it does in NuGet 2.x, but with expanded options for what names can be used inside it and better logic for picking the correct sub-folder when consuming packages. However, when used in conjunction with `ref`, the `lib` folder contains assemblies that implement the surface area defined by the assemblies in the `ref` folder. |
| Ref | `ref` is an optional folder that contains .NET assemblies defining the public surface (public types and methods) for an application to compile against. The assemblies in this folder may have no implementation, they are purely used to define surface area for the compiler. If the package has no `ref` folder, then the `lib` is both the reference assembly and the implementation assembly. |
| Runtimes | `runtimes` is an optional folder that contains OS specific code, such as CPU architecture and OS specific or otherwise platform-dependent binaries. |

## MSBuild targets and props files in packages

NuGet packages can contain `.targets` and `.props` files which are imported into any MSBuild project that the package is installed into. In NuGet 2.x, this was done by injecting `<Import>` statements into the `.csproj` file, in NuGet 3.0 there is no specific "installation to project" action. Instead the package restore process writes two files `[projectname].nuget.props` and `[projectname].NuGet.targets`.

MSBuild knows to look for these two files and automatically imports them near the beginning and near the end of the project build process. This provides very similar behavior to NuGet 2.x, but with one major difference: *there is no guaranteed order of targets/props files in this case*. However, MSBuild does provide ways to order targets through the `BeforeTargets` and `AfterTargets` attributes of the `<Target>` definition (see [Target Element (MSBuild)](/visualstudio/msbuild/target-element-msbuild).

## Lib and Ref

The behavior of the `lib` folder hasn't changed significantly in NuGet v3. However, all assemblies must be within sub-folders named after a TxM, and can no longer be placed directly under the `lib` folder. A TxM is the name of a platform that a given asset in a package is supposed to work for. Logically these are an extension of the Target Framework Monikers (TFM) e.g. `net45`, `net46`, `netcore50`, and `dnxcore50` are all examples of TxMs (see [Target Frameworks](../reference/target-frameworks.md). TxM can refer to a framework (TFM) as well as other platform-specific surface areas. For example the UWP TxM (`uap10.0`) represents the .NET surface area as well as the Windows surface area for UWP applications.

An example lib structure:

```
lib
├───net40
│       MyLibrary.dll
└───wp81
        MyLibrary.dll
```

The `lib` folder contains assemblies that are used at runtime. For most packages a folder under `lib` for each of the target TxMs is all that is required.

## Ref

There are sometimes cases where a different assembly should be used during compilation (.NET Reference Assemblies do this today). For those cases, use a top-level folder called `ref` (short for "Reference Assemblies").

Most package authors don't require the `ref` folder. It is useful for packages that need to provide a consistent surface area for compilation and IntelliSense but then have different implementation for different TxMs. The biggest use case of this are the `System.*` packages that are being produced as part of shipping .NET Core on NuGet. These packages have various implementations that are being unified by a consistent set of ref assemblies.

Mechanically, the assemblies included in the `ref` folder are the reference assemblies being passed to the compiler. For those of you who have used csc.exe these are the assemblies we are passing to the [C# /reference option](/dotnet/articles/csharp/language-reference/compiler-options/reference-compiler-option) switch.

The structure of the `ref` folder is the same as `lib`, for example:

```
└───MyImageProcessingLib
        ├───lib
        │   ├───net40
        │   │       MyImageProcessingLibrary.dll
        │   │
        │   ├───net451
        │   │       MyImageProcessingLibrary.dll
        │   │
        │   └───win81
        │           MyImageProcessingLibrary.dll
        │
        └───ref
            ├───net40
            │       MyImageProcessingLibrary.dll
            │
            └───portable-net451-win81
                    MyImageProcessingLibrary.dll
```

In this example the assemblies in the `ref` directories would all be identical.

## Runtimes

The runtimes folder contains assemblies and native libraries required to run on specific "runtimes", which are generally defined by Operating System and CPU architecture. These runtimes are identified using [Runtime Identifiers (RIDs)](/dotnet/core/rid-catalog) such as `win`, `win-x86`, `win7-x86`, `win8-64`, etc.

## Native helpers to use platform-specific APIs

The following example shows a package that has a purely managed implementation for several platforms, but uses native helpers on Windows 8 where it can call into Windows 8-specific native APIs.

```
└───MyLibrary
        ├───lib
        │   └───net40
        │           MyLibrary.dll
        │
        └───runtimes
            ├───win8-x64
            │   ├───lib
            │   │   └───net40
            │   │           MyLibrary.dll
            │   │
            │   └───native
            │           MyNativeLibrary.dll
            │
            └───win8-x86
                ├───lib
                │   └───net40
                │           MyLibrary.dll
                │
                └───native
                        MyNativeLibrary.dll
```

Given the above package the following things happen:

- When not on Windows 8 the `lib/net40/MyLibrary.dll` assembly is used.

- When on Windows 8 the `runtimes/win8-<architecture>/lib/MyLibrary.dll` is used and the `native/MyNativeHelper.dll` is copied to the output of your build.

In the example above the `lib/net40` assembly is purely managed code, whilst the assemblies in the runtimes folder will p/invoke into the native helper assembly to call APIs specific to Windows 8.

Only a single `lib` folder is ever be picked, so if there is a runtime specific folder it's chosen over non-runtime specific `lib`. The native folder is additive, if it exists it's copied to the output of the build.

## Managed wrapper

Another way to use runtimes is to ship a package that is purely a managed wrapper over a native assembly. In this scenario you create a package like the following:

```
└───MyLibrary
        └───runtimes
            ├───win8-x64
            │   ├───lib
            │   │   └───net451
            │   │           MyLibrary.dll
            │   │
            │   └───native
            │           MyImplementation.dll
            │
            └───win8-x86
                ├───lib
                │   └───net451
                │           MyLibrary.dll
                │
                └───native
                        MyImplementation.dll
```

In this case there is no top-level `lib` folder as that folder as there is no implementation of this package that doesn't rely on the corresponding native assembly. If the managed assembly, `MyLibrary.dll`, was exactly the same in both of these cases then we could put it in a top level `lib` folder, but because the lack of a native assembly doesn't cause the package to fail installing if it was installed on a platform that wasn't win-x86 or win-x64 then the top level lib would be used but no native assembly would be copied.

## Authoring packages for NuGet 2 and NuGet 3

If you want to create a package that can be consumed by projects using `packages.config` as well as packages using `project.json` then the following apply:

- Ref and runtimes only work on NuGet 3. They are both ignored by NuGet 2.

- You cannot rely on `install.ps1` or `uninstall.ps1` to function. These files execute when using `packages.config`, but are ignored with `project.json`. So your package needs to be usable without them running. `init.ps1` still runs on NuGet 3.

- Targets and Props installation is different, so make sure that your package works as expected on both clients.

- Subdirectories of lib must be a TxM in NuGet 3. You cannot place libraries at the root of the `lib` folder.

- Content is not copied automatically with NuGet 3. Consumers of your package could copy the files themselves or use a tool like a task runner to automate copying the files.

- Source and config file transforms are not run by NuGet 3.

If you are supporting NuGet 2 and 3 then your `minClientVersion` should be the lowest version of NuGet 2 client that your package works on. In the case of an existing package it shouldn't need to change.
