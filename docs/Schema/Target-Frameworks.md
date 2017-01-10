---
# required metadata

title: Target Frameworks References for NuGet | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 1/9/2017
ms.topic: article
ms.prod: nuget
#ms.service:
ms.technology: nuget
ms.assetid: 4343a48e-f6df-4a44-9d66-4616c3caacf5

# optional metadata

#description:
#keywords:
#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer:
- karann
- unnir
#ms.suite:
#ms.tgt_pltfrm:
#ms.custom:

---

# Target Frameworks

NuGet uses target framework references in a variety of places to specifically identify and isolate framework-dependent components of a package:

- [nuspec manifest](../schema/nuspec.md): A package can indicate distinct packages to be included in a project depending on the project's target framework.
- [nupkg folder name](../create-packages/creating-a-package.md#from-a-convention-based-working-directory): The folders inside a package's `lib` folder can be named according to the target framework, each of which contains the DLLs and other content appropriate to that framework.
- [project.json](../schema/project.json.md): The `frameworks` node specifies the framework versions that the project can be compiled against.


> [!Note]
> The NuGet client source code that calculates the tables below is found in the following locations:
> -  Supported framework names: [FrameworkConstants.cs](https://github.com/NuGet/NuGet.Client/blob/dev/src/NuGet.Core/NuGet.Frameworks/FrameworkConstants.cs)
> -  Framework precedence and mapping: [DefaultFrameworkMappings.cs](https://github.com/NuGet/NuGet.Client/blob/dev/src/NuGet.Core/NuGet.Frameworks/DefaultFrameworkMappings.cs)


## Supported Frameworks

A framework is typically referenced by a short target framework moniker or TFM. In .NET Standard this is also is generalized to *TxM* to allow a single reference to multiple frameworks.

The NuGet clients support the following frameworks. Equivalents are shown within brackets [].

| Name           | Abbreviation | TFMs/TxMs |
| -------------  | ------------ | --------- |
|.NET Framework  | net          | net11     |
|                |              | net20     |
|                |              | net35     |
|                |              | net40     |
|                |              | net403    |
|                |              |net45      |
|                |              |net451     |
|                |              |net452     |
|                |              |net46      |
|                |              |net461     |
|                |              |net462     |
|.NET Core       | netcore      | netcore [netcore45]
|                |              | netcore45 [win, win8]
|                |              | netcore451 [win81]
|                |              | netcore50
|.NET MicroFramework | netmf    | netmf
|Windows         | win          | win [win8, netcore45]
| | | win8 [netcore45, win]
| | | win81 [netcore451]
| | | win10 (not supported by Windows 10 Platform)
Silverlight | sl | sl4
| | | sl5
Windows Phone | wp | wp [wp7]
| | | wp7
| | | wp75
| | | wp8
| | | wp81
| | | wpa81
Universal Windows Platform | uap | uap [uap10]
| | | uap10 [uap]
.NET Standard | netstandard | netstandard1.0
| | | netstandard1.1
| | | netstandard1.2
| | | netstandard1.3
| | | netstandard1.4
| | | netstandard1.5
| | | netstandard1.6
.NET Core App | netcoreapp | netcoreapp1.0

## Deprecated Frameworks
The following frameworks are deprecated. Packages targeting these frameworks should migrate to the indicated replacements.

Deprecated framework | Replacement
--- | ---
aspnet50 | netcoreapp
aspnetcore50 |
dnxcore50 |
dnx |
dnx45 |
dnx451 |
dnx452 |
dotnet | netstandard
dotnet50 |
dotnet51 |
dotnet52 |
dotnet53 |
dotnet54 |
dotnet55 |
dotnet56 |
winrt | win

## Precedence

A number of frameworks are related to and compatible with one another, but not necessarily equivalent:

Framework | Can use
--- | ---
uap (Universal Windows Platform) | win81
| | wpa81
| | netcore50
win (Windows Store) | winrt
| | winrt45

## NET Platform Standard

The [.NET  Platform Standard](https://github.com/dotnet/corefx/blob/master/Documentation/architecture/net-platform-standard.md) simplifies references between binary-compatible frameworks, allowing a single target framework to reference a combination of others. (For background, see the [.NET Primer](https://docs.microsoft.com/en-us/dotnet/articles/standard/index).)

The [NuGet Get Nearest Framework Tool](https://aka.ms/s2m3th) simulates what NuGet uses to select one framework from many available framework assets in a package based on the project's framework.

The `dotnet` series of monikers should be used in NuGet 3.3 an earlier; the `netstandard` moniker syntax should be used in v3.4 and later.


## Portable Class Libraries

> [!Warning]
> **PCLs are not recommended**
>
> Although PCLs are supported, package authors should support netstandard instead. The .NET Platform Standard is an evolution of PCLs and represents binary portability across platforms using a single moniker that isn't tied to a static like like *portable-a+b+c* monikers.

To define a target framework that refers to multiple child-target-frameworks, the `portable` keyword use used to prefix the list of referenced frameworks. Avoid artificially including extra frameworks that are not directly compiled against because it can lead to unintended side-effects in those frameworks.

Additional frameworks defined by third parties provide compatibility with other environments that are accessible in this manner. Additionally, there are shorthand profile numbers that are available to reference these combinations of related frameworks as `Profile#`, but this is not a recommended practice to use these numbers as it reduces the readability of the folders and nuspec.

Profile # | Frameworks | Full name | .NET Standard
 --- | --- | --- | ---
 Profile2 | .NETFramework 4.0 | portable-net40+win8+sl4+wp7 |
 | Windows 8.0 | |
 | Silverlight 4.0 |
 | WindowsPhone 7.0|
 Profile3 | .NETFramework 4.0 | portable-net40+sl4
 | Silverlight 4.0 |
 Profile4 | .NETFramework 4.5 | portable-net45+sl4+win8+wp7
 | Silverlight 4.0 |
 | Windows 8.0 |
 | WindowsPhone 7.0 |
 Profile5 | .NETFramework 4.0 | portable-net40+win8
 | Windows 8.0 |
 Profile6 | .NETFramework 4.0.3 | portable-net403+win8
 | Windows 8.0 |
 Profile7 | .NETFramework 4.5 | portable-net45+win8    | netstandard1.1
 | Windows 8.0 |
 Profile14 | .NETFramework 4.0 | portable-net40+sl5
 | Silverlight 5.0 |
 Profile18 | .NETFramework 4.0.3 | portable-net403+sl4
 | Silverlight 4.0 |
 Profile19 | .NETFramework 4.0.3 | portable-net403+sl5
 | Silverlight 5.0 |
 Profile23 | .NETFramework 4.5 | portable-net45+sl4
 | Silverlight 4.0 |
 Profile24 | .NETFramework 4.5 | portable-net45+sl5
 | Silverlight 5.0 |
 Profile31 | Windows 8.1 | portable-win81+wp81 | netstandard1.0
 | WindowsPhone 8.1 |
 Profile32 | Windows 8.1 | portable-win81+wpa81 | netstandard1.2
 | WindowsPhone 8.1 |
 Profile36 | .NETFramework 4.0 | portable-net40+sl4+win8+wp8
 | Silverlight 4.0 |
 | Windows 8.0 |
 | WindowsPhone 8.0 |
 Profile37 | .NETFramework 4.0 | portable-net40+sl5+win8
 | Silverlight 5.0 |
 | Windows 8.0 |
 Profile41 | .NETFramework 4.0.3 | portable-net403+sl4+win8
 | Silverlight 4.0 |
 | Windows 8.0 |
 Profile42 | .NETFramework 4.0.3 | portable-net403+sl5+win8
 | Silverlight 5.0 |
 | Windows 8.0 |
 Profile44 | .NETFramework 4.5.1 | portable-net451+win81 | netstandard1.2
 | Windows 8.1 |
 Profile46 | .NETFramework 4.5 | portable-net45+sl4+win8
 | Silverlight 4.0 |
 | Windows 8.0 |
 Profile47 | .NETFramework 4.5 | portable-net45+sl5+win8
 | Silverlight 5.0 |
 | Windows 8.0 |
 Profile49 | .NETFramework 4.5 | portable-net45+wp8 | netstandard1.0
 | WindowsPhone 8.0 |
 Profile78 | .NETFramework 4.5 | portable-net45+win8+wp8 | netstandard1.0
 | Windows 8.0 |
 | WindowsPhone 8.0 |
 Profile84 | WindowsPhone 8.1 | portable-wp81+wpa81 | netstandard1.0
 | WindowsPhone 8.1 |
 Profile88 | .NETFramework 4.0 | portable-net40+sl4+win8+wp75
 | Silverlight 4.0 |
 | Windows 8.0 |
 | WindowsPhone 7.5 |
 Profile92 | .NETFramework 4.0 | portable-net40+win8+wpa81
 | Windows 8.0 |
 | WindowsPhone 8.1 |
 Profile95 | .NETFramework 4.0.3 | portable-net403+sl4+win8+wp7
 | Silverlight 4.0 |
 | Windows 8.0 |
 | WindowsPhone 7.0 |
 Profile96 | .NETFramework 4.0.3 | portable-net403+sl4+win8+wp75
 | Silverlight 4.0 |
 | Windows 8.0 |
 | WindowsPhone 7.5 |
 | Profile102 | .NETFramework 4.0.3 | portable-net403+win8+wpa81
 | Windows 8.0 |
 | WindowsPhone 8.1 |
 Profile104 | .NETFramework 4.5 | portable-net45+sl4+win8+wp75
 | Silverlight 4.0 |
 | Windows 8.0 |
 | WindowsPhone 7.5 |
 Profile111 | .NETFramework 4.5 | portable-net45+win8+wpa81 | netstandard1.1
 | Windows 8.0 |
 | WindowsPhone 8.1 |
 Profile136 | .NETFramework 4.0 | portable-net40+sl5+win8+wp8
 | Silverlight 5.0 |
 | Windows 8.0 |
 | WindowsPhone 8.0 |
 Profile143 | .NETFramework 4.0.3 | portable-net403+sl4+win8+wp8
 | Silverlight 4.0 |
 | Windows 8.0 |
 | WindowsPhone 8.0 |
 Profile147 | .NETFramework 4.0.3 | portable-net403+sl5+win8+wp8
 | Silverlight 5.0 |
 | Windows 8.0 |
 | WindowsPhone 8.0 |
 Profile151 | NETFramework 4.5.1 | portable-net451+win81+wpa81 | netstandard1.2
 | Windows 8.1 |
 | WindowsPhone 8.1 |
 Profile154 | .NETFramework 4.5 | portable-net45+sl4+win8+wp8
 | Silverlight 4.0 |
 | Windows 8.0 |
 | WindowsPhone 8.0 |
 Profile157 | Windows 8.1 | portable-win81+wp81+wpa81 | netstandard1.0
 | WindowsPhone 8.1 |
 | WindowsPhone 8.1 |
 Profile158 | .NETFramework 4.5 | portable-net45+sl5+win8+wp8
 | Silverlight 5.0 |
 | Windows 8.0 |
 | WindowsPhone 8.0 |
 Profile225 | .NETFramework 4.0 | portable-net40+sl5+win8+wpa81
 | Silverlight 5.0 |
 | Windows 8.0 |
 | WindowsPhone 8.1 |
 Profile240 | .NETFramework 4.0.3 | portable-net403+sl5+win8+wpa8
 | Silverlight 5.0 |
 | Windows 8.0 |
 | WindowsPhone 8.1 |
 Profile255 | .NETFramework 4.5 | portable-net45+sl5+win8+wpa81
 | Silverlight 5.0 |
 | Windows 8.0 |
 | WindowsPhone 8.1 |
 Profile259 | .NETFramework 4.5 | portable-net45+win8+wpa81+wp8 | netstandard1.0
 | Windows 8.0 |
 | WindowsPhone 8.1 |
 | WindowsPhone 8.0 |
 Profile328 | .NETFramework 4.0 | portable-net40+sl5+win8+wpa81+wp8
 | Silverlight 5.0 |
 | Windows 8.0 |
 | WindowsPhone 8.1 |
 | WindowsPhone 8.0 |
 Profile336 | .NETFramework 4.0.3 | portable-net403+sl5+win8+wpa81+wp8
 | Silverlight 5.0 |
 | Windows 8.0 |
 | WindowsPhone 8.1 |
 | WindowsPhone 8.1 |
 Profile344 | .NETFramework 4.5 | portable-net45+sl5+win8+wpa81+wp8
 | Silverlight 5.0 |
 | Windows 8.0 |
 | WindowsPhone 8.1 |
 | WindowsPhone 8.0 |

Additionally, NuGet packages targeting Xamarin can use additional Xamarin-defined frameworks. See [Creating NuGet packages for Xamarin](https://developer.xamarin.com/guides/cross-platform/advanced/nuget/).

 Name | Description | .NET Standard
 --- | --- | ---
 monoandroid | Mono Support for Android OS | netstandard1.4
 monotouch | Mono Support for iOS | netstandard1.4
 monomac | Mono Support for OSX | netstandard1.4
 xamarinios | Support for Xamarin for iOS | netstandard1.4
 xamarinmac | Supports for Xamarin for Mac | netstandard1.4
 xamarinpsthree | Support for Xamarin on Playstation 3 | netstandard1.4
 xamarinpsfour | Support for Xamarin on Playstation 4 | netstandard1.4
 xamarinpsvita | Support for Xamarin on PS Vita | netstandard1.4
 xamarinwatchos | Xamarin for Watch OS | netstandard1.4
 xamarintvos | Xamarin for TV OS | netstandard1.4
 xamarinxboxthreesixty | Xamarin for XBox 360 | netstandard1.4
 xamarinxboxone | Xamarin for XBox One | netstandard1.4

> [!Note]
> Stephen Cleary has created a tool that lists the supported PCLs, which you can find on his post, [Framework profiles in .NET](http://blog.stephencleary.com/2012/05/framework-profiles-in-net.html).