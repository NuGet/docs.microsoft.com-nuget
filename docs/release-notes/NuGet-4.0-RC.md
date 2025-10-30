---
title: NuGet 4.0 RC Release Notes
description: Release notes for NuGet 4.0 RC including known issues, bug fixes, added features, and DCRs.
author: JonDouglas
ms.author: jodou
ms.date: 02/03/2017
ms.topic: release-notes
ms.reviewer: ananguar
---

# NuGet 4.0 RC Release Notes

[NuGet 3.5 RTM Release Notes](../release-notes/nuget-3.5-RTM.md)

[NuGet 4.0 RC for Visual Studio 2017](https://blog.nuget.org/20161121/introducing-nuget4.0) is focused on adding support for .NET Core scenarios, addressing key customer feedback and improving performance in a variety of scenarios. This release brings several improvements like support for PackageReference, NuGet commands as MSBuild targets, background package restore, and more.

## Bug Fixes

- Behavioral changes in `dotnet pack --version-suffix foo` - [#3838](https://github.com/NuGet/Home/issues/3838)

- nuget.exe restore on vs "15" machine only fails - [#3834](https://github.com/NuGet/Home/issues/3834)

- .NETCore file new project should block the build during restore - [#3780](https://github.com/NuGet/Home/issues/3780)

- ASP.NET Core web app, migrated from VS2015 to VS "15", unable to restore. - [#3773](https://github.com/NuGet/Home/issues/3773)

- [Test Failure]Package ‘jQuery Validation’ can’t be uninstalled by PM UI - [#3755](https://github.com/NuGet/Home/issues/3755)

- When a package is installed to UWP `project.json`, parent projects should also be restored - [#3731](https://github.com/NuGet/Home/issues/3731)

- Modify the NuGet targets to log the package sources as High verbosity instead of Normal - [#3719](https://github.com/NuGet/Home/issues/3719)

- dotnet
  - dotnetcore pack3 should include XML documentation by default - [#3698](https://github.com/NuGet/Home/issues/3698)

- Batch update fails from UI when source without the package is first and All source is selected - [#3696](https://github.com/NuGet/Home/issues/3696)

- Nuget pack command does not include all files - [#3678](https://github.com/NuGet/Home/issues/3678)

- OOM issue - [#3661](https://github.com/NuGet/Home/issues/3661)

- ProjectFileDependencyGroups section of the assets file should use library names for projects - [#3611](https://github.com/NuGet/Home/issues/3611)

- "dotnet restore" and recursing directories - [#3517](https://github.com/NuGet/Home/issues/3517)

- Restore3 failures are logged as warnings instead of errors - [#3503](https://github.com/NuGet/Home/issues/3503)

- TFS issue: "[file]not be found in your workspace, or you do not have permission to access it" - [#2805](https://github.com/NuGet/Home/issues/2805)

- Typing "nuget \<packagename\>" in vs quicklaunch search box keeps "nuget " prefix - [#2719](https://github.com/NuGet/Home/issues/2719)

- System.Xml.XmlException: Unrecognized root element in Core Properties part. Line 2, position 2. - [#2718](https://github.com/NuGet/Home/issues/2718)

- `.nuspec` with escaped &lt; or &gt; in text fields no longer builds - [#2651](https://github.com/NuGet/Home/issues/2651)

- nuget.exe delete won't prompt for credentials (it's in non-interactive mode) - [#2626](https://github.com/NuGet/Home/issues/2626)

- nuget.exe delete warns about API Key for local sources, even though it makes no sense - [#2625](https://github.com/NuGet/Home/issues/2625)

- Error experience poor when installing EF -pre package - [#2566](https://github.com/NuGet/Home/issues/2566)

- Visual Studio crashed attempting after changing selection in Package Manager - [#2551](https://github.com/NuGet/Home/issues/2551)

- dotnet
  - dotnetcore restore performs case sensitive Id lookups on flat-list local repositories when floating versions are used - [#2516](https://github.com/NuGet/Home/issues/2516)

- nuget.exe delete is broken for V2 feed - [#2509](https://github.com/NuGet/Home/issues/2509)

- nuget.exe push timeout needs a better error message - [#2503](https://github.com/NuGet/Home/issues/2503)

- Tool restore without proper imports silently fails. - [#2462](https://github.com/NuGet/Home/issues/2462)

- NuGet prompts to enter credentials when there is a private feed even when installing from nuget.org - [#2346](https://github.com/NuGet/Home/issues/2346)

- ApplicationInsights 2.0 package is listed but doesn't exist yet - [#2317](https://github.com/NuGet/Home/issues/2317)

- UIDelay in VS "15" preview 5 branch - [#3500](https://github.com/NuGet/Home/issues/3500)

- First OnBuild event is missed for Restore during Build for UWP - [#3489](https://github.com/NuGet/Home/issues/3489)

- PowerShell5 breaks EntityFramework install? - [#3312](https://github.com/NuGet/Home/issues/3312)

- Add source to detailed logging (consider for 3.5) - [#3294](https://github.com/NuGet/Home/issues/3294)

- NoCache parameter not honored in nuget client version 3.4+ - [#3074](https://github.com/NuGet/Home/issues/3074)

- When a credential provider fails to load in VS, don't break NuGet - [#2422](https://github.com/NuGet/Home/issues/2422)

## Features

- Set up CI to run x86 - [#3868](https://github.com/NuGet/Home/issues/3868)

- Auto Restore 3/3: non blocking UI - [#3658](https://github.com/NuGet/Home/issues/3658)

- Auto Restore 2/3: background restore on nomination - [#3657](https://github.com/NuGet/Home/issues/3657)

- Restore project refs to match build behavior (recurse) - [#3615](https://github.com/NuGet/Home/issues/3615)

- DPL support in VS "15" - minbar - [#3614](https://github.com/NuGet/Home/issues/3614)

- Move settings file to Program Files - [#3613](https://github.com/NuGet/Home/issues/3613)

- Generated restore props and targets need cross-targeting participation support - [#3496](https://github.com/NuGet/Home/issues/3496)

- NuGet Restore support for PackageTargetFallback (f.k.a Imports) - [#3494](https://github.com/NuGet/Home/issues/3494)

- ToolsRef implementation - [#3472](https://github.com/NuGet/Home/issues/3472)

- Restore3 for a RID - [#3465](https://github.com/NuGet/Home/issues/3465)

- NuGet UI to support Add/Remove/Update of PackageRefs - [#3457](https://github.com/NuGet/Home/issues/3457)

- Auto Restore 1/3: Implemenation of Nomination API via Caching Project Restore Info - [#3456](https://github.com/NuGet/Home/issues/3456)

- [0] NuGet Restore Task & Targets - [#2994](https://github.com/NuGet/Home/issues/2994)

- [1] Enable Solution level restore in MSBuild - [#2993](https://github.com/NuGet/Home/issues/2993)

- Support credential provider public extensibility in Visual Studio - [#2909](https://github.com/NuGet/Home/issues/2909)

- Recursive nuget restore - [#2533](https://github.com/NuGet/Home/issues/2533)

- Can't load Microsoft.TeamFoundation.Client on dev15, need to update Microsoft.TeamFoundation.Client version to 15.0 for VS "15" Preview - [#2392](https://github.com/NuGet/Home/issues/2392)

- Unable to install C++ package to C++ UWP project in VS "15" Preview - [#2369](https://github.com/NuGet/Home/issues/2369)

- Nupkg needs to support \buildCrossTargeting\ folder - and import `.targets` / `.props` for "crosstargeting" MSBuild scope. - [#3499](https://github.com/NuGet/Home/issues/3499)

- ToolsReference Design - [#3462](https://github.com/NuGet/Home/issues/3462)

- Fix NuGet UI to support restore w/ PackageReferences in `.csproj` - [#3455](https://github.com/NuGet/Home/issues/3455)

- Adding clear cache button to VS package manager settings - [#3289](https://github.com/NuGet/Home/issues/3289)

## DCRs

- Solution Restore should be blocked while auto restore is happening. - [#3797](https://github.com/NuGet/Home/issues/3797)

- NetCore install from NuGet Package Manager UI installs to every TFM , instead of ones that the package supports - [#3721](https://github.com/NuGet/Home/issues/3721)

- Restore nominator API needs to support DotNetCliToolsReferences too. - [#3702](https://github.com/NuGet/Home/issues/3702)

- Mark our VS "15" vsix as a systemcomponent - [#3700](https://github.com/NuGet/Home/issues/3700)

- Migrate from referencing MS.VS.Services.Client to MS.VS.Services.Client.Interactive - [#3670](https://github.com/NuGet/Home/issues/3670)

- $(RestoreLegacyPackagesDirectory) should be respected at a project level by restore - [#3618](https://github.com/NuGet/Home/issues/3618)

- Restore to project with single TargetFramework must not condition props - [#3588](https://github.com/NuGet/Home/issues/3588)

- dotnet
  - dotnetcore restore3 foo.csproj should follow projectref dependencies, and restore those too. Like build. - [#3577](https://github.com/NuGet/Home/issues/3577)

- "type": "platform" Dependencies represented as "type":"package" in lock file - [#2695](https://github.com/NuGet/Home/issues/2695)

- nuget.exe Verbose mode should show the download url - [#2629](https://github.com/NuGet/Home/issues/2629)

- Move NuGet xplat to Microsoft.NetCore.App and netcoreapp1.0 - [#2483](https://github.com/NuGet/Home/issues/2483)

- Push - It should be possible to override the symbol server when pushing from the command line - [#2348](https://github.com/NuGet/Home/issues/2348)

- Consolidate code for finding the global packages path - [#2296](https://github.com/NuGet/Home/issues/2296)

- Need a better name than suppressParent - [#2196](https://github.com/NuGet/Home/issues/2196)

- Determine `project.json` dependency name to use for MSBuild projects - [#1914](https://github.com/NuGet/Home/issues/1914)

- Add SemVer 2.0.0 support to NuGet.Core - [#3383](https://github.com/NuGet/Home/issues/3383)

- Allow transitive dependency NuPkgs with `.targets` to be available in MSBuild - [#3342](https://github.com/NuGet/Home/issues/3342)

- NuGet restore from the commandline is significantly slower than VS - [#3330](https://github.com/NuGet/Home/issues/3330)

- Make package ID and version comparison case insensitive - [#2522](https://github.com/NuGet/Home/issues/2522)

- NoCache option does not work for `packages.config` based restore/install (GlobalPackagesFolder) - [#1406](https://github.com/NuGet/Home/issues/1406)

- FindPackageByIdResource resources needs a default cache context and logger - [#1357](https://github.com/NuGet/Home/issues/1357)
