---
title: NuGet 5.7 Release Notes
description: Release notes for NuGet 5.7 including new features, bug fixes, and DCRs.
author: nkolev92
ms.author: nikolev
ms.date: 8/14/2020
ms.topic: conceptual
---

# NuGet 5.7 Release Notes

NuGet distribution vehicles:

| NuGet version | Available in Visual Studio version | Available in .NET SDK(s) |
|:---|:---|:---|
| [**5.7.0**](https://nuget.org/downloads) | [Visual Studio 2019 version 16.7](https://visualstudio.microsoft.com/downloads/) | [3.1.401](https://dotnet.microsoft.com/download/dotnet-core/3.1)<sup>1</sup> |
| [**5.7.1**](https://nuget.org/downloads) | [Visual Studio 2019 version 16.7](https://visualstudio.microsoft.com/downloads/) | [3.1.408](https://dotnet.microsoft.com/download/dotnet-core/3.1)<sup>1</sup> |
| [**5.7.2**](https://nuget.org/downloads) | N/A | [3.1.420](https://dotnet.microsoft.com/download/dotnet-core/3.1)<sup>1</sup> |

<sup>1</sup> Installed with Visual Studio 2019 with .NET Core workload

## Summary: What's New in 5.7.2

* [Security]: Microsoft Security Advisory CVE 2022-30184 | .NET Information Disclosure Vulnerability - [#11883](https://github.com/NuGet/Home/issues/1188

## Summary: What's New in 5.7

### Features added in this release

* Added extern alias support for NuGet package references - [#4989](https://github.com/NuGet/Home/issues/4989)

* Made switching between Installed and Updates tabs faster by allowing them to share a data source and reducing resfreshing - [#8294](https://github.com/NuGet/Home/issues/8294)

* Make restore faster - speed up evaluations by calling MSBuild Static Graph apis (dotnet.exe) - [#9644](https://github.com/NuGet/Home/issues/9644)

* Added Visual Studio partial restore for PackageReference projects (no-op++) - [#9513](https://github.com/NuGet/Home/issues/9513)

* Visual Studio Package Manager UI will crash less often when searching misbehaving package sources that return more than the requested number of results per HTTP request. - [#8478](https://github.com/NuGet/Home/issues/8478)

* Added integration of PackageVersion information for non-SDK style projects in VS restore  - [#9236](https://github.com/NuGet/Home/issues/9236)

* Added support for nuget.exe update `-self -Source` https://feed - [#1783](https://github.com/NuGet/Home/issues/1783)

* Added support for multiple config files in %APPDATA%\NuGet directory - [#9394](https://github.com/NuGet/Home/issues/9394)

* DeterministicSourcePaths now takes NuGet source packages into account - [#9431](https://github.com/NuGet/Home/issues/9431)

* Added INuGetProjectService.GetInstalledPackagesAsync extensibility API - [#9702](https://github.com/NuGet/Home/issues/9702)

* Added interop API to enumerate fallback folders without requiring a solution/project - [#9395](https://github.com/NuGet/Home/issues/9395)

* Added `latest` option for `-MSBuildVersion` - [#8808](https://github.com/NuGet/Home/issues/8808)

### Issues fixed in this release

**Bugs:**

* In a dotnet CLI restore, when launching credential plugins, try the dotnet CLI on the system path if the `DOTNET_HOST_PATH`  environment variable is not defined. - [#7438](https://github.com/NuGet/Home/issues/7438)

* nuget.exe spec generates a copyright tag with hard-coded text of Copyright YYYY Instead of `$copyright$` - [#8696](https://github.com/NuGet/Home/issues/8696)

* NuGet.exe throws exception 'authors required' during pack of a csproj ignoring placeholders and assemblyinfo attributes if the assembly name is changed - [#4234](https://github.com/NuGet/Home/issues/4234)

* HttpRequestMessage gets reused multiple times which is not supported with the SocketHttpHandler - [#8661](https://github.com/NuGet/Home/issues/8661)

* NuGet.Indexing 5.6.0 preview 3 and later use a different public key token - [#9481](https://github.com/NuGet/Home/issues/9481)

* Honor TreatWarningsAsErrors during NuGet Package creation - [#7404](https://github.com/NuGet/Home/issues/7404)

* [CPVM] Spurious package downgrades for multiple p2p projects  - [#9549](https://github.com/NuGet/Home/issues/9549)

* The “Browse” tab is not aligned left with search box - [#9559](https://github.com/NuGet/Home/issues/9559)

* The installed version is inconsistent with the embedded icon in the solution level PM UI for one package id with multiple versions installed - [#9321](https://github.com/NuGet/Home/issues/9321)

* Leak: PartCreationPolicy(CreationPolicy.NonShared) NuGet.SolutionRestoreManager.RestoreOperationLogger - [#9595](https://github.com/NuGet/Home/issues/9595)

* Avoid reading the assets file in no-op restores - [#9693](https://github.com/NuGet/Home/issues/9693)

* NuGet.Protocol does not support getting a version's download count from search - [#9086](https://github.com/NuGet/Home/issues/9086)

* Improve memory performance of PackageMetadataResourceV3 by reducing the JObject dependencies - [#9719](https://github.com/NuGet/Home/issues/9719)

**Design change requests:**

* Suppressed the `<owners>` element when it is redundant - [#5134](https://github.com/NuGet/Home/issues/5134)

* Log IntervalTrackers as ETW events - [#9593](https://github.com/NuGet/Home/issues/9593)

* Added an informational message on restore to inform CPVM users that the feature is in preview - [#9340](https://github.com/NuGet/Home/issues/9340)

* Populate Solution Explorer package/project transitive dependencies from assets file - [#9580](https://github.com/NuGet/Home/issues/9580)

* Installed packages tab shouldn't paginate the packages list - [#6995](https://github.com/NuGet/Home/issues/6995)

**[List of all issues fixed in this release - 5.7](https://app.zenhub.com/workspaces/nuget-client-team-55aec9a240305cf007585881/reports/release?release=5ea77f51ab1a972297db2e92)**

### Community contributions

Thank you to all the contributors who helped make this NuGet release awesome!

|Who|PRs|Issues|
|----|----|----|
|[campersau](https://github.com/campersau)|[3433](https://github.com/NuGet/NuGet.Client/pull/3433), [3120](https://github.com/NuGet/NuGet.Client/pull/3120)|NuGet.Protocol does not support getting a version's download count from search - [#9086](https://github.com/NuGet/Home/issues/9086) </br>HttpRequestMessage gets reused multiple times which is not supported with the SocketHttpHandler - [#8661](https://github.com/NuGet/Home/issues/8661)|
|[Joseph Musser (jnm2)](https://github.com/jnm2)|[3241](https://github.com/NuGet/NuGet.Client/pull/3241)|Suppressed the `<owners>` element when it is redundant - [#5134](https://github.com/NuGet/Home/issues/5134)|
|[Volodymyr Shkolka (BlackGad)](https://github.com/BlackGad)|[3273](https://github.com/NuGet/NuGet.Client/pull/3273)|NuGet cannot restore from HTTPS sources that require Client Certificates - [#5773](https://github.com/NuGet/Home/issues/5773)|
|[Marius Ungureanu (Therzok)](https://github.com/Therzok)|[3357](https://github.com/NuGet/NuGet.Client/pull/3357)|HttpSourceAuthenticationHandler SemaphoreSlim future proofing - [#9463](https://github.com/NuGet/Home/issues/9463)|
|[Sunner (SuNNjek)](https://github.com/SuNNjek)|[3088](https://github.com/NuGet/NuGet.Client/pull/3088)|nuget.exe spec generates a copyright tag with hard-coded text of Copyright YYYY Instead of `$copyright$` - [#8696](https://github.com/NuGet/Home/issues/8696)|
|[Olivier Spinelli (olivier-spinelli)](https://github.com/olivier-spinelli)|[3335](https://github.com/NuGet/NuGet.Client/pull/3335)|In a dotnet CLI restore, when launching credential plugins, try the dotnet CLI on the system path if the `DOTNET_HOST_PATH`  environment variable is not defined. - [#7438](https://github.com/NuGet/Home/issues/7438)|
|[goyzhang](https://github.com/goyzhang)|[3370](https://github.com/NuGet/NuGet.Client/pull/3370)|Added `latest` option for `-MSBuildVersion` - [#8808](https://github.com/NuGet/Home/issues/8808)|

## Summary: What's New in 5.7.1

* Extend the .nupkg.metadata file to include the installation source - [#10354](https://github.com/NuGet/Home/issues/10354)

* Log package contenthash during restore logging (during extraction) - [#10384](https://github.com/NuGet/Home/issues/10384)

* When restoring at normal verbosity, log which source a package is being restored from - [#10461](https://github.com/NuGet/Home/issues/10461)

**[List of all issues fixed in this release - 5.7.1](https://app.zenhub.com/workspaces/nuget-client-team-55aec9a240305cf007585881/reports/release?release=6075f5724f84579cc29a79ee)**

**[List of commits in this release - 5.7.1](https://github.com/NuGet/NuGet.Client/compare/80512866a2c127e52ce3e86fd803fff77e9b9b52...5.7.1.4)**
