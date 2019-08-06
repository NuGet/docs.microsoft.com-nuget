---
title: How to publish NuGet symbol packages using the new symbol package format '.snupkg'| Microsoft Docs
author: cristinamanu
ms.author:
- cristinamanu
- kraigb
manager: skofman
ms.date: 10/30/2018
ms.topic: reference
ms.prod: nuget
ms.technology: null
description: How to create NuGet symbol packages (snupkg).
keywords: NuGet symbol packages, NuGet package debugging, supporting NuGet debugging, package symbols, symbol package conventions
ms.reviewer:
- anangaur
- karann
---


# Creating symbol packages (.snupkg)

Symbol packages allow you to improve the debugging experience of your NuGet packages.

## Prerequisites

[nuget.exe v4.9.0 or above](https://www.nuget.org/downloads) or [dotnet.exe v2.2.0 or above](https://www.microsoft.com/net/download/dotnet-core/2.2), which implement the required [NuGet protocols](../api/nuget-protocols.md).

## Creating a symbol package

You can create a snupkg symbol package using dotnet.exe, NuGet.exe, or MSBuild. If you're using NuGet.exe, you can use the following commands to create a .snupkg file in addition to the .nupkg file:

```
nuget pack MyPackage.nuspec -Symbols -SymbolPackageFormat snupkg

nuget pack MyPackage.csproj -Symbols -SymbolPackageFormat snupkg
```

If you're using dotnet.exe or MSBuild, use the following steps to create a .snupkg file in addition to the .nupkg file:

1. Add the following properties to your .csproj file:

    ```xml
    <PropertyGroup>
      <IncludeSymbols>true</IncludeSymbols>
      <SymbolPackageFormat>snupkg</SymbolPackageFormat>
    </PropertyGroup>
    ```

1. Pack your project with `dotnet pack MyPackage.csproj` or `msbuild -t:pack MyPackage.csproj`.

The [`SymbolPackageFormat`](/dotnet/core/tools/csproj#symbolpackageformat) property can have one of two values: `symbols.nupkg` (the default) or `snupkg`. If the [`SymbolPackageFormat`](/dotnet/core/tools/csproj#symbolpackageformat) property is not specified, a legacy symbol package will be created.

> [!Note]
> The legacy format `.symbols.nupkg` is still supported but only for compatibility reasons (see [Legacy Symbol Packages](Symbol-Packages.md)). NuGet.org's symbol server only accepts the new symbol package format - `.snupkg`.

## Publishing a symbol package

1. For convenience, first save your API key with NuGet (see [publish a package](../nuget-org/publish-a-package.md)).

    ```cli
    nuget SetApiKey Your-API-Key
    ```

1. After publishing your primary package to nuget.org, push the symbol package as follows.

    ```cli
    nuget push MyPackage.snupkg
    ```

1. You can also push both primary and symbol packages at the same time using the below command. Both .nupkg and .snupkg files need to be present in the current folder.

    ```cli
    nuget push MyPackage.nupkg
    ```

NuGet will publish both packages to nuget.org. `MyPackage.nupkg` will be published first, followed by `MyPackage.snupkg`.

> [!Note]
> If the symbol package isn't published, check that you've configured the NuGet.org source as `https://api.nuget.org/v3/index.json`. Symbol package publishing is only supported by [the NuGet V3 API](../api/overview.md#versioning).

## NuGet.org symbol server

NuGet.org supports its own symbols server repository and only accepts the new symbol package format - `.snupkg`. Package consumers can use the symbols published to nuget.org symbol server by adding `https://symbols.nuget.org/download/symbols` to their symbol sources in Visual Studio, which allows stepping into package code in the Visual Studio debugger. See [Specify symbol (.pdb) and source files in the Visual Studio debugger](https://docs.microsoft.com/en-us/visualstudio/debugger/specify-symbol-dot-pdb-and-source-files-in-the-visual-studio-debugger?view=vs-2017) for details on that process.

### Nuget.org symbol package constraints

The symbol packages supported on nuget.org have the following contraints

- Only the following file extensions are allowed to be added to a symbol package. ```.pdb,.nuspec,.xml,.psmdcp,.rels,.p7s```
- Only managed [Portable pdbs](https://github.com/dotnet/corefx/blob/master/src/System.Reflection.Metadata/specs/PortablePdb-Metadata.md) are currently supported on nuget symbol server.
- The pdbs and associated nupkg dlls need to be built with the compiler in Visual Studio version 15.9 or above (see [pdb crypto hash](https://github.com/dotnet/roslyn/issues/24429))

The symbol package publish on nuget.org will fail if any other file types are included in the .snupkg.

### Symbol package validation and indexing

Symbol packages published to [NuGet.org](https://www.nuget.org/) undergo several validations, such as virus checks.

When the package has passed all validation checks, it might take a while for the symbols to index and be available for consumption from the NuGet.org symbol servers. If the package fails a validation check, the package details page for the .nupkg will update to display the associated error and you will also receive an email notifying you about it.

Package validation and indexing usually takes under 15 minutes. If the package publishing is taking longer than expected, visit [status.nuget.org](https://status.nuget.org/) to check if nuget.org is experiencing any interruptions. If all systems are operational and the package hasn't been successfully published within an hour, please login to nuget.org and contact us using the Contact Support link on the package details page.

## Symbol package structure

The .nupkg file would be exactly the same as it is today, but the .snupkg file would have the following characteristics:

1) The .snupkg will have the same id and version as the corresponding .nupkg.
2) The .snupkg will have the exact folder structure as the nupkg for any DLL or EXE files with the distinction that instead of DLLs/EXEs, their corresponding PDBs will be included in the same folder hierarchy. Files and folders with extensions other than PDB will be left out of the snupkg.
3) The .nuspec file in the .snupkg will also specify a new PackageType as below. This should the only one PackageType specified. 
``` 
<packageTypes>
  <packageType name="SymbolsPackage"/>
</packageTypes>
```
4) If an author decides to use a custom nuspec to build their nupkg and snupkg, the snupkg should have the same folder hierarchy and files detailed in 2).
5) ```authors``` and ```owners``` field will be excluded from the snupkg's nuspec.

## See Also

[NuGet-Package-Debugging-&-Symbols-Improvements](https://github.com/NuGet/Home/wiki/NuGet-Package-Debugging-&-Symbols-Improvements)
