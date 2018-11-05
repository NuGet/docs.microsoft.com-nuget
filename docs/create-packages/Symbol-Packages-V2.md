---
title: How to publish NuGet symbol packages using the new symbol package format '.snupkg'| Microsoft Docs
author:
- cristinamanu
- kraigb
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
----


# Creating symbol packages (.snupkg)
> [!Important]
> This feature is available only with [nuget.exe v4.9.0 or above](https://www.nuget.org/downloads) or [dotnet.exe v2.2.0 or above](https://www.microsoft.com/net/download/dotnet-core/2.2), which implement the required [NuGet protocols](../api/nuget-protocols.md).
> The new format for the symbol packages in .snupkg. The previous format .symbols.nupkg is still supported but only for compatibility reasons (see [Symbol-Packages](Symbol-Packages.md)).

Nuget.org supports its own symbols server repository. In addition to building packages for nuget.org or other sources, NuGet also supports creating associated symbol packages and publishing them to the symbol server repository.
Package consumers can use the symbols published to nuget.org symbol server by adding `https://symbols.nuget.org/download/symbols` to their symbol sources in Visual Studio, which allows stepping into package code in the Visual Studio debugger. See [Specify symbol (.pdb) and source files in the Visual Studio debugger](/visualstudio/debugger/specify-symbol-dot-pdb-and-source-files-in-the-visual-studio-debugger) for details on that process.

Nuget.exe pack and msbuild /t:pack has an additional command line property called ```-SymbolsPackageFormat```. Since the symbols package will not be produced by default, so this option will only be applicable if passed along with ```-Symbols``` in nuget.exe or ```--include-symbols``` in dotnet.exe. SymbolsPackageFormat property can have one of the two values: symbols.nupkg(the default) or snupkg. If the SymbolsPackageFormat is not specified the symbols.nupkg is created in the same way as it is today.


## Creating a symbol package

A snupkg symbol package can be created from a .nuspec file or from a .csproj file. Nuget.exe and dotnet.exe are both supported. When the options ```-Symbols -SymbolPackageFormat snupkg``` are used on the nuget.exe pack command a .snupkg file will be created in additon to the .nupkg file.

Example commands to create .snupkg files
```
dotnet.exe pack MyPackage.csproj --include-symbols -p:SymbolPackageFormat=snupkg

nuget.exe pack MyPackage.nuspec -Symbols -SymbolPackageFormat snupkg

nuget.exe pack MyPackage.csproj -Symbols -SymbolPackageFormat snupkg
```

## Symbol package structure

The .nupkg file would be exactly the same as it is today, but the .snupkg file would have the following characteristics:

1) The .snupkg will have the same id and version as the corresponding .nupkg.
2) The .snupkg will have the exact folder structure as the nupkg for any DLL or EXE files with the distinction that instead of DLLs/EXEs, their corresponding PDBs will be included in the same folder hierarchy. Files and folders with extensions other than PDB will be left out of the snupkg.
3) The .nuspec file in the .snupkg will also specify a new PackageType as follows:
``` 
<packageTypes>
  <packageType name="SybmolsPackage"/>
</packageTypes>
```
4) If an author decides to use a custom nuspec to build their nupkg and snupkg, the snupkg should have the same folder hierarchy and files detailed in 2).
5) ```authors``` and ```owners``` field will be excluded from the snupkg's nuspec.


## Nuget.org symbol package constraints
The symbol packages supported on nuget.org have the following contraints

- Only the following file extensions are allowed to be added to a symbol package. ```.pdb,.nuspec,.xml,.psmdcp,.rels,.p7s```
- Only managed [Portable pdbs](https://github.com/dotnet/corefx/blob/master/src/System.Reflection.Metadata/specs/PortablePdb-Metadata.md) are currently supported on nuget symbol server.
- The pdbs and associated nupkg dlls need to be built with the compiler in Visual Studio version 15.9 or above (see [pdb crypto hash](https://github.com/dotnet/roslyn/issues/24429))

The symbol package publish on nuget.org will fail if any other file types are included in the .snupkg.

## Publishing a symbol package

1. For convenience, first save your API key with NuGet (see [publish a package](../create-packages/publish-a-package.md)).

    ```cli
    nuget SetApiKey Your-API-Key
    ```

1. After publishing your primary package to nuget.org, push the symbol package as follows.

    ```cli
    nuget push MyPackage.snupkg
    ```

1. You can also push both primary and symbol packages at the same time using the below command. Both, .nupkg and .snupkg files need to be present in the current folder.

    ```cli
    nuget push MyPackage.nupkg
    ```

In this case, NuGet will publish to nuget.org the `MyPackage.nupkg` first followed by `MyPackage.snupkg`.

## See Also

[NuGet-Package-Debugging-&-Symbols-Improvements](https://github.com/NuGet/Home/wiki/NuGet-Package-Debugging-&-Symbols-Improvements)
