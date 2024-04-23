---
title: Target Frameworks Reference for NuGet
description: NuGet target framework references identify and isolate framework-dependent components of a package.
author: JonDouglas
ms.author: jodou
ms.date: 12/11/2017
ms.topic: reference
ms.reviewer: anangaur
---

# Target frameworks

NuGet uses target framework references in a variety of places to specifically identify and isolate framework-dependent components of a package:

- [project file](../create-packages/multiple-target-frameworks-project-file.md): For SDK-style projects, the *.csproj* contains the target framework references.
- [.nuspec manifest](../reference/nuspec.md): A package can indicate distinct packages to be included in a project depending on the project's target framework.
- [.nupkg folder name](../create-packages/creating-a-package.md#from-a-convention-based-working-directory): The folders inside a package's `lib` folder can be named according to the target framework, each of which contains the DLLs and other content appropriate to that framework.
- [packages.config](../reference/packages-config.md): The `targetframework` attribute of a dependency specifies the variant of a package to install.

> [!Note]
> NuGet supports all of the modern .NET target frameworks:
> - For a list of the latest target frameworks, see the [Target frameworks in SDK-style projects](/dotnet/standard/frameworks) documentation.

## Supported frameworks

A framework is typically referenced by a short target framework moniker or TFM. In .NET Standard this is also generalized to *TxM* to allow a single reference to multiple frameworks.

> [!Note]
> The NuGet client source code that calculates the tables below is found in the following locations:
> - Supported framework names: [FrameworkConstants.cs](https://github.com/NuGet/NuGet.Client/blob/dev/src/NuGet.Core/NuGet.Frameworks/FrameworkConstants.cs)
> - Framework precedence and mapping: [DefaultFrameworkMappings.cs](https://github.com/NuGet/NuGet.Client/blob/dev/src/NuGet.Core/NuGet.Frameworks/DefaultFrameworkMappings.cs)

The NuGet clients support the frameworks in the table below. Equivalents are shown within brackets []. Note that some tools, such as `dotnet`, might use variations of canonical TFMs in some files. For example, `dotnet pack` uses  `.NETCoreApp2.0` in a `.nuspec` file rather than `netcoreapp2.0`. The various NuGet client tools handle these variations properly, but you should always use canonical TFMs when editing files directly.

| Name | Abbreviation | TFMs/TxMs |
| ------------- | ------------ | --------- |
|.NET Framework | net | net11 |
| | | net20 |
| | | net35 |
| | | net40 |
| | | net403 |
| | | net45 |
| | | net451 |
| | | net452 |
| | | net46 |
| | | net461 |
| | | net462 |
| | | net47 |
| | | net471 |
| | | net472 |
| | | net48 |
|Microsoft Store (Windows Store) | netcore | netcore [netcore45] |
| | | netcore45 [win, win8] |
| | | netcore451 [win81] |
| | | netcore50 |
|.NET MicroFramework | netmf | netmf |
|Windows | win | win [win8, netcore45] |
| | | win8 [netcore45, win] |
| | | win81 [netcore451] |
| | | win10 (not supported by Windows 10 Platform) |
Silverlight | sl | sl4 |
| | | sl5 |
Windows Phone (SL) | wp | wp [wp7] |
| | | wp7 |
| | | wp75 |
| | | wp8 |
| | | wp81 |
Windows Phone (UWP) | | wpa81 |
Universal Windows Platform | uap | uap [uap10.0] |
| | | uap10.0 |
| | | uap10.0.xxxxx  (where 10.0.xxxxx is the target platform min version of the consuming app) |
.NET Standard | netstandard | netstandard1.0 |
| | | netstandard1.1 |
| | | netstandard1.2 |
| | | netstandard1.3 |
| | | netstandard1.4 |
| | | netstandard1.5 |
| | | netstandard1.6 |
| | | netstandard2.0 |
| | | netstandard2.1 |
.NET 5+ (and .NET Core) | netcoreapp | netcoreapp1.0 |
| | | netcoreapp1.1 |
| | | netcoreapp2.0 |
| | | netcoreapp2.1 |
| | | netcoreapp2.2 |
| | | netcoreapp3.0 |
| | | netcoreapp3.1 |
| | net | net5.0 |
| | | net6.0 |
| | | net7.0 |
| | | net8.0 |
Tizen | tizen | tizen3 |
| | | tizen4 |
| Native | native | native |

## Deprecated frameworks

The following frameworks are deprecated. Packages targeting these frameworks should migrate to the indicated replacements.

| Deprecated framework | Replacement
| --- | ---
| aspnet50 | netcoreapp |
| aspnetcore50 |
| dnxcore50 |
| dnx |
| dnx45 |
| dnx451 |
| dnx452 |
| dotnet | netstandard |
| dotnet50 | |
| dotnet51 | |
| dotnet52 | |
| dotnet53 | |
| dotnet54 | |
| dotnet55 | |
| dotnet56 | |
| winrt | win |

## Precedence

A number of frameworks are related to and compatible with one another, but not necessarily equivalent:

| Framework | Can use |
| -- | --- |
| uap (Universal Windows Platform) | win81 |
| | wpa81 |
| | netcore50 |
| win (Microsoft Store) | winrt |
| | |

## NET Standard

[.NET Standard](/dotnet/standard/net-standard) simplifies references between binary-compatible frameworks, allowing a single target framework to reference a combination of others. (For background, see the [.NET Primer](/dotnet/articles/standard/index).)

The [NuGet Get Nearest Framework Tool](https://aka.ms/s2m3th) simulates what NuGet uses to select one framework from many available framework assets in a package based on the project's framework.

The `dotnet` series of monikers should be used in NuGet 3.3 and earlier; the `netstandard` moniker syntax should be used in v3.4 and later.

## Portable Class Libraries

> [!Warning]
> **PCLs are not recommended**. Although PCLs are supported, package authors should support netstandard instead. The .NET Platform Standard is an evolution of PCLs and represents binary portability across platforms using a single moniker that isn't tied to a static library like *portable-a+b+c* monikers.

To define a target framework that refers to multiple child-target-frameworks, the `portable` keyword use used to prefix the list of referenced frameworks. Avoid artificially including extra frameworks that are not directly compiled against because it can lead to unintended side-effects in those frameworks.

Additional frameworks defined by third parties provide compatibility with other environments that are accessible in this manner. Additionally, there are shorthand profile numbers that are available to reference these combinations of related frameworks as `Profile#`, but this is not a recommended practice to use these numbers as it reduces the readability of the folders and `.nuspec`.

| Profile # | Frameworks | Full name | .NET Standard |
 --- | --- | --- | ---
 Profile2 | .NETFramework 4.0 | portable-net40+win8+sl4+wp7 |
 | | Windows 8.0 | |
 | | Silverlight 4.0 |
 | | WindowsPhone 7.0|
 Profile3 | .NETFramework 4.0 | portable-net40+sl4
 | | Silverlight 4.0 |
 Profile4 | .NETFramework 4.5 | portable-net45+sl4+win8+wp7
 | | Silverlight 4.0 |
 | | Windows 8.0 |
 | | WindowsPhone 7.0 |
 Profile5 | .NETFramework 4.0 | portable-net40+win8
 | | Windows 8.0 |
 Profile6 | .NETFramework 4.0.3 | portable-net403+win8
 | | Windows 8.0 |
 Profile7 | .NETFramework 4.5 | portable-net45+win8 | netstandard1.1
 | | Windows 8.0 |
 Profile14 | .NETFramework 4.0 | portable-net40+sl5
 | | Silverlight 5.0 |
 Profile18 | .NETFramework 4.0.3 | portable-net403+sl4
 | | Silverlight 4.0 |
 Profile19 | .NETFramework 4.0.3 | portable-net403+sl5
 | | Silverlight 5.0 |
 Profile23 | .NETFramework 4.5 | portable-net45+sl4
 | | Silverlight 4.0 |
 Profile24 | .NETFramework 4.5 | portable-net45+sl5
 | | Silverlight 5.0 |
 Profile31 | Windows 8.1 | portable-win81+wp81 | netstandard1.0
 | | WindowsPhone 8.1 (SL) |
 Profile32 | Windows 8.1 | portable-win81+wpa81 | netstandard1.2
 | | WindowsPhone 8.1 (UWP) |
 Profile36 | .NETFramework 4.0 | portable-net40+sl4+win8+wp8
 | | Silverlight 4.0 |
 | | Windows 8.0 |
 | | WindowsPhone 8.0 (SL) |
 Profile37 | .NETFramework 4.0 | portable-net40+sl5+win8
 | | Silverlight 5.0 |
 | | Windows 8.0 |
 Profile41 | .NETFramework 4.0.3 | portable-net403+sl4+win8
 | | Silverlight 4.0 |
 | | Windows 8.0 |
 Profile42 | .NETFramework 4.0.3 | portable-net403+sl5+win8
 | | Silverlight 5.0 |
 | | Windows 8.0 |
 Profile44 | .NETFramework 4.5.1 | portable-net451+win81 | netstandard1.2
 | | Windows 8.1 |
 Profile46 | .NETFramework 4.5 | portable-net45+sl4+win8
 | | Silverlight 4.0 |
 | | Windows 8.0 |
 Profile47 | .NETFramework 4.5 | portable-net45+sl5+win8
 | | Silverlight 5.0 |
 | | Windows 8.0 |
 Profile49 | .NETFramework 4.5 | portable-net45+wp8 | netstandard1.0
 | | WindowsPhone 8.0 (SL) |
 Profile78 | .NETFramework 4.5 | portable-net45+win8+wp8 | netstandard1.0
 | | Windows 8.0 |
 | | WindowsPhone 8.0 (SL) |
 Profile84 | WindowsPhone 8.1 | portable-wp81+wpa81 | netstandard1.0
 | | WindowsPhone 8.1 (UWP) |
 Profile88 | .NETFramework 4.0 | portable-net40+sl4+win8+wp75
 | | Silverlight 4.0 |
 | | Windows 8.0 |
 | | WindowsPhone 7.5 |
 Profile92 | .NETFramework 4.0 | portable-net40+win8+wpa81
 | | Windows 8.0 |
 | | WindowsPhone 8.1 (UWP) |
 Profile95 | .NETFramework 4.0.3 | portable-net403+sl4+win8+wp7
 | | Silverlight 4.0 |
 | | Windows 8.0 |
 | | WindowsPhone 7.0 |
 Profile96 | .NETFramework 4.0.3 | portable-net403+sl4+win8+wp75
 | | Silverlight 4.0 |
 | | Windows 8.0 |
 | | WindowsPhone 7.5 |
 Profile102 | .NETFramework 4.0.3 | portable-net403+win8+wpa81
 | | Windows 8.0 |
 | | WindowsPhone 8.1 (UWP) |
 Profile104 | .NETFramework 4.5 | portable-net45+sl4+win8+wp75
 | | Silverlight 4.0 |
 | | Windows 8.0 |
 | | WindowsPhone 7.5 |
 Profile111 | .NETFramework 4.5 | portable-net45+win8+wpa81 | netstandard1.1
 | | Windows 8.0 |
 | | WindowsPhone 8.1 (UWP) |
 Profile136 | .NETFramework 4.0 | portable-net40+sl5+win8+wp8
 | | Silverlight 5.0 |
 | | Windows 8.0 |
 | | WindowsPhone 8.0 (SL) |
 Profile143 | .NETFramework 4.0.3 | portable-net403+sl4+win8+wp8
 | | Silverlight 4.0 |
 | | Windows 8.0 |
 | | WindowsPhone 8.0 (SL) |
 Profile147 | .NETFramework 4.0.3 | portable-net403+sl5+win8+wp8
 | | Silverlight 5.0 |
 | | Windows 8.0 |
 | | WindowsPhone 8.0 (SL) |
 Profile151 | NETFramework 4.5.1 | portable-net451+win81+wpa81 | netstandard1.2
 | | Windows 8.1 |
 | | WindowsPhone 8.1 (UWP) |
 Profile154 | .NETFramework 4.5 | portable-net45+sl4+win8+wp8
 | | Silverlight 4.0 |
 | | Windows 8.0 |
 | | WindowsPhone 8.0 (SL) |
 Profile157 | Windows 8.1 | portable-win81+wp81+wpa81 | netstandard1.0
 | | WindowsPhone 8.1 (SL) |
 | | WindowsPhone 8.1 (UWP) |
 Profile158 | .NETFramework 4.5 | portable-net45+sl5+win8+wp8
 | | Silverlight 5.0 |
 | | Windows 8.0 |
 | | WindowsPhone 8.0 (SL) |
 Profile225 | .NETFramework 4.0 | portable-net40+sl5+win8+wpa81
 | | Silverlight 5.0 |
 | | Windows 8.0 |
 | | WindowsPhone 8.1 (UWP) |
 Profile240 | .NETFramework 4.0.3 | portable-net403+sl5+win8+wpa8
 | | Silverlight 5.0 |
 | | Windows 8.0 |
 | | WindowsPhone 8.1 (UWP) |
 Profile255 | .NETFramework 4.5 | portable-net45+sl5+win8+wpa81
 | | Silverlight 5.0 |
 | | Windows 8.0 |
 | | WindowsPhone 8.1 (UWP) |
 Profile259 | .NETFramework 4.5 | portable-net45+win8+wpa81+wp8 | netstandard1.0
 | | Windows 8.0 |
 | | WindowsPhone 8.1 (UWP) |
 | | WindowsPhone 8.0 (SL) |
 Profile328 | .NETFramework 4.0 | portable-net40+sl5+win8+wpa81+wp8
 | | Silverlight 5.0 |
 | | Windows 8.0 |
 | | WindowsPhone 8.1 (UWP) |
 | | WindowsPhone 8.0 (SL) |
 Profile336 | .NETFramework 4.0.3 | portable-net403+sl5+win8+wpa81+wp8
 | | Silverlight 5.0 |
 | | Windows 8.0 |
 | | WindowsPhone 8.1 (UWP) |
 | | WindowsPhone 8.0 (SL) |
 Profile344 | .NETFramework 4.5 | portable-net45+sl5+win8+wpa81+wp8
 | | Silverlight 5.0 |
 | | Windows 8.0 |
 | | WindowsPhone 8.1 (UWP) |
 | | WindowsPhone 8.0 (SL) |

Additionally, NuGet packages targeting Xamarin can use additional Xamarin-defined frameworks. See [Creating NuGet packages for Xamarin](https://developer.xamarin.com/guides/cross-platform/advanced/nuget/).

| Name | Description | .NET Standard |
| --- | --- | ---
| monoandroid | Mono Support for Android OS | netstandard1.4 |
| monotouch | Mono Support for iOS | netstandard1.4 |
| monomac | Mono Support for OSX | netstandard1.4 |
| xamarinios | Support for Xamarin for iOS | netstandard1.4 |
| xamarinmac | Supports for Xamarin for Mac | netstandard1.4 |
| xamarinpsthree | Support for Xamarin on Playstation 3 | netstandard1.4 |
| xamarinpsfour | Support for Xamarin on Playstation 4 | netstandard1.4 |
| xamarinpsvita | Support for Xamarin on PS Vita | netstandard1.4 |
| xamarinwatchos | Xamarin for Watch OS | netstandard1.4 |
| xamarintvos | Xamarin for TV OS | netstandard1.4 |
| xamarinxboxthreesixty | Xamarin for XBox 360 | netstandard1.4 |
| xamarinxboxone | Xamarin for XBox One | netstandard1.4 |

> [!Note]
> Stephen Cleary has created a tool that lists the supported PCLs, which you can find on his post, [Framework profiles in .NET](http://blog.stephencleary.com/2012/05/framework-profiles-in-net.html).
