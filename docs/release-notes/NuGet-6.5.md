---
title: NuGet 6.5 Release Notes
description: Release notes for NuGet 6.5 including new features, bug fixes, and DCRs.
author: martinrrm
ms.author: mruizmares
ms.date: 2/21/2023
ms.topic: conceptual
---

# NuGet 6.5 Release Notes

NuGet distribution vehicles:

| NuGet version | Available in Visual Studio version | Available in .NET SDK(s) |
|:---|:---|:---|
| [**6.5**](https://nuget.org/downloads) | [Visual Studio 2022 version 17.5](https://visualstudio.microsoft.com/downloads/) | [7.0.200](https://dotnet.microsoft.com/download/dotnet-core/7.0)<sup>1</sup> |
| [**6.5.1**](https://nuget.org/downloads) | N/A | N/A <sup>1</sup> |

<sup>1</sup> Installed with Visual Studio 2022 with.NET Core workload

## Summary: What's New in 6.5.1

* [Security]: Microsoft Security Advisory CVE-2023-29337 | NuGet Client Remote Code Execution Vulnerability - [#12653](https://github.com/NuGet/Home/issues/12653)

> [!NOTE]
> There is a behavior breaking change on Linux. The temp folder where NuGet stores temporary files during its various operations is changed from `/tmp/NuGetScratch` to `/tmp/NuGetScratch<username>` on Linux. E.g. for user User1, the temp folder will be `/tmp/NuGetScratchUser1`.

## Summary: What's New in 6.5

* Manage packages in the Directory.Packages.props file for CPM projects - [#11890](https://github.com/NuGet/Home/issues/11890)

* Package Source Mapping UI - Allow the Creation/Removal of package source mappings in the NuGet Options UI - [#11363](https://github.com/NuGet/Home/issues/11363)

* Package Source Mapping UI - List Package Source Mappings in the NuGet Options UI - [#11362](https://github.com/NuGet/Home/issues/11362)

* Observe Retry-After delay on HTTP request retry - [#10558](https://github.com/NuGet/Home/issues/10558)

* Machine readable output for dotnet list package - [#7752](https://github.com/NuGet/Home/issues/7752)

* Honor WarningsNotAsErrors - [#5375](https://github.com/NuGet/Home/issues/5375)

* Reload Visual Studio package sources when nuget.config is modified manually - [#1538](https://github.com/NuGet/Home/issues/1538)

* Restore dependencies for projects listed in a solution filter (.slnf) file - [#10809](https://github.com/NuGet/Home/issues/10809)

### Issues fixed in this release

**DCRs:**

* During MSBuild project SDK resolution, always log errors in the case of failure - [#12312](https://github.com/NuGet/Home/issues/12312)

* Environment variable `NUGET_CLI_LANGUAGE` to control language of nuget.exe command output - [#12181](https://github.com/NuGet/Home/issues/12181)

* Make it obvious when a warning is elevated to an error - [#8803](https://github.com/NuGet/Home/issues/8803)

* Show error when using nuget.exe to pack SDK csproj - [#7778](https://github.com/NuGet/Home/issues/7778)

**Bugs:**

* Output version option only applicable for format json option in dotnet list package - [#12293](https://github.com/NuGet/Home/issues/12293)

* NuGet transitive pinning changes g.props import order, breaking the build - [#12278](https://github.com/NuGet/Home/issues/12278)

* PrivateAssets for central transitive dependencies should flow regardless whether the parent is a project or a package - [#12276](https://github.com/NuGet/Home/issues/12276)

* Include flags flow incorrectly to transitively pinned centrally managed dependencies - [#12274](https://github.com/NuGet/Home/issues/12274)

* Possible race condition in ConfigurationDefaults.Instance.DefaultPackageSources - [#12246](https://github.com/NuGet/Home/issues/12246)

* NuGet doesn't retry on HTTP 429 responses - [#12214](https://github.com/NuGet/Home/issues/12214)

* Simplification of Linq Any method for performance improvement - [#12193](https://github.com/NuGet/Home/issues/12193)

* StackOverflowException in InstallPackagesFromVSExtensionRepository - [#12192](https://github.com/NuGet/Home/issues/12192)

* Versions_SelectionChanged throws NullReferenceException when changing selected package - [#12184](https://github.com/NuGet/Home/issues/12184)

* Static graph-based restore fails with a NullReferenceException during SetPlatform negotiation - [#12177](https://github.com/NuGet/Home/issues/12177)

* Reduce memory allocation while creating empty InnerNodes and ParentNodes for a new GraphNode during restore - [#12157](https://github.com/NuGet/Home/issues/12157)

* SettingsLoadingContext throws exceptions when a different settings file causes an exception - [#12154](https://github.com/NuGet/Home/issues/12154)

* Bad NuGet.config causes NuGet-based MSBuild SDK resolver to throw an unhandled exception - [#12152](https://github.com/NuGet/Home/issues/12152)

* Watermark TextBox font color is incorrect in Add Dialog of Package Source Mapping Options - [#12141](https://github.com/NuGet/Home/issues/12141)

* VS2022 17.1.2: Dependency between .NET Standard 2.0 and .NET 4.7.1 throws NuGet error System.Memory, Version=4.0.1.1 not found - [#12137](https://github.com/NuGet/Home/issues/12137)

* Breaking change in .NET 8 - Environment.GetFolderPath returns incorrect path on Unix - [#12127](https://github.com/NuGet/Home/issues/12127)

* nuget.exe strings from NuGet.Commands are not localized - [#12097](https://github.com/NuGet/Home/issues/12097)

* Process argument string is too long when publishing in Visual Studio with static graph enabled - [#11968](https://github.com/NuGet/Home/issues/11968)

* Provide solution for NuGet Error NU1012 - the pack error does not call out the problem files - [#11905](https://github.com/NuGet/Home/issues/11905)

* Some CLI commands don't respect DOTNET_CLI_UI_LANGUAGE - [#11326](https://github.com/NuGet/Home/issues/11326)

* Reduce UI thread switching when determining solution folder and if solution is open - [#11090](https://github.com/NuGet/Home/issues/11090)

* Issue saving settings in Visual Studio - adding a source throws an exception - [#8407](https://github.com/NuGet/Home/issues/8407)

* PackageDependencyGroup does not implement Equals correctly - [#6478](https://github.com/NuGet/Home/issues/6478)

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