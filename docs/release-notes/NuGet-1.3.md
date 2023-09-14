---
title: NuGet 1.3 Release Notes
description: Release notes for NuGet 1.3 including known issues, bug fixes, added features, and DCRs.
author: JonDouglas
ms.author: jodou
ms.date: 11/11/2016
ms.topic: conceptual
---

# NuGet 1.3 Release Notes

[NuGet 1.2 Release Notes](../release-notes/nuget-1.2.md) | [NuGet 1.4 Release Notes](../release-notes/nuget-1.4.md)

NuGet 1.3 was released on April 25, 2011.

## New Features

### Streamlined Package Creation with symbol server integration

The NuGet team partnered with the folks at [SymbolSource.org](http://www.symbolsource.org/) to offer
a really simple way of publishing your sources and PDB’s along with your package. This allows consumers
of your package to step into the source for your package in the debugger. For more details, read
[Creating and Publishing a Symbol Package](../create-packages/symbol-packages.md)
The easy way to publish NuGet packages with sources. You can also watch a live demonstration of this
feature as part of the NuGet in Depth talk at Mix11. This feature is fully demonstrated starting at
the 20 minute mark of the video.

> [!NOTE]
> The above is deprecated and no longer supported.

### `Open-PackagePage` Command

This command makes it easy to get to the project page for a package from within the Package Manager
Console. It also provides options to open the license URL and the report abuse page for the package.
The syntax for the command is:

```
Open-PackagePage -Id <string> [-Version] [-Source] [-License] [-ReportAbuse] [-PassThru]
```

The `-PassThru` option is used to return the value of the specified URL.

Examples:

```
PM> Open-PackagePage Ninject
```

Opens a browser to the project URL specified in the Ninject package.

```
PM> Open-PackagePage Ninject -License
```

Opens a browser to the license URL specified in the Ninject package.

```
PM> Open-PackagePage Ninject -ReportAbuse
```

Opens a browser to the URL at the current package source used to report abuse for the specified package.

```
PM> $url = Open-PackagePage Ninject -License -WhatIf -PassThru
```

Assigns the license URL to the variable, $url, without opening the URL in a browser.

### Performance Improvements

NuGet 1.3 introduces a lot of performance improvements. NuGet 1.3 avoids downloading the same version of
a package multiple times by including a local per-user cache. The cache can be accessed and cleared via
the Package Manager Settings dialog:

![NuGet Options Dialog with Package Cache Settings](./media/nuget-options.png)

Other performance improvements include adding support for HTTP compression and improving the package
installation speed within Visual Studio.

### Visual Studio and nuget.exe uses the same list of package sources

Prior to NuGet 1.3, the list of package sources used by nuget.exe and the NuGet Visual Studio Add-In
were not stored in the same place. NuGet 1.3 now uses the same list in both places. The list is stored
in `NuGet.Config` and stored in the AppData folder.

### nuget.exe Ignores Files and Folders that start with '.' by default

In order to make NuGet work well with source control systems such Subversion and Mercurial, nuget.exe
ignores folders and files that start with the '.' character when creating packages. This can be overridden
using two new flags:

* __-NoDefaultExcludes__ is used to override this setting and include all files.
* __-Exclude__ is used to add other files/folders to exclude using a pattern. For example, to exclude
all files with the '.bak' file extension

```cli
nuget Pack MyPackage.nuspec -Exclude **\*.bak
```  

_Note: the pattern is not recursive by default._

### Support for WiX Projects and the .NET Micro Framework

Thanks to community contributions, NuGet includes support for WiX project types as well as the .NET Micro Framework.

## Bug Fixes

For a full list of bug fixes, please view the NuGet Issue Tracker for this release.

## Bug fixes worth noting

* Packages with source files work in both Websites and in Web Application Projects.
For Websites, source files are copied into the `App_Code` folder
