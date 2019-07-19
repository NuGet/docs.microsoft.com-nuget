---
title: NuGet 5.2 RTM Release Notes
description: Release notes for NuGet 5.2 including new features, bug fixes, and DCRs.
author: karann-msft
ms.author: karann
ms.date: 07/23/2019
ms.topic: conceptual
---

# NuGet 5.2 Release Notes

NuGet distribution vehicles:

| NuGet version | Available in Visual Studio version| Available in .NET SDK(s)|
|:---|:---|:---|
| [**5.2.0**](https://nuget.org/downloads) | [Visual Studio 2019 version 16.1](https://visualstudio.microsoft.com/downloads/) | [2.1.70X](https://dotnet.microsoft.com/download/dotnet-core/2.1)<sup>1</sup>, [2.2.30X](https://dotnet.microsoft.com/download/dotnet-core/2.2)<sup>2</sup> |

<sup>1</sup>Installed with Visual Studio 2019 with .NET Core workload 

<sup>2</sup>Available as an optional install with Visual Studio 2019 with .NET Core workload

## Summary: What's New in 5.2

**TODO**
* Support to skip a package push if it already exists to allow for better integration with CI/CD workflows - [#1630](https://github.com/NuGet/Home/issues/1630#issuecomment-483461100)

### Issues fixed in this release

**Bugs**

* Plugin:  NuGet waits full handshake timeout if plugin fails to launch or terminates early - [#8300](https://github.com/NuGet/Home/issues/8300)

* Plugins:  improve diagnosability of plugin launch failure - [#8271](https://github.com/NuGet/Home/issues/8271)

* Issue with nuget.exe discovery of built in plugins - [#8269](https://github.com/NuGet/Home/issues/8269)

* Package Manager Console:  UI delay updating "Default project" combobox selected value - [#8235](https://github.com/NuGet/Home/issues/8235)

* Plugins:  cache file is never read - [#8210](https://github.com/NuGet/Home/issues/8210)

* Plugins:  "A task was canceled." errors with authentication plugin during restore - [#8198](https://github.com/NuGet/Home/issues/8198)

* LockFile with ATF has false NU1004 due to a bad target framework equality check - [#8187](https://github.com/NuGet/Home/issues/8187)

* Restore:  installing a tampered signed package results in multiple failed install attempts (with repeated output) - [#8175](https://github.com/NuGet/Home/issues/8175)

* VS:  solution user options fail to deserialize after NuGet update - [#8166](https://github.com/NuGet/Home/issues/8166)

* '--locked-mode' restore flag not respected if lock file is empty or malformed - [#8160](https://github.com/NuGet/Home/issues/8160)

* dotnet-list-package in a UnitTest project returns an error - [#8154](https://github.com/NuGet/Home/issues/8154)

* Don't lowercase projects with custom assembly names in packages lock file - [#8114](https://github.com/NuGet/Home/issues/8114)

* Ensure all projects use the same version of Newtonsoft.Json - [#8108](https://github.com/NuGet/Home/issues/8108)

* Performance improvements in the PM UI - [#8039](https://github.com/NuGet/Home/issues/8039)

* Create NuGet package group for VS installer - fixing some VSIX setup problems - [#8033](https://github.com/NuGet/Home/issues/8033)

* Plugins cache not discoverable intermittently on linux platforms - [#7845](https://github.com/NuGet/Home/issues/7845)

* Make project reference lower case in lock file  - [#7840](https://github.com/NuGet/Home/issues/7840)

* GeneratePackageOnBuild should not set NoBuild. - [#7801](https://github.com/NuGet/Home/issues/7801)

* The new option "-SymbolPackageFormat snupkg" generates an error when the .nuspec file contains an explicit assembly reference element - [#7638](https://github.com/NuGet/Home/issues/7638)

* NuGet.targets(498,5): error : Could not find a part of the path '/tmp/NuGetScratch - [#7341](https://github.com/NuGet/Home/issues/7341)

* UI Delay when reading Default Project in PMC - [#6824](https://github.com/NuGet/Home/issues/6824)

* [vsfeedback] NuGet Update tab freezes for a local package source - [#6470](https://github.com/NuGet/Home/issues/6470)

**DCR:**

* Add an msbuild property that indicates that PackageDownload is supported - [#8106](https://github.com/NuGet/Home/issues/8106)

* FrameworkReference suppress dependency flow via FrameworkReference.PrivateAssets - [#7988](https://github.com/NuGet/Home/issues/7988)

* Mechanism for supplying runtime.json outside of a package - [#7351](https://github.com/NuGet/Home/issues/7351)

**[List of all issues fixed in this release - 5.2 RTM](https://github.com/nuget/home/issues?q=is%3Aissue+is%3Aclosed+milestone%3A%225.2")**


