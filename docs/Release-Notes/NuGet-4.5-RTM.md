---
# required metadata

title: NuGet 4.5 RTM Release Notes | Microsoft Docs
author: anangaur
ms.author: anangaur
manager: unniravindranathan
ms.date: 12/4/2017
ms.topic: article
ms.prod: nuget
#ms.service:
ms.technology: null
ms.assetid: 68751bde-8814-4da3-89fd-7976a2a96762

# optional metadata

description: Release notes for NuGet 4.5 RTM including known issues, bug fixes, added features, and DCRs.
keywords: NuGet 4.5 RTM release notes, bug fixes, known issues, added features, DCRs
#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer:
- karann-msft
- unniravindranathan
- anangaur
#ms.suite:
#ms.tgt_pltfrm:
#ms.custom:

---

# 4.5 RTM Release Notes

[Visual Studio 2017 15.5 RTW](https://www.visualstudio.com/news/releasenotes/vs2017-relnotes) comes with NuGet 4.5 RTM.

## Known issues

### Issues with .NET Standard 2.0 with .NET Framework & NuGet 
.NET Standard & its tooling was designed such that projects targeting .NET Framework 4.6.1 can consume NuGet packages & projects targeting .NET Standard 2.0 or earlier. [This document](https://github.com/dotnet/standard/issues/481) summarizes the issues around that scenario, the plan for addressing them, and workarounds you can deploy with today's state of the tooling.

### You will be unable to view, add, or update DotNetCLITools, using Nuget Package Manager

#### Issue:
NuGet Package Manager does not display and does not allow add/update of DotNetCLITools. [NuGet#4256](https://github.com/NuGet/Home/issues/4256)

#### Workaround:
DotNetCLIToolReferences must be manually edited in your project file.

### Retargeting target framework version may lead to incomplete Intellisense

#### Issue:
Retargeting target framework version may lead to incomplete Intellisense, in Visual Studio. This happens when you are using PackageReferences as the package manager format. [NuGet#4216](https://github.com/NuGet/Home/issues/4216)

#### Workaround:
Do a manual restore.


### A package in a .NET Core project that contains an assembly with an invalid signature, can trigger an infinite restore loop

#### Issue:
Occasionally, when you use a package that contains an assembly with an invalid signature or when the package version is set with 'DateTime' ticker, it causes the package auto-restore to run in an infinite loop [dotnet/project-system#1457](https://github.com/dotnet/project-system/issues/1457).

#### Workaround:
There is no workaround at this time.


## Issues fixed in NuGet 4.5 RTM timeframe

[NuGet 4.4 RTM Release Notes](../release-notes/nuget-4.4-RTM.md) - Lists all the issues fixed for NuGet 4.4 RTM

**Feature:**

* Disable auto-push of symbols package - [#6113](https://github.com/NuGet/Home/issues/6113)

* Add Support: Enable installing Full .Net Framework  from within netstandad 2 ,netcore 2 library project by option - [#5977](https://github.com/NuGet/Home/issues/5977)

* nuget spec doesn't detect csproj dependencies - [#5833](https://github.com/NuGet/Home/issues/5833)

* RS3 – UWP should work with NET Standard 2.0 using PackageTargetFallback - [#4957](https://github.com/NuGet/Home/issues/4957)

* Deprecation and Security warnings in NuGet.exe/Visual Studio and NuGet.org - [#4731](https://github.com/NuGet/Home/issues/4731)

* Automate offer suggestions for NuGet packages - [#3634](https://github.com/NuGet/Home/issues/3634)

* Add end to end tests for MSBuild targets - [#3605](https://github.com/NuGet/Home/issues/3605)

* <clear /> on <packageSourceCredentials> does not work - [#3568](https://github.com/NuGet/Home/issues/3568)

* Adding static analysis tooling to help prevent semantic versioning breaks. - [#2195](https://github.com/NuGet/Home/issues/2195)

* Feature Discussion: Add --interactive flag to command-line - [#2055](https://github.com/NuGet/Home/issues/2055)


**DCR:**

* enable restore to share obj directory between projects - [#4154](https://github.com/NuGet/Home/issues/4154)

* Adapt Pack for Transitive Project References change - [#4077](https://github.com/NuGet/Home/issues/4077)

* Restore fails with custom framework names - [#4056](https://github.com/NuGet/Home/issues/4056)

* Restore, http cache and failed packages - [#4055](https://github.com/NuGet/Home/issues/4055)

* Investigate the usage of Managed C++ projects - [#2090](https://github.com/NuGet/Home/issues/2090)

* Determine a shared v2 and v3 MachineCache location - [#2088](https://github.com/NuGet/Home/issues/2088)

* IVsPackageRestorer API does not support UWP projects - [#2066](https://github.com/NuGet/Home/issues/2066)

* Add useragent to the ILogger context - [#2028](https://github.com/NuGet/Home/issues/2028)


**Bug:**

* Package Manager Console (PMC): Sorting in "default project" combobox changes after selection - [#6194](https://github.com/NuGet/Home/issues/6194)

* [Regression] in 15.5p1: Portable0.0 is skipped - [#6105](https://github.com/NuGet/Home/issues/6105)

* Assets from packages are missing after restore - [#5995](https://github.com/NuGet/Home/issues/5995)

* Plugin credential providers do not work with URIs containing spaces - [#5982](https://github.com/NuGet/Home/issues/5982)

* NuGet support for source build of NetCore - [#5903](https://github.com/NuGet/Home/issues/5903)

* nuget.exe list PackageName -IncludeDelisted doesn't show unlisted package - [#5896](https://github.com/NuGet/Home/issues/5896)

* Visual Studio 2017 failing to install nuget package in .NET 4.7 project (path too long) - [#5835](https://github.com/NuGet/Home/issues/5835)

* NuGet UI in VS displays the wrong available versions of nuget packages. - [#5738](https://github.com/NuGet/Home/issues/5738)

* If package failed to restore, error should be printed i the output even with Minimal verbosity ON - [#5658](https://github.com/NuGet/Home/issues/5658)

* dotnet restore at solution-level doesn't follow ProjectReference with ReferenceOutputAssembly of false leading to random build failures - [#5490](https://github.com/NuGet/Home/issues/5490)

* Avoid user specific data from nuget props/targets files - [#4929](https://github.com/NuGet/Home/issues/4929)

* linux subsystem (bash) windows 10 - fails to communicate network - [#4844](https://github.com/NuGet/Home/issues/4844)

* Auto-complete in PMC works incorrectly with object methods - [#4800](https://github.com/NuGet/Home/issues/4800)

* Unable to nuget.exe delete on a prerelease build - [#4746](https://github.com/NuGet/Home/issues/4746)

* Migrated project shows warning when packed. - [#4718](https://github.com/NuGet/Home/issues/4718)

* nuget.exe restore fails with Visual Studio 2015 toolset - [#4713](https://github.com/NuGet/Home/issues/4713)

* Visual Studio package manager not updated when NuGet.config is edited - [#4285](https://github.com/NuGet/Home/issues/4285)

* update packages on .net core projects not working - [#4278](https://github.com/NuGet/Home/issues/4278)

* perf - pmc is expensive to instantiate in vs2017 - [#4205](https://github.com/NuGet/Home/issues/4205)

* Latest NuGet PACK yields PREVIOUS version AssemblyVersionAttribute values - [#4149](https://github.com/NuGet/Home/issues/4149)

* VSFeedback: Update of Microsoft.Owin.Security.Cookies took long time - [#4111](https://github.com/NuGet/Home/issues/4111)

* Slow to get dependency information on slow connection - [#4089](https://github.com/NuGet/Home/issues/4089)

* Feature Request: Enhance support for Management Pack projects (mpproj) - [#4068](https://github.com/NuGet/Home/issues/4068)

* uninstall-package w/ -RemoveDependencies will fail if multiple packages share a common dependency - [#4026](https://github.com/NuGet/Home/issues/4026)

* Error in Nuget restore command - nuget 2.8.6 - [#4010](https://github.com/NuGet/Home/issues/4010)

* Can't enable package source from local config if that source is disabled in global config. - [#3608](https://github.com/NuGet/Home/issues/3608)

* Move NuGet.targets to contentFiles in NuGet.Build.Tasks - [#3603](https://github.com/NuGet/Home/issues/3603)

* Version conflicts can occur when a project is used instead of a package - [#3602](https://github.com/NuGet/Home/issues/3602)

* Remove noninteractive option from nuget.exe pack command - [#3598](https://github.com/NuGet/Home/issues/3598)

* Logging of nuget.exe push does not agree with source parameter - [#3597](https://github.com/NuGet/Home/issues/3597)

* Consider: nuget locals should still support nuget.exe locals -clear packages-cache - [#3591](https://github.com/NuGet/Home/issues/3591)

* Finalize NuGet.Core.nupkg for publishing - [#3581](https://github.com/NuGet/Home/issues/3581)

* NuGet pack resolves dependency ID from directory name when -IncludeProjectReferences is used for csproj + project.json - [#3566](https://github.com/NuGet/Home/issues/3566)

* Unable to write any unit test because of Nuget - [#3254](https://github.com/NuGet/Home/issues/3254)

* The type initializer for 'NuGet.ProxyCache' threw an exception - [#3144](https://github.com/NuGet/Home/issues/3144)

* nuget restore performance issue with kudu - [#3087](https://github.com/NuGet/Home/issues/3087)

* VS2015 freezes on startup loading Nuget Packages - [#2175](https://github.com/NuGet/Home/issues/2175)

* UI Client fails to show any error or warning when search is ahead of registration blobs - [#2149](https://github.com/NuGet/Home/issues/2149)

* Restore summary has incorrect "packages installed" output - [#2139](https://github.com/NuGet/Home/issues/2139)

* Get-Packages -Updates generates an incorrect query - [#2135](https://github.com/NuGet/Home/issues/2135)

* NuGet 3 dependency resolution model breaks the ability to use Debug builds - [#2120](https://github.com/NuGet/Home/issues/2120)

* Dependency version resolution appears broken - [#2118](https://github.com/NuGet/Home/issues/2118)

* Inconsistent Behavior in Packages View in Package Manager UI - [#2111](https://github.com/NuGet/Home/issues/2111)

* Uninstall packages tries to install old version first - [#2050](https://github.com/NuGet/Home/issues/2050)

* Managa Packages for Solution UX in VS 2015 - [#2030](https://github.com/NuGet/Home/issues/2030)

* Unhelpful error message - [#2027](https://github.com/NuGet/Home/issues/2027)

* nuget.commandline 3.3.0 -Source not adhering to source - [#2018](https://github.com/NuGet/Home/issues/2018)

* nuget.exe update -Id not working when project has not restored packages ? - [#2013](https://github.com/NuGet/Home/issues/2013)

* Packages with deep content get partially unpacked and an incorrect error result of "Package Not Found" - [#1727](https://github.com/NuGet/Home/issues/1727)


**Docs:**

* We need docs for AssetTargetFallback, both generally and migrating from PackageTargetFallback . - [#5998](https://github.com/NuGet/Home/issues/5998)

* No documentation? - [#4890](https://github.com/NuGet/Home/issues/4890)

* Project style dialog should not apply to templates - [#4748](https://github.com/NuGet/Home/issues/4748)

* Project.json docs page contains link-to-self for Dependency Resolution. - [#4107](https://github.com/NuGet/Home/issues/4107)

* NuGet docs do not specify behavior for ./tools or install.ps1 args - [#3656](https://github.com/NuGet/Home/issues/3656)


## Link to GitHub issues fixed in 4.5 RTM

[Issues list](https://github.com/NuGet/Home/issues?q=is%3Aissue+milestone%3A4.5+is%3Aclosed)
