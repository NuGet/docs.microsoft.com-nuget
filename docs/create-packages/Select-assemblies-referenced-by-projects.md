---
title: Select Assemblies Referenced By Projects
description: Make a subset of assemblies in the package available to the compiler, while all assemblies are available at runtime.
author: zivkan
ms.author: zivkan
ms.date: 05/24/2019
ms.topic: conceptual
---

# Select Assemblies Referenced By Projects

Assemblies are used in two different ways during a build. The first is for compile, which allows the package consumer's code to compile against APIs in the assembly, and for Intellisense to give suggestions. The second is runtime, where the assembly is copied to the `bin` directory and is used at runtime.

## Recommendation: Package dependencies and include/exclude assets

Our recommendation is to have one package per assembly, in which case you can use `PrivateAssets="compile"` in your `PackageReference` or `ProjectReference`. This provides more flexibility and minimizes risk that two different packages will contain different versions of the same assembly, preventing NuGet from being able to manage versions.

```xml
<ItemGroup>
  <ProjectReference Include="..\OtherProject\OtherProject.csproj" PrivateAssets="compile" />
  <PackageReference Include="SomePackage" Version="1.2.3" PrivateAssets="compile" />
</ItemGroup>
```

This gives your package consumers the flexibility to directly reference the package to get the compile time assets.

## Not recommended: Multiple assemblies in one package

`PackageReference` and `packages.config` have different features available. Whether you want to support your package consumers who use `PackageReference`, `packages.config`, or both, changes how you must author your package.

NuGet's MSBuild Pack target does not support automatically including project references in the package. There is [an issue on GitHub](https://github.com/NuGet/Home/issues/3891), where community members shared ways they achieved this outcome, which usually involves using `PackagePath` MSBuild item metadata to place files anywhere in the package, as described in the the docs on [including content in a package](../reference/msbuild-targets.md#including-content-in-a-package), and using [`SuppressDependenciesWhenPacking` to avoid the project references becoming package dependencies](../reference/msbuild-targets.md#pack-target-inputs). Also consider community developed tools such as [NuGetizer](https://github.com/devlooped/nugetizer).

### `PackageReference` support

When a package consumer uses `PackageReference`, NuGet selects compile and runtime assets independently.

Compile assets prefer `ref/<tfm>/*.dll`, but if that does not exist, then it will fall back to `lib/<tfm>/*.dll`.

Runtime assets prefer `runtimes/<rid>/lib/<tfm>/*.dll`, but if that does not exist, then it will fall back to `lib/<tfm>/*.dll`.

Since assemblies in `ref\<tfm>\` are not used at runtime, they may be [metadata-only assemblies](https://github.com/dotnet/roslyn/blob/main/docs/features/refout.md) to reduce package size.

### `packages.config` support

Projects using `packages.config` to manage NuGet packages normally add references to all assemblies in the `lib\<tfm>\` directory. The `ref\` directory was added to support `PackageReference` and therefore isn't considered when using `packages.config`. To explicitly set which assemblies are referenced for projects using `packages.config`, the package must use the [`<references>` element in the nuspec file](../reference/nuspec.md#explicit-assembly-references). For example:

> [!Note]
> Explicit assembly references are related to .NET assemblies. It is not a method to distribute native assemblies that are P/Invoked by a managed assembly.

```xml
<references>
    <group targetFramework="net45">
        <reference file="MyLibrary.dll" />
    </group>
</references>
```

The MSBuild pack targets don't support the `<references>` element. See the docs on [packing using a .nuspec file](../reference/msbuild-targets.md#packing-using-a-nuspec-file) when using MSBuild pack.

> [!Note]
> `packages.config` project use a process called [ResolveAssemblyReference](https://github.com/Microsoft/msbuild/blob/main/documentation/wiki/ResolveAssemblyReference.md) to copy assemblies to the `bin\<configuration>\` output directory. Your project's assembly is copied, then the build system looks at the assembly manifest for referenced assemblies, then copies those assemblies and recursively repeats for all assemblies. This means that if any of the assemblies loaded only by reflection (`Assembly.Load`, MEF or another dependency injection framework), then it may not be copied to your project's `bin\<configuration>\` output directory despite being in `bin\<tfm>\`.

### Supporting both `PackageReference` and `packages.config`

> [!Important]
> If a package contains the nuspec `<references>` element and does not contain assemblies in `ref\<tfm>\`, NuGet will advertise the assemblies listed in the nuspec `<references>` element as both the compile and runtime assets. This means there will be runtime exceptions when the referenced assemblies need to load any other assembly in the `lib\<tfm>\` directory. Therefore, it is important to use both the nuspec `<references>` for `packages.config` support, as well as duplicating assemblies in the `ref/` folder for `PackageReference` support.


#### Example

My package will contain three assemblies, `MyLib.dll`, `MyHelpers.dll` and `MyUtilities.dll`, which are targeting the .NET Framework 4.7.2. `MyUtilities.dll` contains classes intended to be used only by the other two assemblies, so I don't want to make those classes available in IntelliSense or at compile time to projects using my package. My `nuspec` file needs to contain the following XML elements:

```xml
<references>
    <group targetFramework="net472">
        <reference file="MyLib.dll" />
        <reference file="MyHelpers.dll" />
    </group>
</references>
```

I need to ensure my package contents are:

```text
lib\net472\MyLib.dll
lib\net472\MyHelpers.dll
lib\net472\MyUtilities.dll
ref\net472\MyLib.dll
ref\net472\MyHelpers.dll
```
