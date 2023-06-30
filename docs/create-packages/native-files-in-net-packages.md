---
title: Native files in .NET packages
description: How to pack native libraries in .NET packages
author: zivkan
ms.author: zivkan
ms.date: 07/15/2023
ms.topic: conceptual
---

# Including native libraries in .NET packages

This document explains how to create packages that work for projects targeting .NET (5+), where a .NET managed assembly uses [Platform Invoke (P/Invoke)](/dotnet/standard/native-interop/pinvoke) from a .NET managed language project, with the .NET runtime using [probing paths](/dotnet/core/dependency-loading/default-probing) to load and locate the native library.

To support other scenarios, your best option is to package your own [MSBuild props and targets files](../concepts/MSBuild-props-and-targets.md), and handle build and publish targets yourself.
By not using NuGet and the .NET runtime's built in feature,  you need to be sufficiently knowledgeable in MSBuild and the build systems for the relevant project types.
.NET Framework ASP.NET projects in particular work quite differently with regards to many other project types, but .NET Framework (non-SDK style) projects may also not work the same as SDK style projects.
.NET Framework projects using packages with `pacakges.config`, rather than `PackageReference`, have no support for the `ref/` and `runtimes/` features described below.

While NuGet has a [`contentFiles/` feature](../reference/msbuild-targets#including-content-in-a-package), it is not recommended to use this, as it doesn't support per-RID selection, and therefore doesn't participate in the .NET runtime's probing paths features.

## Background

First we'll consider a project referencing packages.
When the app is published for a specific [Runtime Identifier (RID)](/dotnet/core/rid-catalog), then NuGet package assets for that RID will be copied into the output directory.
When the app is published without a specific RID, all RID specific content will be published in subdirectories, and a `{project}.deps.json` file is created that the .NET runtime loads to determine the .

## Understanding NuGet package asset selection

There are two [types of assets](../consume-packages/Package-References-in-Project-Files.md#controlling-dependency-assets) in a NuGet file that are most relevant to this scenario, compile and runtime assets.

### Compile assets

Compile assets are passed to the compiler, and therefore must only contain .NET assemblies.
NuGet will select compile assets from `ref/{tfm}/`, and if that directory does not exist, it will fall back to `lib/{tfm}`, where `{tfm}` is the canonical target framework of the project referencing the package.

The .NET compiler will generate warnings when an assembly targets a different architecture than what the current compilation is targeting, so your package consumers will have the best experience if your `ref/` or `lib/` assemblies target `AnyCPU`.

If your assembly contains a lot of managed code, not just P/Invoke method definitions, you can save space by making the `ref/` assembly a [reference assembly](/dotnet/standard/assembly/reference-assemblies#generating-reference-assemblies), but be sure to include it under `ref/` in your package, not `lib/`.

Therefore, there is no capability to provide different compile-time assemblies for different RIDs, you will need to provide the same API surface for all RIDs, and do runtime checks (for example, throw )

### Runtime assets

Runtime assets are copied to the output directory on build and publish, and the .NET SDK generates the `deps.json` with all the RID-specific asset information.
NuGet will select runtime assets from `runtimes/{rid}/native/` and `runtimes/{rid}/lib/{tfm}/` (see below to understand `sub-path`), and if that doesn't exist, then it will fall back to `lib/{tfm}/`.

When there are both `runtimes/{rid}/native/` and `runtimes/{rid}/lib/{tfm}/` files, NuGet will always select the `runtimes/{rid}/native/` assets, and then the assets from only one of the `runtimes/{rid}/lib/{tfm}/` directories, just like it does for compile assets in `ref/{tfms}/` or `lib/{tfms}/`.

## Putting it together

How to build any binaries native libraries is out of scope of this document.
It is assumed that all the native binaries and the .NET assemblies have been completed and compied to a machine for packing.

Consider a package `Contoso.Native` that provides .NET APIs to `contoso.dll` on Windows, and `contoso.so` on other platforms.
The package contents should be (excluding files required by the [Open Packaging Conventions](https://en.wikipedia.org/wiki/Open_Packaging_Conventions)):

```text
Contoso.Native.1.0.0.nuspec
ref/net8.0/Contoso.Native.dll
runtimes/android/lib/net8.0/Contoso.Native.dll
runtimes/android/native/contoso.so
runtimes/ios/lib/net8.0/Contoso.Native.dll
runtimes/ios/native/contoso.so
runtimes/linux-arm64/lib/net8.0/Contoso.Native.dll
runtimes/linux-arm64/native/contoso.so
runtimes/linux-x64/lib/net8.0/Contoso.Native.dll
runtimes/linux-x64/native/contoso.so
runtimes/maccatalyst-arm64/lib/net8.0/Contoso.Native.dll
runtimes/maccatalyst-arm64/native/contoso.so
runtimes/maccatalyst-x64/lib/net8.0/Contoso.Native.dll
runtimes/maccatalyst-x64/native/contoso.so
runtimes/win-arm64/lib/net8.0/Contoso.Native.dll
runtimes/win-arm64/native/contoso.so
runtimes/win-x64/lib/net8.0/Contoso.Native.dll
runtimes/win-x64/native/contoso.so
```

If all the managed assemblies in `net8.0` directories are the same (there are no `#if` directives in the code), a technique to reduce the number of duplicated `runtimes/{rid}/lib/{tfm}/` files is to split the package into separate native and managed packages.
The native package will be a dependency of the managed package.
For example, a `Contoso.Native.runtime` package that contains all the `runtimes/{rid}/native/` files, and an empty (zero byte) file `ref/net8.0/_._` (zip files only understand files, not directories, so NuGet uses `_._` as a placeholder to say this directory should be considered empty).
The `Contoso.Native` package could then contain `lib/net8.0/Contoso.Native.dll`, and not need separate `ref/` and `runtimes/` assets.

When packing with [NuGet's MSBuild Pack target](../reference/msbuild-targets.md#pack-target), you can include arbitrary files in arbitrary package paths using `Pack="true" PackagePath="{path}"` metadata on MSBuild items.
For example, `<None Include="../cross-compile/linux-x64/contoso.so" Pack="true" PackagePath="runtimes/linux-x64/native/" />`.
