---
title: NuGet 4.7 RTM Release Notes
description: Release notes for NuGet 4.7.0 including known issues, bug fixes, added features, and DCRs.
author: karann-msft
ms.author: karann
manager: unnir
ms.date: 5/14/2018
ms.topic: conceptual
---

# NuGet 4.7 RTM Release Notes

[Visual Studio 2017 15.7 RTW](https://www.visualstudio.com/news/releasenotes/vs2017-relnotes) comes with [NuGet 4.7.0](https://dist.nuget.org/win-x86-commandline/v4.7.0/nuget.exe).

## Summary: What's New in this Release
* We have added support for [signing packages](https://docs.microsoft.com/en-us/nuget/create-packages/sign-a-package).  
* Visual Studio 2017 and nuget.exe now verifies package integrity before installing, restoring packages for [signed packages](https://docs.microsoft.com/en-us/nuget/reference/signed-packages-reference).
* We have improved performance of successive restores.

## Known issues
### Issues with .NET Standard 2.0 with .NET Framework & NuGet 

.NET Standard & its tooling was designed such that projects targeting .NET Framework 4.6.1 can consume NuGet packages & projects targeting .NET Standard 2.0 or earlier. [This document](https://github.com/dotnet/standard/issues/481) summarizes the issues around that scenario, the plan for addressing them, and workarounds you can deploy with today's state of the tooling.

## Top issues fixed in this release

**Performance fixes**
* Don't write asset files when there is no change - [#6491](https://github.com/NuGet/Home/issues/6491)
* Restore causes extra MSBuild evaluations when child projects' TFM do not match with the parent project's - [#6311](https://github.com/NuGet/Home/issues/6311)
* Improve NoOp restore perf by optimizing dependency graph spec creation - [#6252](https://github.com/NuGet/Home/issues/6252)

**Bugs**
* Migrator dialog title shows a {0} - [#6832](https://github.com/NuGet/Home/issues/6832)
* [Accessibility] Package compatibility issues in migrator are not narrated - [#6831](https://github.com/NuGet/Home/issues/6831)
* [Accessibility] Help icon in PR migrator do not scale well at high resolution and high DPI - [#6830](https://github.com/NuGet/Home/issues/6830)
* Enforce RepositorySignatureInformation on Repository countersignatures - [#6807](https://github.com/NuGet/Home/issues/6807)
* EF package broken with devDependency flag explicitly setting exclude=Runtime for PackageReference - [#6753](https://github.com/NuGet/Home/issues/6753)
* NuGet runs into dead lock in .Net Core project system (new regression). - [#6733](https://github.com/NuGet/Home/issues/6733)
* PackagePath is constructed incorrectly if TfmSpecificPackageFile is used with globbing paths - [#6726](https://github.com/NuGet/Home/issues/6726)
* Remove gold bar for packages.config to packageref migrator until we determine the correct promotion strategy - [#6707](https://github.com/NuGet/Home/issues/6707)
* Migrator dogfooding issues - [#6666](https://github.com/NuGet/Home/issues/6666)
* VS UI and PMC take 30min to see new package (nuget.exe sees it right away) - [#6657](https://github.com/NuGet/Home/issues/6657)
* Signing:  verify command usage examples incorrect - [#6637](https://github.com/NuGet/Home/issues/6637)
* Signing:  incorrect encoding used - [#6600](https://github.com/NuGet/Home/issues/6600)
* Signing:  small changes to NativeCms - [#6567](https://github.com/NuGet/Home/issues/6567)
* Signing:  SignatureUtility.GetCertificateChain(...) does not check all chain statuses - [#6565](https://github.com/NuGet/Home/issues/6565)
* Signing:  improve DER GeneralizedTime handling - [#6564](https://github.com/NuGet/Home/issues/6564)
* lockFile.GetLibrary is case sensitive - [#6500](https://github.com/NuGet/Home/issues/6500)
* end to end tests are failing post the ngen change - [#6443](https://github.com/NuGet/Home/issues/6443)
* VS does not show a NU3002 error when installing a tampered package - [#6337](https://github.com/NuGet/Home/issues/6337)
* web api project cannot create package unless ispackable is explicitly set. - [#6156](https://github.com/NuGet/Home/issues/6156)
* Perf: Improve NuGet package 'restore' times - [#4897](https://github.com/NuGet/Home/issues/4897)
* nuget.exe pack calls dotnet.exe - [#4831](https://github.com/NuGet/Home/issues/4831)
* Installing packages takes a very long time in Package Manager Console - [#3480](https://github.com/NuGet/Home/issues/3480)
* Install/update restore code and Restore code paths are not consistent - [#3471](https://github.com/NuGet/Home/issues/3471)
* Solution PackageManager Version ComboBox can select separator via keyboard - [#2606](https://github.com/NuGet/Home/issues/2606)
* Unable to load the service index for source https://www.myget.org/F/<id> ---> System.Net.Http.HttpRequestException: Response status code does not indicate success: 403 (Forbidden) - [#2530](https://github.com/NuGet/Home/issues/2530)
* dotnet restore puts packages in an unexpected location - [#2488](https://github.com/NuGet/Home/issues/2488)
* Restore from a directory with inaccessible subdirectories fails - [#2485](https://github.com/NuGet/Home/issues/2485)
* Convert_WithValidImageUrl_DownloadsImage fails on CI - [#2474](https://github.com/NuGet/Home/issues/2474)
* Re-enable NuGet.Synchronization.Test tests - [#2410](https://github.com/NuGet/Home/issues/2410)
* Package restore in VS reports "Failed to resolve all project references"..., builds sometimes fail - [#1860](https://github.com/NuGet/Home/issues/1860)
* nuget push prompts for credentials, but does not push once provided - [#997](https://github.com/NuGet/Home/issues/997)
* nuget update does not delete old versions of packages from packages directory - [#996](https://github.com/NuGet/Home/issues/996)
* NuGet.CommandLine - packing a project file uses first .nuspec file in the project directory when multiple nuspecs exist. - [#988](https://github.com/NuGet/Home/issues/988)
* Wrong behavior related to Show preview window and Do not show this again checkboxes - [#821](https://github.com/NuGet/Home/issues/821)
* PackagePathResolver.GetInstalledPath(PackageIdentity) could be more efficient - [#803](https://github.com/NuGet/Home/issues/803)
* Install-Package fails for ps1 package due to permission? - [#640](https://github.com/NuGet/Home/issues/640)

**Feature:**
* Integrate Verification API with Zhi's PR on RepoSign - [#6700](https://github.com/NuGet/Home/issues/6700)
* add the complete validation set in pc -> pr migrator - [#6632](https://github.com/NuGet/Home/issues/6632)
* Signing: Add support for repository sign - [#6619](https://github.com/NuGet/Home/issues/6619)
* Validate with RepoSign (IDE) For all v3 servers that are reposigned - [#6606](https://github.com/NuGet/Home/issues/6606)
* NuGet CLI appends service endpoint to a private server url - [#6586](https://github.com/NuGet/Home/issues/6586)
* Allow project with DotnetToolReference=RestoreProjectStyle to add additional PackageReference when marked with metadata - [#6559](https://github.com/NuGet/Home/issues/6559)
* Signing:  get signature value of repository countersignature - [#6551](https://github.com/NuGet/Home/issues/6551)
* Repo sign implementation - [#6546](https://github.com/NuGet/Home/issues/6546)
* Course of action for packages that raise warnings, fail the build in the migrator - [#6532](https://github.com/NuGet/Home/issues/6532)
* Improve the PC->PR Migrator UI - [#6530](https://github.com/NuGet/Home/issues/6530)
* Need a way to figure out the sha for a given nuget library - [#6492](https://github.com/NuGet/Home/issues/6492)
* Signing:  add utility methods for creating and verifying repository signatures/countersignatures - [#6473](https://github.com/NuGet/Home/issues/6473)
* V3 protocol change for package signing follow up - [#6425](https://github.com/NuGet/Home/issues/6425)
* [SPEC] Nuget verify repo signature - [#6424](https://github.com/NuGet/Home/issues/6424)
* [SPEC] Nuget.mssign.exe Repo signature - [#6422](https://github.com/NuGet/Home/issues/6422)
* Repository Signatures - [#6378](https://github.com/NuGet/Home/issues/6378)
* Support for verifying repository signatures - [#6276](https://github.com/NuGet/Home/issues/6276)
* Expose a way to wait on running restore operation running in Visual Studio via IVs apis. - [#6029](https://github.com/NuGet/Home/issues/6029)
* Provide migration tool to move from packages.config project to packagereference - [#5488](https://github.com/NuGet/Home/issues/5488)
* Emit X-NuGet-Session-Id header to correlate across requests [feature proposal] - [#5330](https://github.com/NuGet/Home/issues/5330)
* Support Extension SDK's as NuGet packages - [#3304](https://github.com/NuGet/Home/issues/3304)
* [Feature] Package signing - [#2577](https://github.com/NuGet/Home/issues/2577)
* Install nuget.exe with an install of visual studio - [#2473](https://github.com/NuGet/Home/issues/2473)
* Feature request: validate TFMs - [#969](https://github.com/NuGet/Home/issues/969)

**DCR:**
* Signing:  enable removal of repository signature/countersignature - [#6646](https://github.com/NuGet/Home/issues/6646)
* Signing:  API for stripping repository signature/countersignature - [#6589](https://github.com/NuGet/Home/issues/6589)
* Log source information in VS - [#6527](https://github.com/NuGet/Home/issues/6527)
* Filter /tools on only TFM and RID, so the settings XML can be put in /tools folder - [#6197](https://github.com/NuGet/Home/issues/6197)
* Warn when Pack command excludes a file that starts with .  - [#3308](https://github.com/NuGet/Home/issues/3308)
* Add "Highest Installed" dependency resolution option - [#2144](https://github.com/NuGet/Home/issues/2144)
* Enhancement: NuGet spec command should have parameter to override automatic import of project files - [#967](https://github.com/NuGet/Home/issues/967)
* Nupkg compression is bad - [#890](https://github.com/NuGet/Home/issues/890)
* Inconsistent authoring model for inside package vs runtime.json - [#850](https://github.com/NuGet/Home/issues/850)
* Compat check / Runtime.json does not consider version - [#849](https://github.com/NuGet/Home/issues/849)
* Re-examine Framework Assembly References - [#794](https://github.com/NuGet/Home/issues/794)

**Spec:**
* Improve VS error experience for signed packages - [#6427](https://github.com/NuGet/Home/issues/6427)

**Docs:**
* Doc Request: Project.lock.json - [#3364](https://github.com/NuGet/Home/issues/3364)


[List of all issues fixed in this release](https://github.com/NuGet/Home/issues?q=is%3Aissue+is%3Aclosed+milestone%3A%224.7")
