---
title: NuGet 5.8 Release Notes
description: Release notes for NuGet 5.8 including new features, bug fixes, and DCRs.
author: JonDouglas
ms.author: jodou
ms.date: 11/9/2020
ms.topic: release-notes
---

# NuGet 5.8 Release Notes

NuGet distribution vehicles:

| NuGet version | Available in Visual Studio version | Available in .NET SDK(s) |
|:---|:---|:---|
| [**5.8**](https://nuget.org/downloads) | [Visual Studio 2019 version 16.8](https://visualstudio.microsoft.com/downloads/) | [5.0](https://dotnet.microsoft.com/download/dotnet-core/5.0)<sup>1</sup> |
| [**5.8.1**](https://nuget.org/downloads) | [Visual Studio 2019 version 16.8.4](https://visualstudio.microsoft.com/downloads/) | |

<sup>1</sup> Installed with Visual Studio 2019 with .NET Core workload
  
> [!NOTE]
> Visual Studio 16.8, MSBuild 16.8, and .NET 5.0 require NuGet.exe 5.8 or later.


## Summary: What's New in 5.8
🎉 **This is the first release to offer full authoring and restoring support for NuGet packages targeting .NET 5.0** 🎉

* Speed up nupkg extraction using mmap/CreateFileMapping - [#9807](https://github.com/NuGet/Home/issues/9807)

* Display package vulnerability details in the Package Manager UI package details pane - [#9850](https://github.com/NuGet/Home/issues/9850)

* Verify signed NuGet packages with the new [`dotnet nuget verify`](/dotnet/core/tools/dotnet-nuget-verify) command - [#8051](https://github.com/NuGet/Home/issues/8051)

* [`dotnet add package`](/dotnet/core/tools/dotnet-add-package#:~:text=dotnet%20add%20package%201%20Name%202%20Synopsis%203,when%20targeting%20a%20specific%20framework.%20...%206%20Examples) supports `--prerelease` option to add the latest version of a package, including prerelease versions - [#4699](https://github.com/NuGet/Home/issues/4699)

* Search for packages in the CLI with [`nuget.exe search`](../reference/cli-reference/cli-ref-search.md) command - [#9704](https://github.com/NuGet/Home/issues/9704)

* [`dotnet list package`](/dotnet/core/tools/dotnet-list-package) command supports `--verbosity` option - [#9600](https://github.com/NuGet/Home/issues/9600)

* Enable fast No-Op restore optimization for csproj-style, PackageReference-based projects in Visual Studio - [#9565](https://github.com/NuGet/Home/issues/9565)

* Solution level Package Manager UI operations such as package installs and updates are up to 10x faster - [#6010](https://github.com/NuGet/Home/issues/6010)

* Several other NuGet performance improvements in Visual Studio - [#9982](https://github.com/NuGet/Home/issues/9982), [#9984](https://github.com/NuGet/Home/issues/9984), [#10052](https://github.com/NuGet/Home/issues/10052), [#9903](https://github.com/NuGet/Home/issues/9903)


### Issues fixed in this release

**DCRs:**

* .NET 5.0 TFM: Framework Precedence Rules - [#9436](https://github.com/NuGet/Home/issues/9436)

* NuGet should not infer dots platform version when parsing TargetFramework - [#9842](https://github.com/NuGet/Home/issues/9842)

* Use TargetFrameworkMoniker & TargetPlatformMoniker to infer the frameworks instead of using individual TFI, TFV, TPI, TPV properties - [#9895](https://github.com/NuGet/Home/issues/9895)

* Update `GetReferenceNearestTargetFrameworkTask()` to support target frameworks with platforms (such as net5.0-windows) - [#9894](https://github.com/NuGet/Home/issues/9894)

* .NET 5.0 Visual Studio APIs - [#9650](https://github.com/NuGet/Home/issues/9650)

* Package Manager UI: Consolidate or Update packages operations should not be blocked due to errors (Package Downgrade, etc.) - [#9224](https://github.com/NuGet/Home/issues/9224)

* NuGet features should light up for projects that have the capability; "PackageReferences" - [#9957](https://github.com/NuGet/Home/issues/9957)

* Suppress No-Op Restore messages in Visual Studio - [#6384](https://github.com/NuGet/Home/issues/6384)

**Bugs:**

* OutputWindowTextWriter constructor should not be called on background thread - [#9764](https://github.com/NuGet/Home/issues/9764)

* Restore signed packages on Big Endian CPUs - [#9547](https://github.com/NuGet/Home/issues/9547)

* OutputConsoleLogger should not call affinitized methods in MEF constructors - [#9591](https://github.com/NuGet/Home/issues/9591)

* Bug in NuGet.CommandLine.Console `PrintJustified()` method - [#9737](https://github.com/NuGet/Home/issues/9737)

* Package Manager UI memory leak when package metadata is garbage collected due to a bad binding - [#9757](https://github.com/NuGet/Home/issues/9757)

* [Signing] No warning is showed in Error List when installing a signed package with packages.config format in Package Manager UI - [#9798](https://github.com/NuGet/Home/issues/9798)

* NuGet.CommandLine.XPlat should not have public APIs - [#9821](https://github.com/NuGet/Home/issues/9821)

* Reduce resource contention at Solution Load time caused by blocking a threaded-pool thread with `BlockingCollection.Take()` - [#9822](https://github.com/NuGet/Home/issues/9822)

* In command line restore, with multi targeted projects, NuGet should read the target framework related information from the inner build - [#9869](https://github.com/NuGet/Home/issues/9869)

* Read Runtime Identifier graph through TargetFrameworkInformation item - [#9874](https://github.com/NuGet/Home/issues/9874)

* Static graph restore is inconsistent with regards to CrossTargeting property in compared to Visual Studio and regular MSBuild evaluation restore - [#9881](https://github.com/NuGet/Home/issues/9881)

* In static graph restore, with multi targeted projects, NuGet should read the target framework related information from the inner build. - [#9870](https://github.com/NuGet/Home/issues/9870)

* Allow `net5.0-platform` projects to be loaded and restored in Visual Studio - [#9863](https://github.com/NuGet/Home/issues/9863)

* Display the resolved version in the Package Manager UI - [#9826](https://github.com/NuGet/Home/issues/9826)

* Package Manager UI: Solution Explorer is not showing all NuGet package dependencies - [#9898](https://github.com/NuGet/Home/issues/9898)

* Update the SPDX license list - [#9946](https://github.com/NuGet/Home/issues/9946)

* VS 2019 crashes after opening Manage NuGet Packages: icon causes unhandled exception in image conversio - [#9696](https://github.com/NuGet/Home/issues/9696)

* NuGet.Packaging.Extraction needs ilmerge to exclude Newtonsoft.Json - [#9966](https://github.com/NuGet/Home/issues/9966)

* Packing with ContinuePackingAfterGeneratingNuspec=false should not fail when there are no errors - [#9786](https://github.com/NuGet/Home/issues/9786)

* Package Manager UI: Icons aren't inverting colors properly - [#10017](https://github.com/NuGet/Home/issues/10017)

* Incorrect project counts for Up-To-Date and No-Op projects at Restore - [#10026](https://github.com/NuGet/Home/issues/10026)

* Using `/p:RestoreUseStaticGraphEvaluation=true` Results in Value Cannot Be Null - [#9280](https://github.com/NuGet/Home/issues/9280)

* `dotnet pack` mistakenly uses alias for WPF Library projects - [#10020](https://github.com/NuGet/Home/issues/10020)

* Package Manager UI: NullReferenceException when signature validation fails - [#10042](https://github.com/NuGet/Home/issues/10042)

* Codespaces: do not use `object` type for project metadata values  - [#10055](https://github.com/NuGet/Home/issues/10055)

* Codespaces: saving package sources in tools options will overwrite credentials - [#9711](https://github.com/NuGet/Home/issues/9711)


**[List of all issues fixed in this release - 5.8](https://app.zenhub.com/workspaces/nuget-client-team-55aec9a240305cf007585881/reports/release?release=5f03519b777e78b4ffb2edeb)**

**[List of issues in this release - 5.8](https://github.com/NuGet/NuGet.Client/compare/5.7.0.6726...5.8.0.6930)**

### Community contributions

Thank you to all the contributors who helped make this NuGet release awesome!

|Who|PRs|Issues|
|----|----|----|
[omajid](https://github.com/omajid) | [3437](https://github.com/NuGet/NuGet.Client/pull/3437) | Typo in error message. "administator" instead of "administrator" - [#9662](https://github.com/NuGet/Home/issues/9662)
[odalet](https://github.com/odalet) | [3341](https://github.com/NuGet/NuGet.Client/pull/3341) | NuGet Pack with invalid AssemblyInformationalVersion reports "description is required" - [#5548](https://github.com/NuGet/Home/issues/5548)
[campersau](https://github.com/campersau) | [3501](https://github.com/NuGet/NuGet.Client/pull/3501) | `RepositoryMetadata.Equals()` does not account for Branch and Commit properties - [#9613](https://github.com/NuGet/Home/issues/9613)
[Youssef1313](https://github.com/Youssef1313) | [3599](https://github.com/NuGet/NuGet.Client/pull/3599) | Clicking NU code in Visual Studio Error List window should go to [Errors and warnings](/nuget/reference/errors-and-warnings/) - [#9934](https://github.com/NuGet/Home/issues/9934)
[ChrisMaddock](https://github.com/ChrisMaddock) | [3624](https://github.com/NuGet/NuGet.Client/pull/3624) | Use 'https://' when adding new package source through Visual Studio options - [#9974](https://github.com/NuGet/Home/issues/9974)
[Therzok](https://github.com/Therzok) | [3636](https://github.com/NuGet/NuGet.Client/pull/3636) | `RuntimeEnvironmentHelper.IsRunningOnVisualStudio` performance issue on Mono - [#9989](https://github.com/NuGet/Home/issues/9989)
[thomaslevesque](https://github.com/thomaslevesque) | [3442](https://github.com/NuGet/NuGet.Client/pull/3442) | Add a TypeConverter for the SemanticVersion class - [#9125](https://github.com/NuGet/Home/issues/9125)

## Summary: What's New in 5.8.1

* packages.config package.lock.json uses an incorrect target framework in 5.8 - [#10257](https://github.com/NuGet/Home/issues/10257)

* 5.8 + 16.8 Cannot resolve transitive project dependencies when mixing PackageReference and packages.config - [#10326](https://github.com/NuGet/Home/issues/10326)

**[List of all issues fixed in this release - 5.8.1](https://app.zenhub.com/workspaces/nuget-client-team-55aec9a240305cf007585881/reports/release?release=5ff7aeae16150e3b19910391)**

**[List of commits in this release - 5.8.1](https://github.com/NuGet/NuGet.Client/compare/5.8.0.6930...5.8.1.7021)**

## Feedback welcome

Your feedback is important to us.  If there are any problems with this release, check our
[GitHub Issues](https://github.com/NuGet/Home/issues) and
[Visual Studio Developer Community](https://developercommunity.visualstudio.com/)
for existing issues.  For new issues within NuGet, please report a
[GitHub Issue](https://github.com/NuGet/Home/issues/new).
For general NuGet experience issues, let us know via the
[Report a Problem](/visualstudio/ide/how-to-report-a-problem-with-visual-studio)
option found in your favorite IDE under **Help > Report a Problem**.
