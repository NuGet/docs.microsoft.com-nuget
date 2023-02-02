---
title: How to publish NuGet symbol packages using the new symbol package format '.snupkg'| Microsoft Docs
author: JonDouglas
ms.author: jodou
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

A good debugging experience relies on the presence of debug symbols as they provide critical information like the association between the compiled and the source code, names of local variables, stack traces, and more. You can use symbol packages (.snupkg) to distribute these symbols and improve the debugging experience of your NuGet packages.

> Note that symbol package isn't the only strategy to make the debug symbols available to the consumers of your library. It's also [possible to `embed`](/dotnet/core/deploying/single-file#include-pdb-files-inside-the-bundle) them in the `dll` or `exe` with the following project property:
> `<DebugType>embedded</DebugType>`

## Prerequisites

[nuget.exe v4.9.0 or above](https://www.nuget.org/downloads) or [dotnet CLI v2.2.0 or above](https://www.microsoft.com/net/download/dotnet-core/2.2), which implement the required [NuGet protocols](../api/nuget-protocols.md).

## Creating a symbol package

If you're using dotnet CLI or MSBuild, you need to set the `IncludeSymbols` and `SymbolPackageFormat` properties to create a .snupkg file in addition to the .nupkg file.

* Either add the following properties to your .csproj file:

   ```xml
   <PropertyGroup>
      <IncludeSymbols>true</IncludeSymbols>
      <SymbolPackageFormat>snupkg</SymbolPackageFormat>
   </PropertyGroup>
   ```

* Or specify these properties on the command-line:

     ```dotnetcli
     dotnet pack MyPackage.csproj -p:IncludeSymbols=true -p:SymbolPackageFormat=snupkg
     ```

  or

  ```cli
  msbuild MyPackage.csproj /t:pack /p:IncludeSymbols=true /p:SymbolPackageFormat=snupkg
  ```

If you're using NuGet.exe, you can use the following commands to create a .snupkg file in addition to the .nupkg file:

```cli
nuget pack MyPackage.nuspec -Symbols -SymbolPackageFormat snupkg

nuget pack MyPackage.csproj -Symbols -SymbolPackageFormat snupkg
```

The [`SymbolPackageFormat`](/dotnet/core/tools/csproj#symbolpackageformat) property can have one of two values: `symbols.nupkg` (the default) or `snupkg`. If this property is not specified, a legacy symbol package will be created.

> [!Note]
> The legacy format `.symbols.nupkg` is still supported but only for compatibility reasons like native packages (see [Legacy Symbol Packages](Symbol-Packages.md)). NuGet.org's symbol server only accepts the new symbol package format - `.snupkg`.

## Publishing a symbol package

> [!Note]
> [Azure Devops Artifacts](https://azure.microsoft.com/services/devops/artifacts) does not currently support debugging via `.snupkg` files.

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

NuGet.org supports its own symbols server repository and only accepts the new symbol package format - `.snupkg`. Package consumers can use the symbols published to nuget.org symbol server by adding `https://symbols.nuget.org/download/symbols` to their symbol sources in Visual Studio, which allows stepping into package code in the Visual Studio debugger. See [Specify symbol (.pdb) and source files in the Visual Studio debugger](/visualstudio/debugger/specify-symbol-dot-pdb-and-source-files-in-the-visual-studio-debugger) for details on that process.

### NuGet.org symbol package constraints

NuGet.org has the following constraints for symbol packages:

- Only the following file extensions are allowed in symbol packages: `.pdb`, `.nuspec`, `.xml`, `.psmdcp`, `.rels`, `.p7s`
- Only managed [Portable PDBs](https://github.com/dotnet/runtime/blob/87572a799bfd37779c079faf28544e3f9a16be58/src/libraries/System.Reflection.Metadata/specs/PortablePdb-Metadata.md) are supported on NuGet.org's symbol server.
- The PDBs and their associated .nupkg DLLs need to be built with the compiler in Visual Studio version 15.9 or above (see [PDB crypto hash](https://github.com/dotnet/roslyn/issues/24429))

Symbol packages published to NuGet.org will fail validation if these constraints aren't met. 

> [!NOTE]
> Native projects, such as C++ projects, produce Windows PDBs instead of Portable PDBs. These are not supported by NuGet.org's symbol server. Please use [Legacy Symbol Packages](Symbol-Packages.md) instead.

### Symbol package validation and indexing

Symbol packages published to [NuGet.org](https://www.nuget.org/) undergo several validations, including malware scanning. If a package fails a validation check, its package details page will display an error message. In addition, the package's owners will receive an email with instructions on how to fix the identified issues.

When the symbol package has passed all validations, the symbols will be indexed by NuGet.org's symbol servers and will be available for consumption.

Package validation and indexing usually takes under 15 minutes. If the package publishing takes longer than expected, visit [status.nuget.org](https://status.nuget.org/) to check if NuGet.org is experiencing any interruptions. If all systems are operational and the package hasn't been successfully published within an hour, please login to nuget.org and contact us using the Contact Support link on the package details page.

## Symbol package structure

The symbol package (.snupkg) has the following characteristics:

1) The .snupkg has the same id and version as its corresponding NuGet package (.nupkg).
2) The .snupkg has the same folder structure as its corresponding .nupkg for any DLL or EXE files with the distinction that instead of DLLs/EXEs, their corresponding PDBs will be included in the same folder hierarchy. Files and folders with extensions other than PDB will be left out of the snupkg.
3) The symbol package's .nuspec file has the `SymbolsPackage` package type:

   ```xml
   <packageTypes>
      <packageType name="SymbolsPackage"/>
   </packageTypes>
   ```

4) If an author decides to use a custom nuspec to build their nupkg and snupkg, the snupkg should have the same folder hierarchy and files detailed in 2).
5) The following fields will be excluded from the snupkg's nuspec: ```authors```, ```owners```, ```requireLicenseAcceptance```, ```license type```, ```licenseUrl```, and  ```icon```.
6) Do not use the ```<license>``` element. A .snupkg is covered under the same license as the corresponding .nupkg.

## See also

Consider using Source Link to enable source code debugging of .NET assemblies. For more information, please refer to the [Source Link guidance](/dotnet/standard/library-guidance/sourcelink).

For more information on symbol packages, please refer to the [NuGet Package Debugging & Symbols Improvements](https://github.com/NuGet/Home/wiki/NuGet-Package-Debugging-&-Symbols-Improvements) design spec.
