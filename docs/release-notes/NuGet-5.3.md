---
title: NuGet 5.3 Release Notes
description: Release notes for NuGet 5.3 including new features, bug fixes, and DCRs.
author: JonDouglas
ms.author: jodou
ms.date: 09/06/2019
ms.topic: release-notes
---

# NuGet 5.3 Release Notes

NuGet distribution vehicles:

| NuGet version | Available in Visual Studio version| Available in .NET SDK(s)|
|:---|:---|:---|
| [**5.3.0**](https://nuget.org/downloads) | [Visual Studio 2019 version 16.3](https://visualstudio.microsoft.com/downloads/) | [3.0.100](https://dotnet.microsoft.com/download/dotnet-core/3.0)<sup>1</sup> |
| [**5.3.1**](https://nuget.org/downloads) | [Visual Studio 2019 version 16.3.6](https://visualstudio.microsoft.com/downloads/) | [Future version: 3.0.101](https://dotnet.microsoft.com/download/dotnet-core/3.0) |

<sup>1</sup>Installed with Visual Studio 2019 with .NET Core workload

## Summary: What's New in 5.3

* [Package Icon can be embedded in the package](../reference/msbuild-targets.md#packing-an-icon-image-file), instead of needing an external URL. - [#352](https://github.com/NuGet/Home/issues/352)

* Improved security with SHA tracking and enforcement for Packages.Config - [#7281](https://github.com/NuGet/Home/issues/7281)

* Enable deprecation of obsolete/legacy NuGet Packages [#2867](https://github.com/NuGet/Home/issues/2867) | [Blog post](https://devblogs.microsoft.com/nuget/deprecating-packages-on-nuget-org/) | [Docs](../nuget-org/deprecate-packages.md)

### Issues fixed in this release

**Bugs**

* NuGet packages produced with 3.0.100-preview9 SDK cannot be used by 2.2 SDK users...depending on your timezone [#8603](https://github.com/NuGet/Home/issues/8603)

* Quote " characters in PATH cause "Illegal characters in path" failure in `nuget restore` [#8168](https://github.com/NuGet/Home/issues/8168)

* VS: assemblies are fully ngen-ed not partially ngen-ed - [#8513](https://github.com/NuGet/Home/issues/8513)

* Reduce memory usage (unsubscribe from events) - [#8471](https://github.com/NuGet/Home/issues/8471)

* "Error_UnableToFindProjectInfo" message is not grammatically correct - [#8441](https://github.com/NuGet/Home/issues/8441)

* NU1403 improvements - validate all packages, include the expected/actual sha values - [#8424](https://github.com/NuGet/Home/issues/8424)

* Multiple enumeration in `NuGetPackageManager.PreviewUpdatePackagesAsync` - [#8401](https://github.com/NuGet/Home/issues/8401)

* Revert "public -> internal" change in PluginProcess - [#8390](https://github.com/NuGet/Home/issues/8390)

* IVsPackageSourceProvider.GetSources(…) has ill-defined exception behavior - [#8383](https://github.com/NuGet/Home/issues/8383)

* Make PluginManager constructor public again - [#8379](https://github.com/NuGet/Home/issues/8379)

* Metrics to track the refresh rate of the PM UI - [#8369](https://github.com/NuGet/Home/issues/8369)

* Decrease the number of UI refreshes when installing through the Package Manager UI - [#8358](https://github.com/NuGet/Home/issues/8358)

* Telemetry:  datetime values use culture-specific formats - [#8351](https://github.com/NuGet/Home/issues/8351)

* Reduce UI refreshes in browse tab of Package Manager UI #6570 - [#8339](https://github.com/NuGet/Home/issues/8339)

* [Test Failure] “Unable to parse config file” will prompt twice - [#8320](https://github.com/NuGet/Home/issues/8320)

* Raise NU5037 error with good doc page that explains customer fixes (The package is missing the required nuspec file) - [#8291](https://github.com/NuGet/Home/issues/8291)

* Locked-mode restore fails when a project's RuntimeIdentifier is changed - [#8260](https://github.com/NuGet/Home/issues/8260)

* Make the Settings reading in VS lazy - [#8156](https://github.com/NuGet/Home/issues/8156)

* Regression in `Nuget sources add` causes "The ':' character, hexadecimal value 0x3A, cannot be included in a name" errors - [#7948](https://github.com/NuGet/Home/issues/7948)

* NuGet plugin credential providers - hide the process window - [#7511](https://github.com/NuGet/Home/issues/7511)

* Enforce PackagePathResolver is an absolute path - [#7349](https://github.com/NuGet/Home/issues/7349)

* Reduce UI refreshes in install and update tabs of Package Manager UI - [#6570](https://github.com/NuGet/Home/issues/6570)

**DCR:**

* Update Xamarin frameworks to map to NetStandard 2.1 - [#8368](https://github.com/NuGet/Home/issues/8368)

* Enable copying the contents of package manager "preview window" for install/update - [#8324](https://github.com/NuGet/Home/issues/8324)

* Enable restore on .proj files - [#8212](https://github.com/NuGet/Home/issues/8212)

* Introduce `NUGET_NETFX_PLUGIN_PATHS` and `NUGET_NETCORE_PLUGIN_PATHS` to support configuration of both at same time - [#8151](https://github.com/NuGet/Home/issues/8151)

* Enable multiple versions for a PackageDownload via Version attribute - [#8074](https://github.com/NuGet/Home/issues/8074)

* Add -SolutionDirectory and -PackageDirectory options to nuget.exe pack - [#7163](https://github.com/NuGet/Home/issues/7163)

**[List of all issues fixed in this release - 5.3](https://github.com/nuget/home/issues?q=is%3Aissue+is%3Aclosed+milestone%3A%225.3")**

## Summary: What's New in 5.3.1

* Plugin: A task was canceled - don't let cancellations affect plugin instantiation - [#8648](https://github.com/NuGet/Home/issues/8648)

* Restore Task cannot be safely run twice in one process (when Credential providers are used) - [#8688](https://github.com/NuGet/Home/issues/8688)
