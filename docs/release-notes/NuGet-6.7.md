---
title: NuGet 6.7 Release Notes
description: Release notes for NuGet 6.7 including new features, bug fixes, and DCRs.
author: jeffkl
ms.author: jeffkl
ms.date: 7/31/2023
ms.topic: conceptual
---

# NuGet 6.7 Release Notes

NuGet distribution vehicles:

| NuGet version | Available in Visual Studio version | Available in .NET SDK(s) |
|:---|:---|:---|
| [**6.7**](https://nuget.org/downloads) | [Visual Studio 2022 version 17.7](https://visualstudio.microsoft.com/downloads/) | [7.0.400](https://dotnet.microsoft.com/download/dotnet-core/7.0)<sup>1</sup> |
| [**6.7.1**](https://nuget.org/downloads) | [Visual Studio 2022 version 17.7](https://visualstudio.microsoft.com/downloads/) | [7.0.406](https://dotnet.microsoft.com/download/dotnet-core/7.0)<sup>1</sup> |

<sup>1</sup> Installed with Visual Studio 2022 with.NET Core workload

## Summary: What's New in 6.7.1

* [Security]: Microsoft Security Advisory CVE-2024-0057 | NuGet Client Security Feature bypass Vulnerability - [#12653](https://github.com/NuGet/Home/issues/13241)

## Summary: What's New in 6.7

* Package Source Mapping status for selected package in Details Pane - [#12586](https://github.com/NuGet/Home/issues/12586)

* Add VulnerabilityInfo APIs into NuGet.Protocol - [#12518](https://github.com/NuGet/Home/issues/12518)

* Signing:  raise actionable message on Linux if verification results in untrusted failure - [#12459](https://github.com/NuGet/Home/issues/12459)

* [Feature]: Show which package versions are vulnerable in the VS PMUI package details pane version dropdown - [#11127](https://github.com/NuGet/Home/issues/11127)

### Issues fixed in this release

**DCRs:**

* There are no visual indicators for the Package Source Mapping status in details pane - [#12609](https://github.com/NuGet/Home/issues/12609)

* VS Options shortcut from PMUI for PackageSourceMappings doesn't scroll to or select the Package - [#12608](https://github.com/NuGet/Home/issues/12608)

* Install/Update buttons are enabled in Details Pane when the PackageSourceMapping is not found - [#12607](https://github.com/NuGet/Home/issues/12607)

* Add nullable annotations to NuGet.Frameworks - [#12570](https://github.com/NuGet/Home/issues/12570)

* NuGet should use a different property for the platform version for C++/CLI - [#12521](https://github.com/NuGet/Home/issues/12521)

* NuGet should use HttpClientHandler.PreAuthentication to reduce HTTP 401's - [#12514](https://github.com/NuGet/Home/issues/12514)

**Bugs:**

* Create SingleFileProvider and use it for content files - [#12706](https://github.com/NuGet/Home/issues/12706)

* Restore task dumps stack because TaskCanceledException should be OperationCanceledException - [#12700](https://github.com/NuGet/Home/issues/12700)

* Improve nuget.exe restore error message when passing file globs - [#12691](https://github.com/NuGet/Home/issues/12691)

* NuGet: LockFileUtils.GetLockFileItems boxing enumerator - [#12684](https://github.com/NuGet/Home/issues/12684)

* Preview Window needs some strings reworded and margins adjusted - [#12681](https://github.com/NuGet/Home/issues/12681)

* PackageSpecificWarningProperties classes do redundant collection lookups - [#12678](https://github.com/NuGet/Home/issues/12678)

* Specify SelectionCriteria list capacity correctly - [#12667](https://github.com/NuGet/Home/issues/12667)

* Avoid value lookup in foreach loop over dictionary's keys - [#12666](https://github.com/NuGet/Home/issues/12666)

* NuGet: VersionRangeFormatter.GetNormalizedString bypassing StringBuilderCache via use of string.format - [#12664](https://github.com/NuGet/Home/issues/12664)

* NuGet: LockFileFormat.ReadTargetLibrary using string.split on a simple pattern - [#12663](https://github.com/NuGet/Home/issues/12663)

* Performance: Don't allocate as many Task instances - [#12659](https://github.com/NuGet/Home/issues/12659)

* Replace unreliable assembly location code with reliable one - [#12650](https://github.com/NuGet/Home/issues/12650)

* PackageSpec should use an empty RuntimeGraph instead of a new one - [#12649](https://github.com/NuGet/Home/issues/12649)

* TargetFrameworkInformation.Clone calls ToDictionary on a type that is already a dictionary, TargetFrameworkInformation.Clone resizes a dictionary it already knows the destination size - [#12648](https://github.com/NuGet/Home/issues/12648)

* PackageSpecReferenceDependencyProvider.GetLibrary unnecessarily resizes a List&lt;T&gt; that it doesn't even need - [#12647](https://github.com/NuGet/Home/issues/12647)

* ResolverUtility.FindLibraryCachedAsync should use a struct as lookup - [#12646](https://github.com/NuGet/Home/issues/12646)

* ContentItemCollection.PopulateItemGroups unnecessarily causing resizes of List&lt;T&gt;, ContentItemCollection.PopulateItemGroups boxing List&lt;T&gt;.Enumerator - [#12645](https://github.com/NuGet/Home/issues/12645)

* PackageSpec.Clone and LibraryDependency.Clone overwrite collections created by their constructors. - [#12642](https://github.com/NuGet/Home/issues/12642)

* RestoreOperationLogger.ReportProgressAsync repeatedly requests UI thread time - [#12640](https://github.com/NuGet/Home/issues/12640)

* Avoid repeated Enum.ToString() in PackageSpecWriter.SetDependencies - [#12638](https://github.com/NuGet/Home/issues/12638)

* ETW events should use default '/' instead of '_' - [#12631](https://github.com/NuGet/Home/issues/12631)

* Parsing NuGetVersion causes significant GC pressure - [#12630](https://github.com/NuGet/Home/issues/12630)

* The vulnerable label doesn’t show in the “version” dropdown box of “Browse” tab when searching for vulnerable packages  - [#12623](https://github.com/NuGet/Home/issues/12623)

* nuget restore fails for solution filters not in same directory as the solution it references. - [#12562](https://github.com/NuGet/Home/issues/12562)

* VersionRangeFormatter should use StringBuilderPool - [#12551](https://github.com/NuGet/Home/issues/12551)

* Reduce allocations in VirtualFileInfo.Name - [#12550](https://github.com/NuGet/Home/issues/12550)

* Reduce allocations when getting hash code of LibraryModel.LibraryRange - [#12549](https://github.com/NuGet/Home/issues/12549)

* NuGet.Build.Tasks.Console should roll forward to newer runtimes - [#12528](https://github.com/NuGet/Home/issues/12528)

* SourceRepository.GetResource throws if type is not an exact match - [#12455](https://github.com/NuGet/Home/issues/12455)

* [Bug]: Disable the option to update version when using VersionOverride in CPM - [#12230](https://github.com/NuGet/Home/issues/12230)

* [Bug]: dotnet nuget push not detecting apikey for 3rd party symbol server - [#11846](https://github.com/NuGet/Home/issues/11846)

* X-NuGet-Warning doesn't work when using proxy due to missing ServerWarningLogHandler - [#5004](https://github.com/NuGet/Home/issues/5004)

[List of commits in this release](https://github.com/NuGet/NuGet.Client/compare/6.7.0.127...6.6.1.2)

### Community contributions

Thank you to all the contributors who helped make this NuGet release awesome!

* [danmoseley](https://github.com/danmoseley)
  * [5276](https://github.com/NuGet/NuGet.Client/pull/5276) fix crash on cancel in Restore task
* [oleksandr-didyk](https://github.com/oleksandr-didyk)
  * [5196](https://github.com/NuGet/NuGet.Client/pull/5196) add review comment to sb files
* [drewnoakes](https://github.com/drewnoakes)
  * [5200](https://github.com/NuGet/NuGet.Client/pull/5200) Reduce allocations in ContentItemCollection
* [Erarndt](https://github.com/Erarndt)
  * [5202](https://github.com/NuGet/NuGet.Client/pull/5202) Avoid allocations while parsing NuGetVersion from strings
* [jerhon](https://github.com/jerhon)
  * [5197](https://github.com/NuGet/NuGet.Client/pull/5197) Fix issue with solution filters not restoring when in different folder than referenced solution
* [NikolaMilosavljevic](https://github.com/NikolaMilosavljevic)
  * [5228](https://github.com/NuGet/NuGet.Client/pull/5228) Add System.Security.Cryptography.Xml dependency
* [DevPaulLiu](https://github.com/DevPaulLiu)
  * [5206](https://github.com/NuGet/NuGet.Client/pull/5206) Use default '/' split symbol in ETW events.
* [drewnoakes](https://github.com/drewnoakes)
  * [5201](https://github.com/NuGet/NuGet.Client/pull/5201) Reduce allocations in PackageSpecReferenceDependencyProvider
* [drewnoakes](https://github.com/drewnoakes)
  * [5199](https://github.com/NuGet/NuGet.Client/pull/5199) Reduce allocations in TargetFrameworkInformation.Clone
* [Erarndt](https://github.com/Erarndt)
  * [5217](https://github.com/NuGet/NuGet.Client/pull/5217) Ensure only one logging task is active at a time
* [Erarndt](https://github.com/Erarndt)
  * [5219](https://github.com/NuGet/NuGet.Client/pull/5219) Update PackageSpec.Clone and LibraryDependency.Clone to avoid allocations
* [Erarndt](https://github.com/Erarndt)
  * [5215](https://github.com/NuGet/NuGet.Client/pull/5215) Add AsString() for LibraryDependencyTarget and LibraryIncludeFlags
* [NikolaMilosavljevic](https://github.com/NikolaMilosavljevic)
  * [5207](https://github.com/NuGet/NuGet.Client/pull/5207) Add dependencies for PVP flow
* [NikolaMilosavljevic](https://github.com/NikolaMilosavljevic)
  * [5193](https://github.com/NuGet/NuGet.Client/pull/5193) Target net8.0 for source-build
* [mthalman](https://github.com/mthalman)
  * [5180](https://github.com/NuGet/NuGet.Client/pull/5180) Exclude WPF projects from source-build
* [NikolaMilosavljevic](https://github.com/NikolaMilosavljevic)
  * [5190](https://github.com/NuGet/NuGet.Client/pull/5190) Enable source-build pre-built detection
* [drewnoakes](https://github.com/drewnoakes)
  * [5146](https://github.com/NuGet/NuGet.Client/pull/5146) Show diagnostic beneath unresolved package/project reference in Solution Explorer
* [0xced](https://github.com/0xced)
  * [5021](https://github.com/NuGet/NuGet.Client/pull/5021) Log warnings from server also when using an http proxy (X-NuGet-Warning)
* [jwfx](https://github.com/jwfx)
  * [5122](https://github.com/NuGet/NuGet.Client/pull/5122) Fall back to using API key also for pushing symbol packages if nothing else was specified as parameter or config
* [MichaelSimons](https://github.com/MichaelSimons)
  * [5132](https://github.com/NuGet/NuGet.Client/pull/5132) Remove MinimalTargetFrameworksExeSigning from MinimalTargetFrameworksExeSigning in source-build
* [dfederm](https://github.com/dfederm)
  * [5125](https://github.com/NuGet/NuGet.Client/pull/5125) Add RollForward to NuGet.Build.Tasks.Console
* [atamagaii](https://github.com/atamagaii)
  * [5107](https://github.com/NuGet/NuGet.Client/pull/5107) Add missing RegistrationsBaseUrls to prevent exceptions when loading valid Service Indexes.
