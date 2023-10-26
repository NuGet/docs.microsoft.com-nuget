---
title: Native files in .NET packages
description: How to pack native libraries in .NET packages
author: zivkan
ms.author: zivkan
ms.date: 09/26/2023
ms.topic: conceptual
---

# Including native libraries in .NET packages

This document explains how to create packages that work for projects targeting .NET (5+), where a .NET managed assembly uses [Platform Invoke (P/Invoke)](/dotnet/standard/native-interop/pinvoke) from a .NET managed language project, with the .NET runtime using [probing paths](/dotnet/core/dependency-loading/default-probing) to load and locate the native library.

If you wish to support .NET Framework with your package, see the [section on projects targeting .NET Framework](#projects-targeting-net-framework).
In general, your best option is to package your own [MSBuild props and targets files](../concepts/MSBuild-props-and-targets.md), and handle build and publish targets yourself.
By not using NuGet and the .NET runtime's built in feature,  you need to be sufficiently knowledgeable in MSBuild and the build systems for the relevant project types.

While NuGet has a [`contentFiles/` feature](../reference/msbuild-targets.md#including-content-in-a-package), it is not recommended to use this, as it doesn't support per-RID selection, and therefore doesn't participate in the .NET runtime's probing paths features.

## Background

First we'll consider a project referencing packages.
When the app is published for a specific [Runtime Identifier (RID)](/dotnet/core/rid-catalog), then NuGet package assets for that RID will be copied into the output directory.
When the app is published without a specific RID, all RID specific content will be published in subdirectories, and a `{project}.deps.json` file is created that the .NET runtime loads to determine which directory to use as a probing path.

Also, be aware that .NET 8 introduces [breaking changes regarding how it determines compatible RIDs from the `.deps.json` file](/dotnet/core/compatibility/deployment/8.0/rid-asset-list).

## Understanding NuGet package asset selection

There are three types of assets in a NuGet file that are most relevant to this scenario: compile, runtime, and native assets.
For a complete list of asset types, see the docs on [controlling dependency assets in `PackageReference` projects](../consume-packages/Package-References-in-Project-Files.md#controlling-dependency-assets).

|Asset type|Short Description|
|--|--|
|[compile](#compile-assets)|Managed assemblies passed to the compiler. `refs/{tfm}/` if it exists, otherwise `lib/{tfm}/`.|
|[runtime](#runtime-assets)|Managed assemblies copied to the output directory. `runtimes/{rid}/lib/{tfm}/` if it exists, otherwise `lib/{tfm}/`.|
|[native](#native-assets)|Native libraries copied to the output directory. `runtimes/{rid}/native/`.|

In all cases, NuGet chooses the "most compatible" directory for a given RID and Target Framework, and only the files in that directory are copied.
Therefore, you cannot put common assets in an inherited RID (such as `any`), because if any other RID is a better match, the `any` RID contents will not be considered.

To inspect the implementation of NuGet's asset selection, see [`NuGet.Client`'s `ManagedCodeConventions.cs` file](https://github.com/NuGet/NuGet.Client/blob/dev/src/NuGet.Core/NuGet.Packaging/ContentModel/ManagedCodeConventions.cs).

### Compile assets

Compile assets are passed to the compiler, and therefore must only contain .NET assemblies.
NuGet will select compile assets from `ref/{tfm}/`, and if that directory does not exist, it will fall back to `lib/{tfm}`, where `{tfm}` is the target framework that is the closest match the project referencing the package.
However, in this scenario of creating a .NET package with native libraries, it is recommended to use `ref/` because if `lib/` is used, NuGet will think the package is compatible with projects using `packages.config`, but those projects will fail at runtime because the native files will not be copied.

The .NET compiler will generate warnings when an assembly targets a different architecture than what the current compilation is targeting, so your package consumers will have the best experience if your `ref/` or `lib/` assemblies target `AnyCPU`.

If your assembly contains a lot of managed code, not just P/Invoke method definitions, you can save space by making the `ref/` assembly a [reference assembly](/dotnet/standard/assembly/reference-assemblies#generating-reference-assemblies), but be sure to include it under `ref/` in your package, not `lib/`.

Therefore, there is no capability to provide different compile-time assemblies for different RIDs, you will need to provide the same API surface for all RIDs, and do runtime checks (for example, throw `PlatformNotSupportedException`).

### Runtime assets

Runtime assets are .NET managed assemblies copied to the output directory on build and publish, and the .NET SDK generates the `deps.json` with all the RID-specific asset information.
NuGet will select runtime assets from `runtimes/{rid}/lib/{tfm}/`, and if that doesn't exist, then it will fall back to `lib/{tfm}/`.
The best `{rid}` directory is selected first, followed by `{tfm}`.
However, in this scenario of creating a .NET package with native libraries, it is recommended to use `runtimes/` because if `lib/` is used, NuGet will think the package is compatible with projects using `packages.config`, but those projects will fail at runtime because the native assets will not be copied.

### Native assets

Native assets are also copied to the output directory on build and publish, and are present in the `deps.json` file.
NuGet will select native assets from the `runtimes/{rid}/native/` directory.

> [!NOTE]
> The [.NET SDK flattens any directory structure](https://github.com/dotnet/sdk/issues/9643) under `runtimes/{rid}/native/` when copying to the output directory.

## Putting it together

How to build any binaries native libraries is out of scope of this document.
It is assumed that all the native binaries and the .NET assemblies have been completed and copied to a machine for packing.

When packing with [NuGet's MSBuild Pack target](../reference/msbuild-targets.md#pack-target), you can include arbitrary files in arbitrary package paths using `Pack="true" PackagePath="{path}"` metadata on MSBuild items.
For example, `<None Include="../cross-compile/linux-x64/libcontoso.so" Pack="true" PackagePath="runtimes/linux-x64/native/" />`.

Consider a package `Contoso.Native` that provides .NET APIs to `libcontoso.dll` on Windows, `libcontoso.dylib` on OSX, and `libcontoso.so` on Linux, using `[DllImport("contoso")]`.
The package contents should be (excluding files required by the [Open Packaging Conventions](https://en.wikipedia.org/wiki/Open_Packaging_Conventions))

### Example 1

If `Contoso.Native` is built as AnyCPU:

```text
Contoso.Native.1.0.0.nuspec
ref/net8.0/Contoso.Native.dll
runtimes/any/lib/net8.0/Contoso.Native.dll
runtimes/linux-arm64/native/libcontoso.so
runtimes/linux-x64/native/libcontoso.so
runtimes/osx-arm64/native/libcontoso.dylib
runtimes/osx-x64/native/libcontoso.dylib
runtimes/win-arm64/native/contoso.dll
runtimes/win-x64/native/contoso.dll
```

While the `ref/` and `runtimes/any/lib` copies of `Contoso.Native.dll` could be deduplicated into a single `lib/net8.0/Contoso.Native.dll` file, this is not recommended because NuGet will believe this package is compatible with projects using `packages.config`, and these projects will fail at runtime when the native `contoso.dll` is not found.

### Example 2

If `Contoso.Native` has any differences between architectures.
For example, but not limited to, using `#if` to compile difference code for different RIDs, or using `<PlatformTarget>` (or anything else that causes `-platform:` to be passed to the compiler) with a value other than `AnyCPU`.

```text
Contoso.Native.1.0.0.nuspec
ref/net8.0/Contoso.Native.dll
runtimes/linux-arm64/lib/net8.0/Contoso.Native.dll
runtimes/linux-arm64/native/libcontoso.so
runtimes/linux-x64/lib/net8.0/Contoso.Native.dll
runtimes/linux-x64/native/libcontoso.so
runtimes/osx-arm64/lib/net8.0/Contoso.Native.dll
runtimes/osx-arm64/native/libcontoso.dylib
runtimes/osx-x64/lib/net8.0/Contoso.Native.dll
runtimes/osx-x64/native/libcontoso.dylib
runtimes/win-arm64/lib/net8.0/Contoso.Native.dll
runtimes/win-arm64/native/contoso.dll
runtimes/win-x64/lib/net8.0/Contoso.Native.dll
runtimes/win-x64/native/contoso.dll
```

## Projects Targeting .NET Framework

.NET has evolved over time, causing project and package incompatibility when certain features are used.
What was described above assumes that the project using the package targets .NET 5 or higher, or .NET Core (`netcoreapp`), which uses the maximum features to simplify the package contents as much as possible.
Projects targeting .NET Standard are not particularly relevant to this document, as those projects can only be class libraries, which cannot be run directly.
However, projects targeting .NET Framework can use either SDK style or non-SDK style projects, and non-SDK style projects may use either `PackageReference` or `packages.config` to use NuGet packages.
All of these combinations have different levels of support, and impose constraints on package authors who wish to support .NET Framework projects using the package.

Our recommendation is to split the package into 3 packages.
The first is a meta-package (a package with no contents/assets, only dependencies to other packages), that has dependencies to one of the other two packages, depending on target framework.
The second package contains the .NET 5+ `ref/*` and `runtimes/*` assets as described above.
The last package will contain the .NET Framework assets, and will not use the `ref/*` or `runtime/*` conventions.
This is due to the build system having (partial) support for these NuGet conventions, but the runtime working sufficiently differently that it's easier to avoid it, to maximize project compatibility.

The package with .NET Framework native libraries should have custom [MSBuild targets and props files](../concepts/MSBuild-props-and-targets.md), under `buildTransitive/<tfm>/<package ID>.[props|targets]`.
For example, `buildTransitive/net472/Contoso.Native.targets`.
To support `packages.config` projects, also have a `build/<tfm>/<package ID>.[props|targets]` which imports the `buildTransitive` file.
This MSBuild script script should copy the native assemblies to subdirectories.

Finally, the managed library that uses P/Invoke to call into the native library, it should have runtime checks to determine what platform the current executable is running in (x86, x64, or ARM64), and then P/Invoke into the native assembly in a correct directory.
This is because .NET Framework supports the concept of "Any CPU" executables, which will execute (JIT compile) as x86 on x86 processors, and x64 otherwise (x64 or ARM64 processors).
.NET Core removed this "Any CPU" executable concept, and separate x64, x86, and ARM64 executables need to be compiled and deployed explicitly.
Therefore, managed libraries with P/Invoke do not need this runtime check on .NET Core runtimes, but do on .NET Framework runtimes.

If the .NET Framework project consuming the library with managed concents explicitly opts out of "Any CPU", then these complications are not relevant, and the package author can use `runtimes/win-x64/lib/net48/contoso.dll`, for example.
However, this is only feasible if the package is private and only used in projects that the package author controls, or at least has sufficient influence over.

### SDK style projects targeting .NET Framework

If you test an SDK style project that targets .NET Framework with the `runtimes/*` convention, using .NET Framework TFM, you will see that the .NET SDK will copy the x86 assets when the build or publish does not specific a RID.
However, as described above, if the x86 `.exe` is running on an x64 or ARM64 CPU, the runtime will JIT compile x64 instructions, and the runtime will fail to execute the x86 native assembly at the moment the managed library first P/Invokes the native library.

However, if `dotnet build -r win-x64` or `dotnet build -r win-arm64` (or the `publish` equivalents) are used, then the executable will be platform specific, failing to run on different CPU architectures, and the build will copy the correct native libraries.

### Non-SDK style projects targeting .NET Framework with `PackageReference`

If you test a non-SDK style `PackageReference` project with a package using the `runtimes/*` convention, the following behavior occurs.
If the project's configuration targets the "Any CPU" platform, the build will not copy any of the RID specific assets from the package, and the app will fail at runtime.
Using Visual Studio's Configuration Manager, the .NET projects can be changed to target x64, x86, or ARM64, instead of "Any CPU", in which case the app will work correctly at runtime.

### `packages.config`

NuGet's original restore style, `packages.config`, does not support `ref/*` or `runtimes/*` conventions.
Therefore, package authors must create their own MSBuild props and targets file to support projects still using `packages.config`.
