---
title: NuGet 6.3 Release Notes
description: Release notes for NuGet 6.3 including new features, bug fixes, and DCRs.
author: mruizmares
ms.author: mruizmares
ms.date: 8/2/2022
ms.topic: conceptual
---

# NuGet 6.3 Release Notes

NuGet distribution vehicles:

| NuGet version | Available in Visual Studio version | Available in .NET SDK(s) |
|:---|:---|:---|
| [**6.3**](https://nuget.org/downloads) | [Visual Studio 17.4 version <VSVersion>](https://visualstudio.microsoft.com/downloads/) | [6.0.400](https://dotnet.microsoft.com/download/dotnet-core/<SDKMajorMinorVersionOnly>)<sup>1</sup> |

<sup>1</sup> Installed with Visual Studio 17.4 with.NET Core workload

## Summary: What's New in 6.3

* Implement "consuming pdbs from packages in PackageReference" - [#11352](https://github.com/NuGet/Home/issues/11352)

* Allow to user to input custom (floating) versions through the PM UI - [#9829](https://github.com/NuGet/Home/issues/9829)

* [Feature] PackageReference with a star in version is displayed under the updates tab/incorrectly represented in the installed tab - [#3788]https://github.com/NuGet/Home/issues/3788

* [Feature] Error on multiple entries for the same package ids in Directory.Packages.props - [#9467](https://github.com/NuGet/Home/issues/9467)

* Add opt-out escape hatch on Linux and macOS - [#11264](https://github.com/NuGet/Home/issues/11264)

### Issues fixed in this release

**DCRs:**

* [DCR]: Print sources in NU1507 - [#11715](https://github.com/NuGet/Home/issues/11715)

* [DCR]: Only cancel VS cred provider requests if VS is closing - [#11970](https://github.com/NuGet/Home/issues/11970)

* Remove net45 and net40 targets from the NuGet build - [#11830](https://github.com/NuGet/Home/issues/11830)

* For C++/CLI PackageReference projects, NuGet should ignore the TargetPlatformMoniker - [#11808](https://github.com/NuGet/Home/issues/11808)

* [DCR]: Include caught exceptions as inner exceptions when rethrowing (in MsBuildUtility) - [#11766](https://github.com/NuGet/Home/issues/11766)

* Specifying both -f ... and -r ... to dotnet build fails to restore if multiple frameworks are present in the project file - [#11653](https://github.com/NuGet/Home/issues/11653)

* PackageSourceMapping public constructor - [#11609](https://github.com/NuGet/Home/issues/11609)

* Add support for system and fallback certificate bundles - [#11263](https://github.com/NuGet/Home/issues/11263)

* Duplicate PackageReference includes are handled inconsistently by all restores, leading to unnecessary full restores - [#9864](https://github.com/NuGet/Home/issues/9864)

**Bugs:**

* [Bug]:  X.509 trust store isn't initialized in `dotnet add package` and SDK resolver code paths - [#11956](https://github.com/NuGet/Home/issues/11956)

* Cache DTE service in VS Solution Manager - [#11902](https://github.com/NuGet/Home/issues/11902)

* Nuget CPS references reader is forcing all vc projects to be fully loaded - [#11877](https://github.com/NuGet/Home/issues/11877)

* Make dotnet package verification env var value comparison case insensitive - [#11876](https://github.com/NuGet/Home/issues/11876)

* Using JsonTextWriter manually in LockFileFormat - [#11870](https://github.com/NuGet/Home/issues/11870)

* Extra allocations in EqualityUtility - [#11867](https://github.com/NuGet/Home/issues/11867)

* [Bug]: Boxing of structs to compute hashcode is causing excessive allocations - [#11866](https://github.com/NuGet/Home/issues/11866)

* When restore raises an NU1301, build might fail with a `project.assets.json doesn't have a target for 'net6.0-windows10.0.19041.0` like error that's a red herring - [#11862](https://github.com/NuGet/Home/issues/11862)

* [Bug]: Package source option "All" appears unsorted in the in the list when using VS in non-English languages - [#11857](https://github.com/NuGet/Home/issues/11857)

* [Bug]: Installing a package in Visual Studio when using central package management results in an error before projects can be modified - [#11828](https://github.com/NuGet/Home/issues/11828)

* [Bug]: [Bug Bash] The “Version” dropdown box is blank in “Consolidate” tab of solution-level PM UI - [#11806](https://github.com/NuGet/Home/issues/11806)

* PackageDownload multiple versions doesn't work in Visual Studio.  - [#11798](https://github.com/NuGet/Home/issues/11798)

* [Bug]: Visual Studio restore sometimes sets originalTargetFrameworks incorrectly in project.assets.json - [#11795](https://github.com/NuGet/Home/issues/11795)

* [Bug]: NuGet does not retry some HTTP timeouts - [#11779](https://github.com/NuGet/Home/issues/11779)

* [Bug]: misspelling in RestoreCommandCannotDeterminePackagesFolder_deu - [#11774](https://github.com/NuGet/Home/issues/11774)

* Update SPDX licenses to bb0099c - [#11765](https://github.com/NuGet/Home/issues/11765)

* "Illegal characters in path" (Solution Directory) - [#11764](https://github.com/NuGet/Home/issues/11764)

* NuGet Package Manager window causes persistent WPF frame rate spike due to a runaway animation - [#11746](https://github.com/NuGet/Home/issues/11746)

* [Bug]: PM UI version list only shows a single latest version - [#11734](https://github.com/NuGet/Home/issues/11734)

* Large number of allocations while processing package references - [#11733](https://github.com/NuGet/Home/issues/11733)

* Unnecessary Allocations in SemanticVersion.ParseSections() - [#11732](https://github.com/NuGet/Home/issues/11732)

* [Bug]: new warning for package source mappings doesn't pass a value for the resource string placeholder - [#11709](https://github.com/NuGet/Home/issues/11709)

* [Bug]: Central package management breaks no-op restores - [#11696](https://github.com/NuGet/Home/issues/11696)

* [Bug]: MsBuild version is not parsed correctly when -MsBuildPath option is passed to nuget.exe restore - [#11689](https://github.com/NuGet/Home/issues/11689)

* [Bug]: Very slow restore or OOM when using NoWarn - [#11669](https://github.com/NuGet/Home/issues/11669)

* [Bug]: Automatic credential plugin discovery is broken when 64 bit msbuild.exe is used by nuget.exe - [#11623](https://github.com/NuGet/Home/issues/11623)

* [Bug]:  Reduce memory allocation while detecting cycles or potential degrades in package versions during restore - [#11614](https://github.com/NuGet/Home/issues/11614)

* Avoid JTF.Run wrapped property retrieval, use async methods instead. - [#11199](https://github.com/NuGet/Home/issues/11199)

* .nupkg.metadata locked and being used by another process - [#10882](https://github.com/NuGet/Home/issues/10882)

* Unexpected error “Your project file doesn’t list ‘win’ as a “RuntimeIdentifier”” occurs when building the solution after enabling “RestoreLockedMode” - [#10590](https://github.com/NuGet/Home/issues/10590)

* NuGet.exe pack issues a warning (NU5128) when packing a project file - [#8713](https://github.com/NuGet/Home/issues/8713)

* Transitive lock files (with wildcard) result in NU1004 - [#8465](https://github.com/NuGet/Home/issues/8465)

* Enhance the experimentation infrastructure in NuGet code to support transitive dependencies - [#10758](https://github.com/NuGet/Home/issues/10758)