---
title: Select Assemblies Referenced By Projects
description: Make a subset of assemblies in the package available to the compiler, while all assemblies are available at runtime.
author: zivkan
ms.author: zivkan
ms.date: 05/24/2019
ms.topic: conceptual
---

# Select Assemblies Referenced By Projects

Assemblies are used in two different ways during a build. The first is for compile, which allows the package consumer's code to compile against APIs in the assembly, and for Intellisense to give suggestions. The second is runtime, where the assembly is copied to the `bin` directory and is used during program execution. Some package authors would like only their own assemblies (or a subset of their assemblies) available to their package consumers at compile time, but need to provide all their dependencies for runtime. This document looks at ways to achieve this outcome.

## Recommended: One assembly per package

Our recommendation is to have one package per assembly, and package dependencies to other assemblies. When NuGet restores a project, it does asset selection and supports including, excluding, and making private different asset classes. In order to prevent your package's dependencies from becoming compile time assets for anyone using your package, you can make `compile` assets private. In the generated package, that will cause `compile` to be excluded from the dependency. Note that the default private assets when none is supplied is `build;analyzers`. Therefore, you should use `PrivateAssets="compile;build;analyzers"` in your `PackageReference` or `ProjectReference`.

```xml
<ItemGroup>
  <ProjectReference Include="..\OtherProject\OtherProject.csproj" PrivateAssets="compile;build;analyzers" />
  <PackageReference Include="SomePackage" Version="1.2.3" PrivateAssets="compile;build;analyzers" />
</ItemGroup>
```

If you are creating a package from a custom `nuspec` file, rather than letting NuGet auto-generate one for you, your `nuspec` should use the `exclude` XML attribute .

```xml
<dependencies>
  <group targetFramework=".NETFramework4.8">
    <dependency id="OtherProject" version="3.2.1" exclude="Compile,Build,Analyzers" />
    <dependency id="SomePackage" version="1.2.3" exclude="Compile,Build,Analyzers" />
  </group>
</dependencies>
```

There are three reasons why this is the recommended solution.

Firstly, useful assemblies often get referenced by new assemblies/packages. While a utility assembly might be intended to only be used by a single package today, making it tempting to ship both assemblies in a single package, if a second package wants to use the "private" utility assembly in the future, either the utility assembly needs to be moved into a new package and the old package needs to be updated to declare it as a dependency, or the utility package needs to ship in both the existing and the new package. If the assembly ships in two different packages, and a project references both packages, if there are different versions of the utility assembly in the two packages, NuGet will be unable to assist in version management.

Secondly, there may be times that the developers using your package want to also use APIs from your dependencies. For example, consider the package [Microsoft.ServiceHub.Client version 3.0.3078](https://www.nuget.org/packages/Microsoft.ServiceHub.Client/3.0.3078). If you download the package and check the `nuspec` file, you can see that it lists two packages starting with `Microsoft.VisualStudio.` as dependencies, meaning it needs them at runtime, but it also excludes their compile assets. This means that projects using Microsoft.ServiceHub.Client will not have the Visual Studio APIs available in IntelliSense or if they build the project, unless the project explicit installs those packages. And this is the advantage that a package dependency with an exclude asset has. Projects using your package, if they want to use your dependencies as well, they can add a reference to the package to make the APIs available to themselves.

Finally, some package authors have been confused in the past about NuGet's assembly selection for packages supporting more than one target framework when their package also contains multiple assemblies. If your main assembly supports different target frameworks to your utility assembly, it may not be obvious which `lib/` directories to put all of the assemblies into. By separating each package by assembly name, it's more intuitive which `lib/` folders each assembly should go into. Note, this does not mean having `Package1.net48` and `Package1.net6.0` packages. It means having `lib/net48/Package1.dll` and `lib/net6.0/Package6.0` in `Package1`, and `lib/netstandard2.0/Package2.dll` and `lib/net5.0/Package2.dll` in `Package2`. When Nuget restores a project, Nuget will independently do asset selection for the two packages.

Also note that dependency include/exclude assets is only used by projects using PackageReference. Any project installing your package using `packages.config` will install your dependencies and have its APIs available as well. `packages.config` is only supported by Visual Studio's older .NET Framework project templates. SDK style projects, even those targeting .NET Framework, do not support `packages.config`, and therefore do support dependency include/exclude assets.

## Not recommended: Multiple assemblies in one package

`PackageReference` and `packages.config` have different features available. Whether you want to support your package consumers who use `PackageReference`, `packages.config`, or both, changes how you must author your package.

NuGet's MSBuild Pack target does not support automatically including project references in the package. It will only list those referenced projects as package dependencies. There is [an issue on GitHub](https://github.com/NuGet/Home/issues/3891), where community members shared ways they achieved this outcome, which usually involves using `PackagePath` MSBuild item metadata to place files anywhere in the package, as described in [the docs on including content in a package](../reference/msbuild-targets.md#including-content-in-a-package), and using [`SuppressDependenciesWhenPacking` to avoid the project references becoming package dependencies](../reference/msbuild-targets.md#pack-target-inputs). There also exist community developed tools that can be used as an alternative to NuGet's official pack, which support this feature.

### `PackageReference` support

When a package consumer uses `PackageReference`, NuGet selects compile and runtime assets independently, as previously described.

Compile assets prefer `ref/<tfm>/*.dll` (for example `ref/net6.0/*.dll`), but if that does not exist, then it will fall back to `lib/<tfm>/*.dll` (for example `lib/net6.0/*.dll`).

Runtime assets prefer `runtimes/<rid>/lib/<tfm>/*.dll` (for example (`runtimes/win11-x64/lib/net6.0/*.dll`)), but if that does not exist, then it will fall back to `lib/<tfm>/*.dll`.

Since assemblies in `ref\<tfm>\` are not used at runtime, they may be [metadata-only assemblies](https://github.com/dotnet/roslyn/blob/main/docs/features/refout.md) to reduce package size.

### `packages.config` support

Projects using `packages.config` to manage NuGet packages normally add references to all assemblies in the `lib\<tfm>\` directory. The `ref\` directory was added to support `PackageReference` and therefore isn't considered when using `packages.config`. To explicitly set which assemblies are referenced for projects using `packages.config`, the package must use the [`<references>` element in the nuspec file](../reference/nuspec.md#explicit-assembly-references). For example:

```xml
<references>
    <group targetFramework="net45">
        <reference file="MyLibrary.dll" />
    </group>
</references>
```

The MSBuild pack targets don't support the `<references>` element. See [the docs on packing using a .nuspec file](../reference/msbuild-targets.md#packing-using-a-nuspec-file) when using MSBuild pack.

> [!Note]
> `packages.config` project use a process called [ResolveAssemblyReference](https://github.com/Microsoft/msbuild/blob/main/documentation/wiki/ResolveAssemblyReference.md) to copy assemblies to the `bin\<configuration>\` output directory. Your project's assembly is copied, then the build system looks at the assembly manifest for referenced assemblies, then copies those assemblies and recursively repeats for all assemblies. This means that if any of the assemblies loaded only by reflection (`Assembly.Load`, MEF or another dependency injection framework), then it may not be copied to your project's `bin\<configuration>\` output directory despite being in `bin\<tfm>\`. This also means that this only works for .NET assemblies, not for native code called with P/Invoke.

### Supporting both `PackageReference` and `packages.config`

> [!Important]
> If a package contains the nuspec `<references>` element and does not contain assemblies in `ref\<tfm>\`, NuGet will advertise the assemblies listed in the nuspec `<references>` element as both the compile and runtime assets. This means there will be runtime exceptions when the referenced assemblies need to load any other assembly in the `lib\<tfm>\` directory. Therefore, it is important to use both the nuspec `<references>` for `packages.config` support, as well as duplicating assemblies in the `ref/` folder for `PackageReference` support. The `runtimes/` package folder does not need to be used, it was added to the above section for completeness.

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
