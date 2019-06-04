---
title: Explicit Assmbly References
description: Make a subset of assemblies in the package available to the compiler, while all assemblies are available at runtime.
author: zivkan
ms.author: zivkan
ms.date: 05/24/2019
ms.topic: conceptual
---

# Explicit Assembly References

Explicit assembly references allows a subset of assemblies to be used for intellisense and compiling, while all assemblies are available at run-time. Projects that use `packages.config` work differently to projects that use `PackageReference` and as a result, package authors need to take care to create the package to be compatible with both types of usage.

> [!Note]
> Explicit assembly references are related to .NET assemblies. It is not a method to distribute native assemblies that are P/Invoked by a managed assembly.

## `PackageReference` support

When a project uses a package with `PackageReference` and the package contains a `ref\<tfm>\` directory, NuGet will advertise those assembles as compile-time assets, while the `lib\<tfm>\` assemblies are advertised as runtime assets. Assemblies in `ref\<tfm>\` are not used at runtime. This means it is necessary for any assembly in `ref\<tfm>\` to have a matching assembly in either `lib\<tfm>\` or a relevant `runtime\` directory, otherwise runtime errors will likely occur. Since assemblies in `ref\<tfm>\` are not used at runtime, they may optionally be [metadata-only assemblies](https://github.com/dotnet/roslyn/blob/master/docs/features/refout.md) to reduce package size.

> [!Important]
> If a package contains the nuspec `<references>` element (used by `packages.config`, see below) and does not contain assemblies in `ref\<tfm>\`, NuGet will advertise the assemblies listed in the nuspec `<references>` element as both the compile and runtime assets. This means there will be runtime exceptions when the referenced assemblies need to load any other assembly in the `lib\<tfm>\` directory.

> [!Note]
> If the package contains a `runtime\` directory, NuGet may not use the assets in the `lib\` directory.

## `packages.config` support

Projects using `packages.config` to use NuGet packages normally add references to all assemblies in the `lib\<tfm>\` directory. The `ref\` directory was a new feature for `PackageReference` and therefore isn't considered when using `packages.config`. In order to explicitly set which assemblies are referenced for projects using `packages.config`, the package must use the [`<references>` element in the nuspec file](..\reference\nuspec.md#explicit-assembly-references). For example:

```xml
<references>
    <group targetFramework="net45">
        <reference file="MyLibrary.dll" />
    </group>
</references>
```

> [!Note]
> The details of how assemblies are copied to the `bin\<configuration>\` directory is though a process called ???. Your project's assembly is copied, then the build system looks at the assembly manifest for referenced assemblies, then copies those assemblies and recursively repeats for all assemblies. This means if your `lib\<tfm>` directory contains 3 assemblies, `a.dll`, `b.dll`, and `c.dll` and you specify `a.dll` as an explicit assembly reference, `a.dll` has an assembly reference to `b.dll`, but `c.dll` is loaded though MEF or `Assembly.Load`, then `c.dll` may not be selected as part of ??? and therefore may not be copied to `bin\<configuration>\` despite being in `bin\<tfm>\`.

# Example

I wish my package to contain three assemblies, `MyLib.dll`, `MyHelpers.dll` and `MyUtilities.dll`, which are targeting the .NET Framework 4.7.2. `MyUtilities.dll` contains classes intended to be used only by the other two assemblies, so I don't want to make those classes available in intellisense or at compile time to projects using my package. My `nuspec` file will contain the following XML elements:

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
