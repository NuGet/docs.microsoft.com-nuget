---
title: Creating legacy symbol packages (.symbols.nupkg)
description: How to create NuGet packages that contain only symbols to support debugging of other NuGet packages in Visual Studio.
author: JonDouglas
ms.author: jodou
ms.date: 09/12/2017
ms.topic: how-to
ms.reviewer: anangaur
---

# Creating legacy symbol packages (.symbols.nupkg)

> [!Important]
> The new recommended format for symbol packages is .snupkg. See [Creating symbol packages (.snupkg)](Symbol-Packages-snupkg.md). </br>
> .symbols.nupkg is still supported but only for compatibility reasons.

In addition to building packages for nuget.org or other sources, NuGet also supports creating associated symbol packages that can be published to symbol servers. 

## Creating a legacy symbol package

To create a legacy symbol package, follow these conventions:

- Name the primary package (with your code) `{identifier}.nupkg` and include all your files except `.pdb` files.
- Name the legacy symbol package `{identifier}.symbols.nupkg` and include your assembly DLL, `.pdb` files, XMLDOC files, source files (see the sections that follow).

You can create both packages with the `-Symbols` option, either from a `.nuspec` file or a project file:

```cli
nuget pack MyPackage.nuspec -Symbols

nuget pack MyProject.csproj -Symbols
```

Note that `pack` requires Mono 4.4.2 on Mac OS X and does not work on Linux systems. On a Mac, you must also convert Windows pathnames in the `.nuspec` file to Unix-style paths.

## Legacy symbol package structure

A legacy symbol package can target multiple target frameworks in the same way that a library package does, so the structure of the `lib` folder should be exactly the same as the primary package, only including `.pdb` files alongside the DLL.

For example, a legacy symbol package that targets .NET 4.0 and Silverlight 4 would have this layout:

```
\lib
    \net40
        \MyAssembly.dll
        \MyAssembly.pdb
    \sl40
        \MyAssembly.dll
        \MyAssembly.pdb
```

Source files are then placed in a separate special folder named `src`, which must follow the relative structure of your source repository. This is because PDBs contain absolute paths to source files used to compile the matching DLL, and they need to be found during the publishing process. A base path (common path prefix) can be stripped out. For example, consider a library built from these files:

```
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
```

Apart from the `lib` folder, a legacy symbol package would need to contain this layout:

```
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
```

## Referring to files in the nuspec

A legacy symbol package can be built by conventions, from a folder structure as described in the previous section, or by specifying its contents in the `files` section of the manifest. For example, to build the package shown in the previous section, use the following in the `.nuspec` file:

```xml
<files>
    <file src="Full\bin\Debug\*.dll" target="lib\net40" />
    <file src="Full\bin\Debug\*.pdb" target="lib\net40" />
    <file src="Silverlight\bin\Debug\*.dll" target="lib\sl40" />
    <file src="Silverlight\bin\Debug\*.pdb" target="lib\sl40" />
    <file src="**\*.cs" target="src" />
</files>
```

## See also

* [Creating symbol packages (.snupkg)](Symbol-Packages-snupkg.md) - The new recommended format for symbol packages
