---
title: Select Assemblies Referenced By Projects
description: Make a subset of assemblies in the package available to the compiler, while all assemblies are available at runtime.
author: zivkan
ms.author: zivkan
ms.date: 05/24/2019
ms.topic: conceptual
---

# Select Assemblies Referenced By Projects

Explicit assembly references allows a subset of assemblies to be used for IntelliSense and compiling, while all assemblies are available at run-time. `PackageReference` and `packages.config` work differently, and as a result package authors need to take care to create the package to be compatible with both project types.

> [!Note]
> Explicit assembly references are related to .NET assemblies. It is not a method to distribute native assemblies that are P/Invoked by a managed assembly.

## `PackageReference` support

When a project uses a package with `PackageReference` and the package contains a `ref\<tfm>\` directory, NuGet will classify those assembles as compile-time assets, while the `lib\<tfm>\` assemblies are classified as runtime assets. Assemblies in `ref\<tfm>\` are not used at runtime. This means it is necessary for any assembly in `ref\<tfm>\` to have a matching assembly in either `lib\<tfm>\` or a relevant `runtime\` directory, otherwise runtime errors will likely occur. Since assemblies in `ref\<tfm>\` are not used at runtime, they may be [metadata-only assemblies](https://github.com/dotnet/roslyn/blob/main/docs/features/refout.md) to reduce package size.

> [!Important]
> If a package contains the nuspec `<references>` element (used by `packages.config`, see below) and does not contain assemblies in `ref\<tfm>\`, NuGet will advertise the assemblies listed in the nuspec `<references>` element as both the compile and runtime assets. This means there will be runtime exceptions when the referenced assemblies need to load any other assembly in the `lib\<tfm>\` directory.

> [!Note]
> If the package contains a `runtime\` directory, NuGet may not use the assets in the `lib\` directory.

## `packages.config` support

Projects using `packages.config` to manage NuGet packages normally add references to all assemblies in the `lib\<tfm>\` directory. The `ref\` directory was added to support `PackageReference` and therefore isn't considered when using `packages.config`. To explicitly set which assemblies are referenced for projects using `packages.config`, the package must use the [`<references>` element in the nuspec file](../reference/nuspec.md#explicit-assembly-references). For example:

```xml
<references>
    <group targetFramework="net45">
        <reference file="MyLibrary.dll" />
    </group>
</references>
```

> [!Note]
> `packages.config` project use a process called [ResolveAssemblyReference](https://github.com/Microsoft/msbuild/blob/main/documentation/wiki/ResolveAssemblyReference.md) to copy assemblies to the `bin\<configuration>\` output directory. Your project's assembly is copied, then the build system looks at the assembly manifest for referenced assemblies, then copies those assemblies and recursively repeats for all assemblies. This means that if any of the assemblies in your `lib\<tfm>\` directory are not listed in any other assembly's manifest as a dependency (if the assembly is loaded at runtime using `Assembly.Load`, MEF or another dependency injection framework), then it may not be copied to your project's `bin\<configuration>\` output directory despite being in `bin\<tfm>\`.

## Example

My package will contain three assemblies, `MyLib.dll`, `MyHelpers.dll` and `MyUtilities.dll`, which are targeting the .NET Framework 4.7.2. `MyUtilities.dll` contains classes intended to be used only by the other two assemblies, so I don't want to make those classes available in IntelliSense or at compile time to projects using my package. My `nuspec` file needs to contain the following XML elements:

```xml
<references>
    <group targetFramework="net472">
        <reference file="MyLib.dll" />
        <reference file="MyHelpers.dll" />
    </group>
</references>
```

and the files in the package will be:

```text
lib\net472\MyLib.dll
lib\net472\MyHelpers.dll
lib\net472\MyUtilities.dll
ref\net472\MyLib.dll
ref\net472\MyHelpers.dll
```
