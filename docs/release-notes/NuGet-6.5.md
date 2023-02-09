---
title: NuGet 6.5 Release Notes
description: Release notes for NuGet 6.5 including new features, bug fixes, and DCRs.
author: <TODO: GitHubAlias>
ms.author: <TODO: MicrosoftAlias>
ms.date: 2/9/2023
ms.topic: conceptual
---

# NuGet 6.5 Release Notes

NuGet distribution vehicles:

| NuGet version | Available in Visual Studio version | Available in .NET SDK(s) |
|:---|:---|:---|
| [**6.5**](https://nuget.org/downloads) | [Visual Studio 2022 version 17.5](https://visualstudio.microsoft.com/downloads/) | [<TODO: Full SDK Version>](https://dotnet.microsoft.com/download/dotnet-core/<TODO: SDKMajorMinorVersionOnly)<sup>1</sup> |

<sup>1</sup> Installed with Visual Studio 2022 with.NET Core workload

## Summary: What's New in 6.5

* Machine readable output for dotnet list package - [#7752](https://github.com/NuGet/Home/issues/7752)

* Honor WarningsNotAsErrors - [#5375](https://github.com/NuGet/Home/issues/5375)

* Reload Visual Studio package sources when nuget.config is modified manually - [#1538](https://github.com/NuGet/Home/issues/1538)

### Issues fixed in this release

**DCRs:**

* During MSBuild project SDK resolution, always log errors in the case of failure - [#12312](https://github.com/NuGet/Home/issues/12312)

**Bugs:**

* StackOverflow in SemanticVersion.ToString - [#12330](https://github.com/NuGet/Home/issues/12330)

* [Bug]: StackOverflowException in InstallPackagesFromVSExtensionRepository - [#12192](https://github.com/NuGet/Home/issues/12192)

* [Bug]: Static graph-based restore fails with a NullReferenceException during SetPlatform negotiation - [#12177](https://github.com/NuGet/Home/issues/12177)

* [Bug]: SettingsLoadingContext throws exceptions when a different settings file causes an exception - [#12154](https://github.com/NuGet/Home/issues/12154)

* [Bug]: Bad NuGet.config causes NuGet-based MSBuild SDK resolver to throw an unhandled exception - [#12152](https://github.com/NuGet/Home/issues/12152)

* [Bug]: Process argument string is too long when publishing in Visual Studio with static graph enabled - [#11968](https://github.com/NuGet/Home/issues/11968)

[List of commits in this release](https://github.com/NuGet/NuGet.Client/compare/6.5.0.154...6.4.1.21)

### Community contributions

Thank you to all the contributors who helped make this NuGet release awesome!

* [Forgind](https://github.com/Forgind)
  * [4970](https://github.com/NuGet/NuGet.Client/pull/4970) Have SDK resolver always log an error when SDK resolution in unsuccessful
* [marcin-krystianc](https://github.com/marcin-krystianc)
  * [4952](https://github.com/NuGet/NuGet.Client/pull/4952) PrivateAssets for central transitive dependencies should flow regardless whether the parent node is a project or a package
* [marcin-krystianc](https://github.com/marcin-krystianc)
  * [4950](https://github.com/NuGet/NuGet.Client/pull/4950) Fix include flags calculation for transitively pinned centrally managed dependencies
* [drewnoakes](https://github.com/drewnoakes)
  * [4891](https://github.com/NuGet/NuGet.Client/pull/4891) Display documents from packages in the dependencies tree
* [kvpt](https://github.com/kvpt)
  * [4790](https://github.com/NuGet/NuGet.Client/pull/4790) Add support for restoring slnf file from command line
* [AtariDreams](https://github.com/AtariDreams)
  * [4863](https://github.com/NuGet/NuGet.Client/pull/4863) Simplification of Linq Any method
* [davidegiacometti](https://github.com/davidegiacometti)
  * [4840](https://github.com/NuGet/NuGet.Client/pull/4840) Refactor PackageDependencyGroup Equals and GetHashCode
* [danjagnow](https://github.com/danjagnow)
  * [4843](https://github.com/NuGet/NuGet.Client/pull/4843) Updated NU1012 error message to display item paths
**TODO: Issues that could not be categorized. Make sure the issue has the correct milestone (if required) or an appropriate Type label - StillOpens:**

* Re-enable flaky Apex tests failing due to IPC errors - [#11308](https://github.com/NuGet/Home/issues/11308)

