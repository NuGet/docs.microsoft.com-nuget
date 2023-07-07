---
title: NuGet 6.6 Release Notes
description: Release notes for NuGet 6.6 including new features, bug fixes, and DCRs.
author: donnie-msft
ms.author: eagoodso
ms.date: 5/1/2023
ms.topic: conceptual
---

# NuGet 6.6 Release Notes

NuGet distribution vehicles:

| NuGet version | Available in Visual Studio version | Available in .NET SDK(s) |
|:---|:---|:---|
| [**6.6**](https://nuget.org/downloads) | [Visual Studio 2022 version 17.6](https://visualstudio.microsoft.com/downloads/) | [7.0.300](https://dotnet.microsoft.com/download/dotnet-core/7.0)<sup>1</sup> |
| [**6.6.1**](https://nuget.org/downloads) | N/A | [7.0.304](https://dotnet.microsoft.com/download/dotnet-core/7.0)<sup>1</sup> |

<sup>1</sup> Installed with Visual Studio 2022 with .NET Core workload

## Summary: What's New in 6.6.1

* [Security]: Microsoft Security Advisory CVE-2023-29337 | NuGet Client Remote Code Execution Vulnerability - [#12653](https://github.com/NuGet/Home/issues/12653)

> [!NOTE]
> There is a behavior breaking change on Linux. The temp folder location, where NuGet stores temporary files during its various operations, has changed from `/tmp/NuGetScratch` to `/tmp/NuGetScratch<username>` on Linux. E.g. for user User1, the temp folder will be `/tmp/NuGetScratchUser1`.

## Summary: What's New in 6.6

* [Epic]: Central Package Management improvements for 17.6 - [#12413](https://github.com/NuGet/Home/issues/12413)

### Issues fixed in this release

**DCRs:**

* Static graph-based restore should always log an error on failure - [#12372](https://github.com/NuGet/Home/issues/12372)

**Bugs:**

* Set CentralPackageVersionOverrideEnabled=false in project with CPM broke project restore - [#12500](https://github.com/NuGet/Home/issues/12500)

* Static graph-based restore crashes on systems with alternate console encodings - [#12373](https://github.com/NuGet/Home/issues/12373)

* GlobalPackageReference is not working for legacy-style csproj projects - [#12368](https://github.com/NuGet/Home/issues/12368)

* WebSite projects opened from IIS fail to install packages - [#12337](https://github.com/NuGet/Home/issues/12337)

* StackOverflow in SemanticVersion.ToString - [#12330](https://github.com/NuGet/Home/issues/12330)

* Static graph restore failure when referencing unrestorable project - [#12322](https://github.com/NuGet/Home/issues/12322)

* CPM opt in detection in VS and commandline is different - [#12285](https://github.com/NuGet/Home/issues/12285)

* PrivateAssets flow incorrectly to transitively pinned centrally managed dependencies - [#12270](https://github.com/NuGet/Home/issues/12270)

* Performance regression of NuGet restores in the sdk v7.0.100 due to calculation of "CentralTransitiveDependencyGroups" - [#12269](https://github.com/NuGet/Home/issues/12269)

* [Bug]: `NuGet.VisualStudio` depends on package not existing on NuGet.org - [#12164](https://github.com/NuGet/Home/issues/12164)

* [Bug]: Custom kernel breaks nuget - [#11995](https://github.com/NuGet/Home/issues/11995)

* PackageSource:  returns possibly incorrect hash code - [#10276](https://github.com/NuGet/Home/issues/10276)

[List of commits in this release](https://github.com/NuGet/NuGet.Client/compare/6.6.0.61...6.5.0.160)

### Community contributions

Thank you to all the contributors who helped make this NuGet release awesome!

* [kant2002](https://github.com/kant2002)
  * [5103](https://github.com/NuGet/NuGet.Client/pull/5103) Fix project restore when CentralPackageVersionOverrideEnabled=false
* [atamagaii](https://github.com/atamagaii)
  * [5078](https://github.com/NuGet/NuGet.Client/pull/5078) Changed english resource MsbuildPathNotExist to correctly describe th…
* [pombredanne](https://github.com/pombredanne)
  * [5083](https://github.com/NuGet/NuGet.Client/pull/5083) Fix minor typo
* [NikolaMilosavljevic](https://github.com/NikolaMilosavljevic)
  * [5091](https://github.com/NuGet/NuGet.Client/pull/5091) Trim away netframework targets in source-build
* [uweigand](https://github.com/uweigand)
  * [5046](https://github.com/NuGet/NuGet.Client/pull/5046) Limit concurrent connections via NUGET_CONCURRENCY_LIMIT
* [marcin-krystianc](https://github.com/marcin-krystianc)
  * [4954](https://github.com/NuGet/NuGet.Client/pull/4954) Improved performance of calculation of PrivateAssets for transitively pinned centrally managed dependencies
  * [4953](https://github.com/NuGet/NuGet.Client/pull/4953) Effective PrivateAssets of centrally managed transitive dependencies should be an intersection of parent dependencies 