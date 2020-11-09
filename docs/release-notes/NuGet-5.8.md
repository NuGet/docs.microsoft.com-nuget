---
title: NuGet 5.8 Release Notes
description: Release notes for NuGet 5.8 including new features, bug fixes, and DCRs.
author: <GithubAlias>
ms.author: <MicrosoftAlias>
ms.date: 11/9/2020
ms.topic: conceptual
---

# NuGet 5.8 Release Notes

NuGet distribution vehicles:

| NuGet version | Available in Visual Studio version | Available in .NET SDK(s) |
|:---|:---|:---|
| [**5.8**](https://nuget.org/downloads) | [Visual Studio 2019 version 16.8](https://visualstudio.microsoft.com/downloads/) | [<SDKVersion>](https://dotnet.microsoft.com/download/dotnet-core/<SDKMajorMinorVersionOnly>)<sup>1</sup> |

<sup>1</sup> Installed with Visual Studio <VSYear> with.NET Core workload

## Summary: What's New in 5.8

* Package manager sources use remote types from server - [#9608](https://github.com/NuGet/Home/issues/9608)

* Search CLI Tool for NuGet.exe - [#9704](https://github.com/NuGet/Home/issues/9704)

* Add a TypeConverter for the SemanticVersion class - [#9125](https://github.com/NuGet/Home/issues/9125)

* Improve the CPVM Visual Studio experience for the case when PackageReference items have version. - [#9507](https://github.com/NuGet/Home/issues/9507)

* VSOE saving package sources in tools options will overwrite credentials - [#9711](https://github.com/NuGet/Home/issues/9711)

* Replay the warnings for LegacyPackageReference projects when a project no-ops in partial restore  - [#9565](https://github.com/NuGet/Home/issues/9565)

* dotnet list should support --verbosity - [#9600](https://github.com/NuGet/Home/issues/9600)

* Add "prerelease" option to dotnet add package - [#4699](https://github.com/NuGet/Home/issues/4699)

* recommend packages for 'All' sources - [#9872](https://github.com/NuGet/Home/issues/9872)

* Surface basic vulnerability metadata in PM UI view models - [#9850](https://github.com/NuGet/Home/issues/9850)

* Signing: implement dotnet verify command - [#8051](https://github.com/NuGet/Home/issues/8051)

### Issues fixed in this release

**DCRs:**

* Net5 TFM: Framework Precedence Rules - [#9436](https://github.com/NuGet/Home/issues/9436)

* NuGet should not infer dots platform version when parsing TargetFramework - [#9842](https://github.com/NuGet/Home/issues/9842)

* Support Nuget.Packaging and transitive closure for net45 - [#9883](https://github.com/NuGet/Home/issues/9883)

* Update GetReferenceNearestTargetFrameworkTask  to support target frameworks with platforms (such as .NET 5-windows) - [#9894](https://github.com/NuGet/Home/issues/9894)

* NuGet should use TargetFrameworkMoniker & TargetPlatformMoniker to infer the frameworks instead of using the individual TFI, TFV, TPI, TPV properties - [#9895](https://github.com/NuGet/Home/issues/9895)

* net5.0 VS APIs - [#9650](https://github.com/NuGet/Home/issues/9650)

* Nuget Solution Package manager UI is slow because restore is run for every project instead of after all projects are updated  - [#6010](https://github.com/NuGet/Home/issues/6010)

* vs: consolidate or update packages should not be blocked due to resulting errors (Detected package downgrade error Nu1605, etc..) - [#9224](https://github.com/NuGet/Home/issues/9224)

* NuGet features should light up for projects that have the capability; "PackageReferences" - [#9957](https://github.com/NuGet/Home/issues/9957)

**Bugs:**

* OutputWindowTextWriter constructor should not be called on background thread. - [#9764](https://github.com/NuGet/Home/issues/9764)

* Restore signed packages on Big Endian CPUs - [#9547](https://github.com/NuGet/Home/issues/9547)

* Typo in error message. "administator" instead of "administrator" - [#9662](https://github.com/NuGet/Home/issues/9662)

* OutputConsoleLogger should not call affinitized methods in MEF constructors - [#9591](https://github.com/NuGet/Home/issues/9591)

* [CPVM-OnBoard] Rejected transitive central dependencies should be removed from the restore graph. - [#9483](https://github.com/NuGet/Home/issues/9483)

* Bug in NuGet.CommandLine.Console's PrintJustified method - [#9737](https://github.com/NuGet/Home/issues/9737)

* Package Manager UI has a memory leak where the the package metadata doesn't get GCed due to a bad binding - [#9757](https://github.com/NuGet/Home/issues/9757)

* [Test Failure][Signing] No warning is showed in Error List when installing a signed package with ‘Package config’ format in PM UI - [#9798](https://github.com/NuGet/Home/issues/9798)

* NuGet.CommandLine.XPlat should not have public APIs - [#9821](https://github.com/NuGet/Home/issues/9821)

* Reduce resource contention at solution load time cause by blocking a threadpool thread with BlockingCollection.Take - [#9822](https://github.com/NuGet/Home/issues/9822)

* NuGet Pack with invalid AssemblyInformationalVersion value reports "description is required" - [#5548](https://github.com/NuGet/Home/issues/5548)

* Enable PM UI experiment - package recommender - [#9845](https://github.com/NuGet/Home/issues/9845)

* RepositoryMetadata Equals does not account for branch and commit properties - [#9613](https://github.com/NuGet/Home/issues/9613)

* In commandline restore, with multi targeted projects, NuGet should read the target framework related information from the inner build. - [#9869](https://github.com/NuGet/Home/issues/9869)

*  Read the runtime identifier graph through the TargetFrameworkInformation item, improve the test coverage of dotnet tool scenarios. - [#9874](https://github.com/NuGet/Home/issues/9874)

* Static graph restore is inconsistent wrt to the CrossTargeting property in compared to VS & regular msbuild evaluation restore  - [#9881](https://github.com/NuGet/Home/issues/9881)

* In static graph restore, with multi targeted projects, NuGet should read the target framework related information from the inner build. - [#9870](https://github.com/NuGet/Home/issues/9870)

* Allow `net5.0-platform` projects to be loaded and restored in VS - [#9863](https://github.com/NuGet/Home/issues/9863)

* Display the resolved version in the PMUI. - [#9826](https://github.com/NuGet/Home/issues/9826)

* Solution explorer not showing all nuget package dependencies - [#9898](https://github.com/NuGet/Home/issues/9898)

* Unnecessary assets file writes can lead to extra design time builds - [#9903](https://github.com/NuGet/Home/issues/9903)

* [CPVM] Error on floating transitive dependencies defined in Directory.Packages.props - [#9384](https://github.com/NuGet/Home/issues/9384)

* Update the SPDX license list - [#9946](https://github.com/NuGet/Home/issues/9946)

* Error when a Project has CPVM enabled but there are not any PackageVersion items defined - [#9341](https://github.com/NuGet/Home/issues/9341)

* VS 2019 crashes after opening Manage Nuget Packages - icon causes unhandled exception in image conversion. - [#9696](https://github.com/NuGet/Home/issues/9696)

* nuget.packaging.extraction needs ilmerge to not contain newtonsoft.json - [#9966](https://github.com/NuGet/Home/issues/9966)

* Packing with ContinuePackingAfterGeneratingNuspec=false should not fail when there are no errors - [#9786](https://github.com/NuGet/Home/issues/9786)

* Clicking NU code in VS ErrorList should go to docs.microsoft.com/nuget/reference - [#9934](https://github.com/NuGet/Home/issues/9934)

* Use https for placeholder text when adding new package source through Visual Studio GUI - [#9974](https://github.com/NuGet/Home/issues/9974)

* [CPVM] Cannot import packages file from a custom directory - [#9841](https://github.com/NuGet/Home/issues/9841)

* RuntimeEnvironmentHelper.IsRunningOnVisualStudio performance issue on Mono - [#9989](https://github.com/NuGet/Home/issues/9989)

* Restore on solution load runs after the first 50 nominations instead of waiting for nominations to come in. - [#9982](https://github.com/NuGet/Home/issues/9982)

* Central package management throws errors about duplicate dictionary keys when attempting to create the lock file - [#9965](https://github.com/NuGet/Home/issues/9965)

* PMUI Icons aren't inverting colors properly - [#10017](https://github.com/NuGet/Home/issues/10017)

* Restore reports incorrect project count numbers for up to date projects & no-op projects.  - [#10026](https://github.com/NuGet/Home/issues/10026)

* Using /p:RestoreUseStaticGraphEvaluation=true Results in Value Cannot Be Null - [#9280](https://github.com/NuGet/Home/issues/9280)

* PM UI:  NullReferenceException when signature validation fails - [#10042](https://github.com/NuGet/Home/issues/10042)

* Instead of waiting for nominations or 20s whichever comes first, implement a sliding window at solution load, which should allow us to run exactly 1 restore even for large solutions - [#9984](https://github.com/NuGet/Home/issues/9984)

* Restore: excessive deep cloning of ProjectSpec, remove cloning in no-op hash creation for PR projects - [#10052](https://github.com/NuGet/Home/issues/10052)

* VS OE:  should not use object data type for project metadata values  - [#10055](https://github.com/NuGet/Home/issues/10055)

* Dotnet Pack mistakenly uses alias for WPF Library projects - [#10020](https://github.com/NuGet/Home/issues/10020)

**Nones:**

* FileAndForget posts a new event for every point of usage - [#9812](https://github.com/NuGet/Home/issues/9812)

* Document RPS tests, including a playbook for investigating issues. - [#9781](https://github.com/NuGet/Home/issues/9781)

* Suppress restore messages for noop restore in VS - [#6384](https://github.com/NuGet/Home/issues/6384)

**StillOpens:**

* Implement a pre-registration mechanism for legacy PR projects that call restore at solution open - [#9986](https://github.com/NuGet/Home/issues/9986)

* Remove temporary fix on HttpRequestMessage.Options - [#9981](https://github.com/NuGet/Home/issues/9981)

* Telemetry: Replace EmitTelemetryEvent with proper telemetry activities - [#9581](https://github.com/NuGet/Home/issues/9581)

**[List of all issues fixed in this release - 5.8](https://app.zenhub.com/workspaces/nuget-client-team-55aec9a240305cf007585881/reports/release?release=5f03519b777e78b4ffb2edeb)**
