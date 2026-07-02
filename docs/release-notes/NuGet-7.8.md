---
title: NuGet 7.8 Release Notes
description: Release notes for NuGet 7.8 including new features, bug fixes, and DCRs.
author: Nigusu-Allehu
ms.date: 07/02/2026
ms.topic: release-notes
---

# NuGet 7.8 Release Notes

NuGet distribution vehicles:

| NuGet version | Available in Visual Studio version | Available in .NET SDK(s) |
|:---|:---|:---|
| [**7.8.0**](https://nuget.org/downloads) | [Visual Studio 2026 version 18.8.0](https://visualstudio.microsoft.com/downloads/) | N/A |

> [!NOTE]
> Visual Studio-only release.
> NuGet SDK packages are published quarterly with .NET.

## Summary: What's New in 7.8.0

* Attribute TargetFramework errors to the offending project - [#12470](https://github.com/NuGet/Home/issues/12470)

### Breaking changes

* NuGet nearest-TFM behavior for Windows TFM should take into account revision for CsWinRT 3.0 multi-targeting - [#14859](https://github.com/NuGet/Home/issues/14859)

* Add deprecation warning for monoandroid TFMs - [#14828](https://github.com/NuGet/Home/issues/14828)

* Plugin ServiceIndex is tied to Newtonsoft.Json (JObject), blocking STJ migration - [#14913](https://github.com/NuGet/Home/issues/14913)

* Plugin Message.Payload is tied to Newtonsoft.Json (JObject), blocking STJ migration - [#14901](https://github.com/NuGet/Home/issues/14901)

### Issues fixed in this release

* Visual Studio crashes with ArgumentOutOfRangeException in Package Manager UI search/refresh - [#14914](https://github.com/NuGet/Home/issues/14914)

* Spurious NU1601 warning when framework aliases of the same framework have different versions of the same package - [#14877](https://github.com/NuGet/Home/issues/14877)

* C++/CLI PackageReference project fails when referencing a C++ PackageReference project - [#14876](https://github.com/NuGet/Home/issues/14876)

* When accidentally specifying multiple TFMs in &lt;TargetFramework&gt;, InvalidOperationException is thrown - [#14605](https://github.com/NuGet/Home/issues/14605)

* &quot;Sequence contains no matching element&quot; when using a bad TF - [#13772](https://github.com/NuGet/Home/issues/13772)

* Perf: ContentFileUtils should cache/reuse virtual file instances - [#12696](https://github.com/NuGet/Home/issues/12696)

* [Bug]: invalid $(Version) causes poor MSBuild error message - [#11230](https://github.com/NuGet/Home/issues/11230)

* Migrate RepositorySignatureResource deserialization to System.Text.Json - [#14887](https://github.com/NuGet/Home/issues/14887)

* Migrate ServiceIndexResource deserialization to System.Text.Json - [#14886](https://github.com/NuGet/Home/issues/14886)

* Add System.Text.Json equivalent of SafeUriConverter - [#14885](https://github.com/NuGet/Home/issues/14885)

* Add System.Text.Json equivalent of SafeBoolConverter - [#14884](https://github.com/NuGet/Home/issues/14884)

* Add System.Text.Json equivalent of MetadataFieldConverter - [#14883](https://github.com/NuGet/Home/issues/14883)

* Add System.Text.Json equivalent of NuGetVersionConverter - [#14881](https://github.com/NuGet/Home/issues/14881)

* Add System.Text.Json equivalent of NuGetFrameworkConverter - [#14880](https://github.com/NuGet/Home/issues/14880)

[List of commits in this release](https://github.com/NuGet/NuGet.Client/compare/7.7.0.44...7.8.0.59)

### Community contributions

Thank you to all the contributors who helped make this NuGet release awesome!

* [nijulia](https://github.com/NuGet/NuGet.Client/pull/7408)
  * [7408](https://github.com/NuGet/NuGet.Client/pull/7408) Update DartLab1ES pipelines to reference templates in DartLab instead of DartLab.Templates
* [omajid](https://github.com/NuGet/NuGet.Client/pull/7020)
  * [7020](https://github.com/NuGet/NuGet.Client/pull/7020) Re-enable support for deterministic pack
* [sbomer](https://github.com/NuGet/NuGet.Client/pull/7229)
  * [7229](https://github.com/NuGet/NuGet.Client/pull/7229) Add NU1703 warning for packages using deprecated MonoAndroid framework
* [manodasanW](https://github.com/NuGet/NuGet.Client/pull/7287)
  * [7287](https://github.com/NuGet/NuGet.Client/pull/7287) Update TFM version compatibility behavior to correctly handle CsWinRT 3.0 .1 TFMs
