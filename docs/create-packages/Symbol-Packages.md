---
title: How to create NuGet symbol packages | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 09/12/2017
ms.topic: article
ms.prod: nuget
ms.technology: null
description: How to create NuGet packages that contain only symbols to support debugging of other NuGet packages in Visual Studio.
keywords: NuGet symbol packages, NuGet package debugging, supporting NuGet debugging, package symbols, symbol package conventions
ms.reviewer:
- anangaur
- karann-msft
- unniravindranathan
---

# Creating symbol packages

In addition to building packages for nuget.org or other sources, NuGet also supports creating associated symbol packages and publishing them to the SymbolSource repository.

Package consumers can then add `https://nuget.smbsrc.net` to their symbol sources in Visual Studio, which allows stepping into package code in the Visual Studio debugger. See [Specify symbol (.pdb) and source files in the Visual Studio debugger](/visualstudio/debugger/specify-symbol-dot-pdb-and-source-files-in-the-visual-studio-debugger) for details on that process.

## Creating a symbol package

To create a symbol package, follow these conventions:

- Name the primary package (with your code) `{identifier}.nupkg` and include all your files except `.pdb` files.
- Name the symbol package `{identifier}.symbols.nupkg` and include your assembly DLL, `.pdb` files, XMLDOC files, source files (see the sections that follow).

You can create both packages with the `-Symbols` option, either from a `.nuspec` file or a project file:

```cli
nuget pack MyPackage.nuspec -Symbols

nuget pack MyProject.csproj -Symbols
```

Note that `pack` requires Mono 4.4.2 on Mac OS X and does not work on Linux systems. On a Mac, you must also convert Windows pathnames in the `.nuspec` file to Unix-style paths.

## Symbol package structure

A symbol package can target multiple target frameworks in the same way that a library package does, so the structure of the `lib` folder should be exactly the same as the primary package, only including `.pdb` files alongside the DLL.

For example, a symbol package that targets .NET 4.0 and Silverlight 4 would have this layout:

    \lib
        \net40
            \MyAssembly.dll
            \MyAssembly.pdb
        \sl40
            \MyAssembly.dll
            \MyAssembly.pdb

Source files are then placed in a separate special folder named `src`, which must follow the relative structure of your source repository. This is because PDBs contain absolute paths to source files used to compile the matching DLL, and they need to be found during the publishing process. A base path (common path prefix) can be stripped out. For example, consider a library built from these files:

    C:\Projects
        \MyProject
            \Common
                \MyClass.cs
            \Full
                \Properties
                    \AssemblyInfo.cs
                \MyAssembly.csproj (producing \lib\net40\MyAssembly.dll)
            \Silverlight
                \Properties
                    \AssemblyInfo.cs
                \MySilverlightExtensions.cs
                \MyAssembly.csproj (producing \lib\sl4\MyAssembly.dll)

Apart from the `lib` folder, a symbol package would need to contain this layout:

    \src
        \Common
            \MyClass.cs
        \Full
            \Properties
                \AssemblyInfo.cs
        \Silverlight
            \Properties
                \AssemblyInfo.cs
            \MySilverlightExtensions.cs

## Referring to files in the nuspec

A symbol package can be built by conventions, from a folder structure as described in the previous section, or by specifying its contents in the `files` section of the manifest. For example, to build the package shown in the previous section, use the following in the `.nuspec` file:

```xml
<files>
    <file src="Full\bin\Debug\*.dll" target="lib\net40" />
    <file src="Full\bin\Debug\*.pdb" target="lib\net40" />
    <file src="Silverlight\bin\Debug\*.dll" target="lib\sl40" />
    <file src="Silverlight\bin\Debug\*.pdb" target="lib\sl40" />
    <file src="**\*.cs" target="src" />
</files>
```

## Publishing a symbol package

> [!Important]
> To push packages to nuget.org you must use [nuget.exe v4.1.0 or above](https://www.nuget.org/downloads), which implements the required [NuGet protocols](../api/nuget-protocols.md).

1. For convenience, first save your API key with NuGet (see [publish a package](../create-packages/publish-a-package.md), which will apply to both nuget.org and symbolsource.org, because symbolsource.org will check with nuget.org to verify that you are the package owner.

    ```cli
    nuget SetApiKey Your-API-Key
    ```

1. After publishing your primary package to nuget.org, push the symbol package as follows, which will automatically use symbolsource.org as the target because of the `.symbols` in the filename:

    ```cli
    nuget push MyPackage.symbols.nupkg
    ```
> [!Note]
> With nuget.exe 4.5.0 or above, the symbols packages are not automatically pushed to symbolsource.org. You would need to push the symbols packages separately as explained in the next step.

1. To publish to a different symbol repository, or to push a symbol package that doesn't follow the naming convention, use the `-Source` option:

    ```cli
    nuget push MyPackage.symbols.nupkg -source https://nuget.smbsrc.net/
    ```

1. You can also push both primary and symbol packages to both repositories at the same time using the following:

    ```cli
    nuget push MyPackage.nupkg
    ```

In this case, NuGet will publish `MyPackage.symbols.nupkg`, if present, to https://nuget.smbsrc.net/ (the push URL for symbolsource.org), after it publishes the primary package to nuget.org.

## See Also

[Moving to the new SymbolSource engine](https://tripleemcoder.com/2015/10/04/moving-to-the-new-symbolsource-engine/) (symbolsource.org)
