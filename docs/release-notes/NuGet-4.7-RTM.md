---
title: NuGet 4.7 RTM Release Notes
description: Release notes for NuGet 4.7.0 including known issues, bug fixes, added features, and DCRs.
author: JonDouglas
ms.author: jodou
ms.date: 5/14/2018
ms.topic: release-notes
---

# NuGet 4.7 Release Notes

[Visual Studio 2017 15.7 RTW](https://www.visualstudio.com/news/releasenotes/vs2017-relnotes) comes with [NuGet 4.7.0](https://dist.nuget.org/win-x86-commandline/v4.7.0/nuget.exe).

## Summary: What's New in 4.7.0

* We have augmented package signing to enable [Repository Signed packages](https://github.com/NuGet/Home/wiki/Repository-Signatures)

* With Visual Studio Version 15.7, we have introduced the capability to [migrate existing projects that use the packages.config format to use PackageReference](../consume-packages/migrate-packages-config-to-package-reference.md) instead.

## Summary: What's New in 4.7.2

* Security Fix: Permissions on files created inside ~/.nuget are too open [#7673](https://github.com/NuGet/Home/issues/7673) [CVE-2019-0757](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0757)

## Summary: What's New in 4.7.3

* Security Fix: Files inside of NUPKGs can have a relative path above the NUPKG directory [#7906](https://github.com/NuGet/Home/issues/7906)

## Known issues

### The `Migrate packages.config to PackageReference...` option is not available in the right-click context menu

#### Issue

When a project is first opened, NuGet may not have initialized until a NuGet operation is performed. This causes the migration option to not show up in the right-click context menu on `packages.config` or `References`.

#### Workaround

Perform any one of the following NuGet actions:
* Open the Package Manager UI - Right-click on `References` and select `Manage NuGet Packages...`
* Open the Package Manager Console - From `Tools > NuGet Package Manager`, select `Package Manager Console`
* Run NuGet restore - Right-click on the solution node in the Solution Explorer and select `Restore NuGet Packages`
* Build the project which also triggers NuGet restore

You should now be able to see the migration option. Note that this option is not supported and will not show up for ASP.NET and C++ project types.

### Issues with .NET Standard 2.0 with .NET Framework & NuGet

.NET Standard & its tooling was designed such that projects targeting .NET Framework 4.6.1 can consume NuGet packages & projects targeting .NET Standard 2.0 or earlier. [This document](https://github.com/dotnet/standard/issues/481) summarizes the issues around that scenario, the plan for addressing them, and workarounds you can deploy with today's state of the tooling.

## Top issues fixed in this release

### Bugs

* NuGet runs into a deadlock in .Net Core project system (new regression). - [#6733](https://github.com/NuGet/Home/issues/6733)
* Pack: PackagePath is constructed incorrectly if TfmSpecificPackageFile is used with globbing paths - [#6726](https://github.com/NuGet/Home/issues/6726)
* Pack: web api project cannot create package unless ispackable is explicitly set. - [#6156](https://github.com/NuGet/Home/issues/6156)
* VS UI and PMC take 30min to see new package (nuget.exe sees it right away) - [#6657](https://github.com/NuGet/Home/issues/6657)
* Signing:  SignatureUtility.GetCertificateChain(...) does not check all chain statuses - [#6565](https://github.com/NuGet/Home/issues/6565)
* Signing:  improve DER GeneralizedTime handling - [#6564](https://github.com/NuGet/Home/issues/6564)
* Signing: VS does not show a NU3002 error when installing a tampered package - [#6337](https://github.com/NuGet/Home/issues/6337)
* lockFile.GetLibrary is case sensitive - [#6500](https://github.com/NuGet/Home/issues/6500)
* Install/update restore code and Restore code paths are not consistent - [#3471](https://github.com/NuGet/Home/issues/3471)
* Solution PackageManager Version ComboBox can select separator via keyboard - [#2606](https://github.com/NuGet/Home/issues/2606)
* Unable to load the service index for source `https://www.myget.org/F/<id>` ---> System.Net.Http.HttpRequestException: Response status code does not indicate success: 403 (Forbidden) - [#2530](https://github.com/NuGet/Home/issues/2530)

### DCRs

* Emit X-NuGet-Session-Id header to correlate across requests [feature proposal] - [#5330](https://github.com/NuGet/Home/issues/5330)
* Expose a way to wait on running restore operation running in Visual Studio via IVs apis. - [#6029](https://github.com/NuGet/Home/issues/6029)
* NuGet.exe -NoServiceEndpoint will avoid appending service url suffix - [#6586](https://github.com/NuGet/Home/issues/6586)
* add commit hash to informational version - [#6492](https://github.com/NuGet/Home/issues/6492)
* Signing:  enable removal of repository signature/countersignature - [#6646](https://github.com/NuGet/Home/issues/6646)
* Signing:  API for stripping repository signature/countersignature - [#6589](https://github.com/NuGet/Home/issues/6589)
* Log source information in VS - [#6527](https://github.com/NuGet/Home/issues/6527)
* Filter /tools on only TFM and RID, so the settings XML can be put in /tools folder - [#6197](https://github.com/NuGet/Home/issues/6197)
* Warn when Pack command excludes a file that starts with .  - [#3308](https://github.com/NuGet/Home/issues/3308)

[List of all issues fixed in this release](https://github.com/NuGet/Home/issues?q=is%3Aissue+is%3Aclosed+milestone%3A%224.7")
