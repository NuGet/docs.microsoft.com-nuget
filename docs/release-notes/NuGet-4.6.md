
---
title: NuGet 4.5 RTM Release Notes | Microsoft Docs
author: anangaur
ms.author: anangaur
manager: unnir
ms.date: 3/7/2018
ms.topic: article
ms.prod: nuget
ms.technology: null
description: Release notes for NuGet 4.6.0 including known issues, bug fixes, added features, and DCRs.
keywords: NuGet 4.6.0 release notes, bug fixes, known issues, added features, DCRs
ms.reviewer:
- karann-msft
- unniravindranathan
- anangaur
---

# NuGet 4.6 RTM Release Notes

[Visual Studio 2017 15.6 RTW](https://www.visualstudio.com/news/releasenotes/vs2017-relnotes) comes with [NuGet 4.6.0](https://dist.nuget.org/win-x86-commandline/v4.6.0/nuget.exe).

## Summary: What's New in this Release
* We have added support for [signing packages](https://docs.microsoft.com/en-us/nuget/create-packages/sign-a-package).  
* Visual Studio 2017 and nuget.exe now verifies package integrity before installing, restoring packages for [signed packages](https://docs.microsoft.com/en-us/nuget/reference/signed-packages-reference).
* We have improved performance of successive restores.
* 

## Known issues
### Issues with .NET Standard 2.0 with .NET Framework & NuGet 

.NET Standard & its tooling was designed such that projects targeting .NET Framework 4.6.1 can consume NuGet packages & projects targeting .NET Standard 2.0 or earlier. [This document](https://github.com/dotnet/standard/issues/481) summarizes the issues around that scenario, the plan for addressing them, and workarounds you can deploy with today's state of the tooling.

### Retargeting target framework version may lead to incomplete Intellisense
#### Issue
Retargeting target framework version may lead to incomplete Intellisense, in Visual Studio. This happens when you are using PackageReferences as the package manager format. [NuGet#4216](https://github.com/NuGet/Home/issues/4216)
#### Workaround
Do a manual restore.

### A package in a .NET Core project that contains an assembly with an invalid signature, can trigger an infinite restore loop

#### Issue

Occasionally, when you use a package that contains an assembly with an invalid signature or when the package version is set with 'DateTime' ticker, it causes the package auto-restore to run in an infinite loop [dotnet/project-system#1457](https://github.com/dotnet/project-system/issues/1457).

#### Workaround

There is no workaround at this time.

## Issues fixed in NuGet 4.5 RTM timeframe

For issues fixed in NuGet 4.4 RTM, please refer to [NuGet 4.4 RTM Release Notes](../release-notes/nuget-4.4-RTM.md) 
#4.6 Release Notes

[Issues List](https://github.com/NuGet/Home/issues?q=is%3Aissue+is%3Aclosed+milestone%3A%224.6")

**Bug:**

* Signing:  sign command fails to find untrusted self-issued certificate in certificate store - [#6616](https://github.com/NuGet/Home/issues/6616)

* Signing:  random timestamping failures - [#6587](https://github.com/NuGet/Home/issues/6587)

* Test:  fix root cause of test flakiness - [#6552](https://github.com/NuGet/Home/issues/6552)

* Signing:  incorrect GeneralizedTime handling - [#6549](https://github.com/NuGet/Home/issues/6549)

* Verification fails for signature with different algorithm than timestamp - [#6539](https://github.com/NuGet/Home/issues/6539)

* Assetsfile comparison issues lead to needless rewriting of the assets => triggering more design time builds - [#6491](https://github.com/NuGet/Home/issues/6491)

* VS2017 publish: project.assets.json doesn't have a target for .NETCoreApp,Version=v1.1 - [#6482](https://github.com/NuGet/Home/issues/6482)

* Signing:  X509ChainElement.Certificate not disposed immediately after use - [#6464](https://github.com/NuGet/Home/issues/6464)

* Timestamping with Safe Creative TSA errors with null reference exception - [#6463](https://github.com/NuGet/Home/issues/6463)

* [Test Failure][Signing] The error doesn’t make sense when timestamp signing certificate does not satisfy certificate requirements - [#6451](https://github.com/NuGet/Home/issues/6451)

* Signing:  guarantee that GetPrimarySignatureCertificates(...)/GetPrimarySignatureTimestampSignatureCertificates(...) returns a non-empty list or throws - [#6447](https://github.com/NuGet/Home/issues/6447)

* Cannot find 'vswhere' via https://api.nuget.org/v3/index.json/FindPackagesById()?id='vswhere' - [#6332](https://github.com/NuGet/Home/issues/6332)

* Push to local folder leaves nupkg locked - [#6325](https://github.com/NuGet/Home/issues/6325)

* Ensure NuGetv3LocalRepository is updated after package installs - [#6316](https://github.com/NuGet/Home/issues/6316)

* Restore evaluates child projects using the parent project target framework - [#6311](https://github.com/NuGet/Home/issues/6311)

* Signing:  verify command's CertificateFingerprint not enforced - [#6304](https://github.com/NuGet/Home/issues/6304)

* contentFiles any doesn't work with PackageReference-style projects - [#6287](https://github.com/NuGet/Home/issues/6287)

* Package signatures can be invalid for certain packages packed with a different ZIP spec - [#6272](https://github.com/NuGet/Home/issues/6272)

* mssign command doesn't produce same logging as sign command - [#6263](https://github.com/NuGet/Home/issues/6263)

* PackageIdentity.ToString throws NullReferenceException if version is null - [#6235](https://github.com/NuGet/Home/issues/6235)

* Downloading Nuget.exe from dist.nuget.org blocked - [#6191](https://github.com/NuGet/Home/issues/6191)

* disabledPackageSources seems not working - [#6155](https://github.com/NuGet/Home/issues/6155)

* Documentation URL forwards user to incorrect location in docs. (from NuGet settings window on VS) - [#6100](https://github.com/NuGet/Home/issues/6100)

* Restore is skipped from VS when two projects are in the same folder - [#6038](https://github.com/NuGet/Home/issues/6038)

* Do not reformat web.config when running NuGet Update-Package command - [#6018](https://github.com/NuGet/Home/issues/6018)

* NuGet.exe replaces '+' with '%2B' in assembly name - [#5956](https://github.com/NuGet/Home/issues/5956)

* VS NuGet writes absolute paths into project files under specific circumstances - [#5888](https://github.com/NuGet/Home/issues/5888)

* dotnet restore with multiple sources crashes - [#5817](https://github.com/NuGet/Home/issues/5817)

* Update and Uninstall not working for projects with multiple TargetFrameworks - [#5472](https://github.com/NuGet/Home/issues/5472)

* Visual Studio not using packageCredentials from NuGetDefaults.config - [#4953](https://github.com/NuGet/Home/issues/4953)

* Pack should re-evaluate project versions to allow git versioning - [#4790](https://github.com/NuGet/Home/issues/4790)

* Update-Package - Error - [#4690](https://github.com/NuGet/Home/issues/4690)

* dotnet nuget push times out after 5 minutes when a symbol package is present - [#4672](https://github.com/NuGet/Home/issues/4672)

* dotnet restore with basic authentication failing - [#4668](https://github.com/NuGet/Home/issues/4668)

* Package Search is nonsensical  - [#4614](https://github.com/NuGet/Home/issues/4614)

* Still gets lots of hard to understand errors when you install an incompatible package - [#4555](https://github.com/NuGet/Home/issues/4555)

* TemplateWizard and PackageReferences - [#4549](https://github.com/NuGet/Home/issues/4549)

* Package-delivered props files ignored when MSBuild.exe is run from outside a Developer Command Prompt - [#4530](https://github.com/NuGet/Home/issues/4530)

* [Watson] dev11nonfatalerror: DEV11NONFATAL_CLR_EXCEPTION_System.NullReferenceException_80004003_NuGet.PackageManagement.VisualStudio.dll!Unknown - [#4485](https://github.com/NuGet/Home/issues/4485)

* Poor error message when referencing .NET Standard Library that is not applicable to project - [#4423](https://github.com/NuGet/Home/issues/4423)

* dotnet add package fails for package targeting portable profile with little guidance - [#4349](https://github.com/NuGet/Home/issues/4349)

* dotnet pack - version suffix missing from ProjectReference - [#4337](https://github.com/NuGet/Home/issues/4337)

* Build errors and VS crash with .NET Core template - [#3973](https://github.com/NuGet/Home/issues/3973)

* Restore logs are lost and not persisted in the output window - [#3884](https://github.com/NuGet/Home/issues/3884)

* Remove dependency on ImportsAfter for CrossTargeting - [#3860](https://github.com/NuGet/Home/issues/3860)

* `dotnet restore` fails with private feed configured in local system account - [#3839](https://github.com/NuGet/Home/issues/3839)

* NuGet operations can hang [Customer report] - [#3823](https://github.com/NuGet/Home/issues/3823)

* For UWP Project, improve restore error message - [#3717](https://github.com/NuGet/Home/issues/3717)

* dotnet pack3 always adds exclude="Build,Analyzers" to .nuspec - [#3697](https://github.com/NuGet/Home/issues/3697)

* Some packages can't be installed on multiple projects within a solution - [#3695](https://github.com/NuGet/Home/issues/3695)

* Installing NuGet packages doesn't work when the Package source is "All" - [#3694](https://github.com/NuGet/Home/issues/3694)

* VS Package Management Console won't work on Windows Server 2016 (missing Powershell-V2 - [#3682](https://github.com/NuGet/Home/issues/3682)

* Unable to load the service index for source https:* - [#3681](https://github.com/NuGet/Home/issues/3681)

* nuget.exe list -allversions doesn't work - [#3441](https://github.com/NuGet/Home/issues/3441)

* Nuspec file src attribute is platform specific? - [#2989](https://github.com/NuGet/Home/issues/2989)

* Misleading dependency resolution error message - [#2984](https://github.com/NuGet/Home/issues/2984)

* NuGet Package Manager GUI Install Button - [#2979](https://github.com/NuGet/Home/issues/2979)

* nuget pack fails when MSBuild version resolved to MSBuild 3.5 - [#2949](https://github.com/NuGet/Home/issues/2949)

* Establish consistent detail in issues/pr/commits - [#2948](https://github.com/NuGet/Home/issues/2948)

* NuGet failed to build due to hardcoded ILMerge path - [#2946](https://github.com/NuGet/Home/issues/2946)

* Cannot complete dotnet restore because some requests timed out after 100000ms - [#2945](https://github.com/NuGet/Home/issues/2945)

* nuget.exe restore doesn't produce .props and .targets files for .msbuildproj (regression in v3.3.0-3.4.4 upgrade) - [#2921](https://github.com/NuGet/Home/issues/2921)

* UI delay when updating a NuGet package with XAML file open - [#2878](https://github.com/NuGet/Home/issues/2878)

* nuget solution update will only update projects under sln dir - [#2875](https://github.com/NuGet/Home/issues/2875)

* NuGet Install Fails with Error: Failed to add reference to 'System.Collections' - [#2840](https://github.com/NuGet/Home/issues/2840)

* Can not push symbols nuget package - [#2837](https://github.com/NuGet/Home/issues/2837)

* WebSite project from IIS fails with illegal characters in path - [#2798](https://github.com/NuGet/Home/issues/2798)

* More helpful error when installing a package targeting the wrong framework via VS UI  - [#2789](https://github.com/NuGet/Home/issues/2789)

* Unable to restore packages in web SITE project using NuGet 3 restore packages with whole solution (commandline) - [#2783](https://github.com/NuGet/Home/issues/2783)

* VS Package Manager: Uninstall of package uses the wrong line breaks when editing project.json - [#2777](https://github.com/NuGet/Home/issues/2777)

* Package Manager UI ScrollBar scrolls to the top if the down arrow key is kept pressed to scroll down the list of packages - [#2756](https://github.com/NuGet/Home/issues/2756)

* Dev15: Got "The extension is not installable on any currently installed products" during installing preview vsix 3.4.2 - [#2737](https://github.com/NuGet/Home/issues/2737)

* Nuget add hangs on CentOS - [#2708](https://github.com/NuGet/Home/issues/2708)

* Restore with packagesavemode -nupkg fails for json.net - [#2706](https://github.com/NuGet/Home/issues/2706)

* Package Manager filter not available in vs output window for restore command - [#2704](https://github.com/NuGet/Home/issues/2704)

* NuGet package manager does not respect VS language settings - [#2689](https://github.com/NuGet/Home/issues/2689)

* Nuget.exe fails when it is denied access to %APPDATA%\Nuget\NuGet.config - [#2676](https://github.com/NuGet/Home/issues/2676)

* nuget setapikey <apikey> outputs the secure apikey - [#2659](https://github.com/NuGet/Home/issues/2659)

* Making pre-release packages using the new -Suffix argument - [#2652](https://github.com/NuGet/Home/issues/2652)

* "nuget.exe delete" doesn't delete a local package folder after the last version is deleted - [#2624](https://github.com/NuGet/Home/issues/2624)

* VS2015 listing give package parameter error - [#2608](https://github.com/NuGet/Home/issues/2608)

* nuget.exe sources update outputs success when no source parameter provided - [#2513](https://github.com/NuGet/Home/issues/2513)

* nuget.exe install should no-op install when using relative output directory - [#2511](https://github.com/NuGet/Home/issues/2511)

* "nuget sources remove -name sourceName" leaves behind credentials in local config - [#2501](https://github.com/NuGet/Home/issues/2501)

* nuget feed returning an unlisted package for NServiceBus - [#2494](https://github.com/NuGet/Home/issues/2494)

* NuGet install installing delisted package if it is highest version - [#2384](https://github.com/NuGet/Home/issues/2384)

* Duplicate XmlUtility in Packaging, Configuration, ProjectManagement, need to merge them into one utility - [#2318](https://github.com/NuGet/Home/issues/2318)

* dotnet restore failed when "Project.json" file name is captialized - [#2306](https://github.com/NuGet/Home/issues/2306)

* Potential blocking thread calls in PackageManagerControl - [#2252](https://github.com/NuGet/Home/issues/2252)

* RDP from a laptop which is 1600 resolution to machine at 150%, Package Manager hangs - [#2251](https://github.com/NuGet/Home/issues/2251)

* build.ps1 almost always fails with System Error 2 or more - [#2248](https://github.com/NuGet/Home/issues/2248)

* Guard rails does not include all file types - [#2247](https://github.com/NuGet/Home/issues/2247)

* Guard rails should include framework assemblies as runtime assets - [#2246](https://github.com/NuGet/Home/issues/2246)

* update command doesn't remove old <error> element from csproj - [#2234](https://github.com/NuGet/Home/issues/2234)

* When you have no enabled sources Browse tab should be always clear. - [#2227](https://github.com/NuGet/Home/issues/2227)

* When uninstalling NuGet without selecting "Remove dependencies" no errors will show up in error window - [#2219](https://github.com/NuGet/Home/issues/2219)

* Nuget 3.4.0.675 PackageSaveMode doesn’t save nuspec when specified - [#2218](https://github.com/NuGet/Home/issues/2218)

* UI Needs "Upgrade NuGet" button when client version does not meet requirements - [#2212](https://github.com/NuGet/Home/issues/2212)

* Strange error description when updating package - [#1996](https://github.com/NuGet/Home/issues/1996)

* VS 2015 Consolidate feature at solution level UX - [#1987](https://github.com/NuGet/Home/issues/1987)

* version list is not populated when the installed package version not available from source  - [#1974](https://github.com/NuGet/Home/issues/1974)

* "Nuget pack" does not find ".nuget\nuget.config" - [#1966](https://github.com/NuGet/Home/issues/1966)

* GetInstalledPackages() doesn't support C++ - [#1965](https://github.com/NuGet/Home/issues/1965)

* Can't update or install package: An item with the same key has already been added - [#1943](https://github.com/NuGet/Home/issues/1943)

* Fail to add xunit.runner.msbuild nuget package if project.json is used - [#1936](https://github.com/NuGet/Home/issues/1936)

* Update command yields null projectFramework. - [#1874](https://github.com/NuGet/Home/issues/1874)

* Downloaded NuGet Packages are missing the DLL's - [#1842](https://github.com/NuGet/Home/issues/1842)

* Update-Package command removes dependency details - [#1832](https://github.com/NuGet/Home/issues/1832)

* Support space in paths - [#1818](https://github.com/NuGet/Home/issues/1818)

* Leading 0 in Package Revision Number Fails to Restore in VSTS - [#1800](https://github.com/NuGet/Home/issues/1800)

* AssemblyInformationalVersion not taken into account - [#1794](https://github.com/NuGet/Home/issues/1794)

* Add diagnostics to nuget restore failures when msbuild fails to resolve P2P - [#1786](https://github.com/NuGet/Home/issues/1786)

* -solutiondirectory flag has no effect in nuget restore command - [#1781](https://github.com/NuGet/Home/issues/1781)

* nuget.exe updating a single package produces messages saying that multiple packages are being resolved - [#1776](https://github.com/NuGet/Home/issues/1776)

* Installed packages are missing information based on source selected - [#1762](https://github.com/NuGet/Home/issues/1762)

* -Tool packaging rules not propagated to referenced packages - [#1753](https://github.com/NuGet/Home/issues/1753)

* nuget.exe add and init commands ignore invalid packages on the destination. Should it overwrite? - [#1738](https://github.com/NuGet/Home/issues/1738)

* NuGet should format whitespace in project.json per VS behavior - [#1731](https://github.com/NuGet/Home/issues/1731)

* C#: creating Windows Universal project shows 'Progress in this operation is taking a long time...' message - [#1718](https://github.com/NuGet/Home/issues/1718)

* Tools-only package can not be installed - [#1710](https://github.com/NuGet/Home/issues/1710)

* No warning is logged when the directory with the packages.config contains more than 1 project file - [#1706](https://github.com/NuGet/Home/issues/1706)

* When NuGet UI re-opens Visual Studio, it opens incorrect solution - [#1702](https://github.com/NuGet/Home/issues/1702)

* UI: we should use the same text for the install/update button - [#1683](https://github.com/NuGet/Home/issues/1683)

* After powershell command is finished in PMC, the UI won't refresh to reflect the changes. - [#1673](https://github.com/NuGet/Home/issues/1673)

* On a UAP project, installing by typing wrong casing for package id fails from a folder based package source - [#1672](https://github.com/NuGet/Home/issues/1672)

* Update not working correctly for NuGet.exe (2.8.6) with -Safe option - [#1651](https://github.com/NuGet/Home/issues/1651)

* nuget.exe pack of folder with [Content_Types].xml folder in it crashes - [#1641](https://github.com/NuGet/Home/issues/1641)

* Running nuget.exe update -Verbosity Quiet does not display user prompts but expects an answer anyway - [#1639](https://github.com/NuGet/Home/issues/1639)

* Command line restore spits a wrong message about enabling restore in visual studio - [#1619](https://github.com/NuGet/Home/issues/1619)

* Investigate 3 functional test failures for project K - [#1616](https://github.com/NuGet/Home/issues/1616)

* Failed msbuild restore needs a reason - [#1606](https://github.com/NuGet/Home/issues/1606)

* NuGetFramework.Version isn't normalized - [#1600](https://github.com/NuGet/Home/issues/1600)

* Binding redirect was added to one project while install-package was called on another - [#1582](https://github.com/NuGet/Home/issues/1582)

* nuget fails to install/update/remove when there is a reference with an Alias - [#1580](https://github.com/NuGet/Home/issues/1580)

* NuGet 2 mirror command does not replace missing packages - [#1574](https://github.com/NuGet/Home/issues/1574)

* Follow up to #1397 - [#1569](https://github.com/NuGet/Home/issues/1569)

* Need to refactor NuGetPackageManagerTests to avoid code redundancy - [#1564](https://github.com/NuGet/Home/issues/1564)

* nuget.exe install jQuery -Version 5.0.0 says 'Package 'jQuery' does not exist'. - [#1554](https://github.com/NuGet/Home/issues/1554)

* ConcurrencyUtilities.FilePathToLockName in NuGet.Packaging issue with long paths? - [#1549](https://github.com/NuGet/Home/issues/1549)

* Dependency range lower bound incorrect in VS - [#1511](https://github.com/NuGet/Home/issues/1511)

* Push doesn't use apiKey for service index when talking to PackagePublish/2.0.0 service - [#1502](https://github.com/NuGet/Home/issues/1502)

* VS2015 Enterprise nuget version 3.2 fails on metadata download - [#1474](https://github.com/NuGet/Home/issues/1474)

* Improve error message when uninstall encounters locked files - [#1458](https://github.com/NuGet/Home/issues/1458)

* NuGet.Xplat - init and add commands - [#1455](https://github.com/NuGet/Home/issues/1455)

* UpdateAll Cancellation implementation - [#1439](https://github.com/NuGet/Home/issues/1439)

* Nuget Restore Circular Dependency  - [#1428](https://github.com/NuGet/Home/issues/1428)

* When %TEMP% and %TMP% env vars overridden to a non-existing folder nuget fails - [#1415](https://github.com/NuGet/Home/issues/1415)

* Search box cannot be accessed with Ctrl+E hotkey after search - [#1404](https://github.com/NuGet/Home/issues/1404)

* Issue during package creation in 'content' folder - [#1388](https://github.com/NuGet/Home/issues/1388)

* NuGet fails to decompress NuGet packages, which are created with a newer ZIP version - [#1381](https://github.com/NuGet/Home/issues/1381)

* When offline or bad package source, build of UWP shows confusing error when packages are missing - [#1363](https://github.com/NuGet/Home/issues/1363)

* Pressing enter in the console does nothing - [#1329](https://github.com/NuGet/Home/issues/1329)

* nuget.exe adding a source with -Config does not update the file unless a username and password is used - [#1315](https://github.com/NuGet/Home/issues/1315)

* nuget.exe sources Add will always add nuget.org - [#1314](https://github.com/NuGet/Home/issues/1314)

* When package failed to install we are getting an exception thrown to the Power Shell console instead of getting an error msg what meant wrong - [#1310](https://github.com/NuGet/Home/issues/1310)

* automatic package restore to run init.ps1 scripts after restoring - [#1292](https://github.com/NuGet/Home/issues/1292)

* Wrong markdown for usage examples - [#1285](https://github.com/NuGet/Home/issues/1285)

* Intermittent failure of NuGet Functional Test UninstallPackageRemoveImportStatement  - [#1278](https://github.com/NuGet/Home/issues/1278)

* NuGet UI at Solution Level for large solutions is very slow - [#1262](https://github.com/NuGet/Home/issues/1262)

* Project "Default" is not found - [#1215](https://github.com/NuGet/Home/issues/1215)

* After saving a solution, 'Restore' button does not show up on Package Manager Console - [#1207](https://github.com/NuGet/Home/issues/1207)

* Files aren't added to VS if they exist before installing a package - [#1190](https://github.com/NuGet/Home/issues/1190)

* Package restore should fail if a package with runtime-specific assets doesn't provide an asset - [#1173](https://github.com/NuGet/Home/issues/1173)

* Intellisense in package manager console in visual studio 2015 is broken - [#1137](https://github.com/NuGet/Home/issues/1137)

* Cannot install managed packages into a CLI project - [#1121](https://github.com/NuGet/Home/issues/1121)

* Support nuget.exe install project.json - [#1045](https://github.com/NuGet/Home/issues/1045)

* Uninstalling Win2D from a UWP c++ project hangs  - [#1030](https://github.com/NuGet/Home/issues/1030)

* .props and .targets from build folder in NuGet package not imported after package restore in VS - [#982](https://github.com/NuGet/Home/issues/982)

* Can't install a package when there are some leftovers in the packages folder - [#492](https://github.com/NuGet/Home/issues/492)

* Saved a nuspec file with wrong reference whitelist when PackageSaveMode = nuspec  - [#434](https://github.com/NuGet/Home/issues/434)

* Completely remove any public methods related to legacy or msbuild based package restore from PackageManagement and Visual Studio extension - [#365](https://github.com/NuGet/Home/issues/365)

* NuGet.PackageManagement: Functional test failure. UninstallPackageHonorPackageReferencesAccordingToProjectFramework - [#164](https://github.com/NuGet/Home/issues/164)

* NuGet.PackageManagement: Use DownloadResource.Progress event - [#50](https://github.com/NuGet/Home/issues/50)

* NuGet.PackageManagement: Should not default null for sourceRepositories to mean all the enabled repositories in SourceRepositoryProvider. Preferable to have function overloads or default values - [#45](https://github.com/NuGet/Home/issues/45)

**Spec:**

* [Spec] Changes to nuget config file and settingsto allow custom hierarchy/scope - [#6420](https://github.com/NuGet/Home/issues/6420)

* [Spec] Schema changes to nuget config file for storing trusted repositories - [#6419](https://github.com/NuGet/Home/issues/6419)

**Docs:**

* Exclude feature doesn't work when using PackageReferences and .NetStandard - [#6399](https://github.com/NuGet/Home/issues/6399)

* [NuGet Packaging] Add Visual Studio Template for NuGet package creation (it's 2018 guys) - [#6391](https://github.com/NuGet/Home/issues/6391)

* Improve Support for Custom / Wrapped NuGet Clients - [#6161](https://github.com/NuGet/Home/issues/6161)

* Documentation URL forwards user to incorrect location in docs. (from NuGet settings window on VS) - [#6100](https://github.com/NuGet/Home/issues/6100)

* NuGet code API - [#5318](https://github.com/NuGet/Home/issues/5318)

* Investigate result of XProj to CSProj migration for nuget.exe - [#3943](https://github.com/NuGet/Home/issues/3943)

* Need a way to test installation of packages via command line - [#2905](https://github.com/NuGet/Home/issues/2905)

* Document with sample building a manage package for all worlds - [#1160](https://github.com/NuGet/Home/issues/1160)

* Improved target documentation - [#358](https://github.com/NuGet/Home/issues/358)

* [Verify/Document] Native (conditional) dependency support.  - [#300](https://github.com/NuGet/Home/issues/300)

**Feature:**

* dotnet-nuget install - [#6375](https://github.com/NuGet/Home/issues/6375)

* Package signing scenarios should be covered by end to end tests - [#6275](https://github.com/NuGet/Home/issues/6275)

* Add additional tests around Timestamp verification involving a mock TSA - [#6239](https://github.com/NuGet/Home/issues/6239)

* Higher priority for search results of reserved prefixes - [#6142](https://github.com/NuGet/Home/issues/6142)

* Feature Request: config file transform redirect configuration - [#6126](https://github.com/NuGet/Home/issues/6126)

* Package Signing Telemetry - [#6075](https://github.com/NuGet/Home/issues/6075)

* Create one mega dll for NuGet and add ngen for hot paths - [#6057](https://github.com/NuGet/Home/issues/6057)

* Feature Request: Add response file support to NuGet.exe - [#5891](https://github.com/NuGet/Home/issues/5891)

* Add 'list' command to NuGet.CommandLine.XPlat - [#5402](https://github.com/NuGet/Home/issues/5402)

* Package should store which source it is installed from - [#4666](https://github.com/NuGet/Home/issues/4666)

* MSBuildTool that depends on DotNetCliTool - [#4653](https://github.com/NuGet/Home/issues/4653)

* Support Restore for Open Folder in VS for solution or project - [#4458](https://github.com/NuGet/Home/issues/4458)

* Enable updating packagereference versions from the command line (for csproj) - [#4358](https://github.com/NuGet/Home/issues/4358)

* Run init.ps1 when nuget package is installed/updated via command-line - [#4318](https://github.com/NuGet/Home/issues/4318)

* Is posible to use ApiKey when restore nugets - [#4049](https://github.com/NuGet/Home/issues/4049)

* Lightweight Solution support for Package Manager Console - [#3989](https://github.com/NuGet/Home/issues/3989)

* Lightweight Solution support for Manage NuGet Packages for Solution UI - [#3988](https://github.com/NuGet/Home/issues/3988)

* Auto-Restore for packages.config - [#3951](https://github.com/NuGet/Home/issues/3951)

* NuGet.exe support for wave3 for api keys improvements - [#3771](https://github.com/NuGet/Home/issues/3771)

* Reconfigure command line tests to test dotnet nuget commands - [#3727](https://github.com/NuGet/Home/issues/3727)

* As part of CI, propagate nuget source to local source server - [#3693](https://github.com/NuGet/Home/issues/3693)

* DotNetCliToolsReference using deps.json - [#3673](https://github.com/NuGet/Home/issues/3673)

* Need to determine API for getting "type: build" dependency information in MSBuild - [#3507](https://github.com/NuGet/Home/issues/3507)

* Command line - list outdated packages, similar to leiningen's "lein ancient" command - [#3415](https://github.com/NuGet/Home/issues/3415)

* NuGet Upgrader: From packages.config to project.json - [#2935](https://github.com/NuGet/Home/issues/2935)

* Enable Vs-Feedback to gather nuget information about a project. - [#2927](https://github.com/NuGet/Home/issues/2927)

* Enable installation of packages with type build or platform. - [#2912](https://github.com/NuGet/Home/issues/2912)

* Excluding specific referenced projects when using IncludeReferencedProjects - [#2897](https://github.com/NuGet/Home/issues/2897)

* Feature Request: Select multiple packages to be installed in VS add-in - [#2872](https://github.com/NuGet/Home/issues/2872)

* "Connect to NuGet Source" via Powershell or Protocol? - [#2703](https://github.com/NuGet/Home/issues/2703)

* Improvements to Browse in Package Manager UI (Multi Install/Uninstall) - [#2584](https://github.com/NuGet/Home/issues/2584)

* `dotnet restore` should have an option for the runtimes to restore - [#2534](https://github.com/NuGet/Home/issues/2534)

* Add support for vector images in the Nuspec iconUrl reference - [#2519](https://github.com/NuGet/Home/issues/2519)

* Add perf tests for more scenarios such as update and package restore - [#2211](https://github.com/NuGet/Home/issues/2211)

* Verified Authors for NuGet.org and packages - [#1882](https://github.com/NuGet/Home/issues/1882)

* Add ability to "-Reinstall" a package from the NuGet Package Manage UI - [#1812](https://github.com/NuGet/Home/issues/1812)

* Network request failure failed restore when all packages were present - [#1810](https://github.com/NuGet/Home/issues/1810)

* allow nuget validation rules to be disabled (-disablerules) - [#1741](https://github.com/NuGet/Home/issues/1741)

* Feature Request: Add 'Browse' button to UI - [#1698](https://github.com/NuGet/Home/issues/1698)

* Request: integrated into VS "Add Reference" dialog - [#1679](https://github.com/NuGet/Home/issues/1679)

* Request: allow "delete" in SolnExplorer to remove NuGet packages - [#1678](https://github.com/NuGet/Home/issues/1678)

* Prerelease Setting Should be Set Per Package Source - [#1666](https://github.com/NuGet/Home/issues/1666)

* Show infobar with links to open readme files when installing packages - [#1662](https://github.com/NuGet/Home/issues/1662)

* Nuget restore needs to support batch restoring for larger projects - [#1653](https://github.com/NuGet/Home/issues/1653)

* Support .NET packages for C++/CLI - [#1645](https://github.com/NuGet/Home/issues/1645)

* Shopping Cart/Ability to pin packages in search, and install them in one go - [#1632](https://github.com/NuGet/Home/issues/1632)

* Package redirects - [#1590](https://github.com/NuGet/Home/issues/1590)

* Prevent installing multiple versions of a package in the solution - [#1579](https://github.com/NuGet/Home/issues/1579)

* Add sym link feature for nupkgs - [#1558](https://github.com/NuGet/Home/issues/1558)

* Improve embeddeding NuGet packages as part of a template/vsix - [#1530](https://github.com/NuGet/Home/issues/1530)

* Disable symbol package upload - [#1524](https://github.com/NuGet/Home/issues/1524)

* Nuget Pack doesn't include resource files (single localized package approach) - [#1482](https://github.com/NuGet/Home/issues/1482)

* Support for runtime light-up dependencies - [#1471](https://github.com/NuGet/Home/issues/1471)

* nuget.exe add and init should add a marker file to feed root - [#1456](https://github.com/NuGet/Home/issues/1456)

* NuGet.Xplat - init and add commands - [#1455](https://github.com/NuGet/Home/issues/1455)

* UpdateAll Cancellation implementation - [#1439](https://github.com/NuGet/Home/issues/1439)

* UpdateAll progress dialog implementation - [#1438](https://github.com/NuGet/Home/issues/1438)

* 'nuget.exe delete' to delete packages from the offline feed - [#1422](https://github.com/NuGet/Home/issues/1422)

* nuget.exe: There are basic tests for nuget.exe update. Need more - [#1322](https://github.com/NuGet/Home/issues/1322)

* nuget.exe excludes *.nuspec files and there is no way to bypass this - [#1290](https://github.com/NuGet/Home/issues/1290)

* Support -ExcludeVersion for Restore and Update commands - [#1286](https://github.com/NuGet/Home/issues/1286)

* nuget.exe update should support project.json - [#1276](https://github.com/NuGet/Home/issues/1276)

* Nuget Package Visualizer is missing in Visual Studio 2015 Enterprise - [#1264](https://github.com/NuGet/Home/issues/1264)

* Better handling of restore in VS with floating versions - [#1256](https://github.com/NuGet/Home/issues/1256)

* Localization for nuget v3 command line - [#1165](https://github.com/NuGet/Home/issues/1165)

* Hierarchical view in packages - [#1155](https://github.com/NuGet/Home/issues/1155)

* Feature request: Local package "groups" - [#1149](https://github.com/NuGet/Home/issues/1149)

* NuGet config is hierarchical. NuGet Settings dialog does not handle or reflect that - [#1114](https://github.com/NuGet/Home/issues/1114)

* nuget list does not support wildcards - [#1001](https://github.com/NuGet/Home/issues/1001)

* [wish] nuget proxy/cache - [#458](https://github.com/NuGet/Home/issues/458)

* Users should be able to specify CopyLocal property for the references - [#446](https://github.com/NuGet/Home/issues/446)

* Add a setting/switch to bypass Recycle bin when removing/updating - [#417](https://github.com/NuGet/Home/issues/417)

**DCR:**

* Signing API doesn't expose timestamps easily - [#6294](https://github.com/NuGet/Home/issues/6294)

* Pack should have a collector logger and throw proper error and warning codes - [#6217](https://github.com/NuGet/Home/issues/6217)

* `NuGet.CommandLine.XPlat  package add` support `--config`  - [#6210](https://github.com/NuGet/Home/issues/6210)

* Support Organization parameter in NuGet push and dotnet nuget push - [#5925](https://github.com/NuGet/Home/issues/5925)

* Feature Request: Need the ability for NuGet.exe pack to create a nupkg without the version in the package name - [#5893](https://github.com/NuGet/Home/issues/5893)

* Honor WarningsNotAsErrors - [#5375](https://github.com/NuGet/Home/issues/5375)

* Request for configurable names of '\packages' and 'packages.config' - [#4580](https://github.com/NuGet/Home/issues/4580)

* dotnet new and dotnet build -- can that do the restore for you? - [#4495](https://github.com/NuGet/Home/issues/4495)

* NuGet Package Manager Always Returns to Installed Tab Rather than Last Tab - [#4299](https://github.com/NuGet/Home/issues/4299)

* Discover msbuild project files from a folder in nuget.exe restore - [#3942](https://github.com/NuGet/Home/issues/3942)

* Refactor GetFolderPath for ProgramFiles - [#3859](https://github.com/NuGet/Home/issues/3859)

* Cleanup CPS integration code when API is settled - [#3810](https://github.com/NuGet/Home/issues/3810)

* Can't browse packages while install is running - [#3796](https://github.com/NuGet/Home/issues/3796)

* Enhance TFM list for RC2 - [#3761](https://github.com/NuGet/Home/issues/3761)

* Consider: Allow clearing a specific package from NuGet cache(s) - [#3688](https://github.com/NuGet/Home/issues/3688)

* Provide more information when template package install fails - [#3413](https://github.com/NuGet/Home/issues/3413)

* dll referred also to be shown in nuget window in VS - [#3411](https://github.com/NuGet/Home/issues/3411)

* Consider collapsing absolute path log message when using relative push source  - [#2958](https://github.com/NuGet/Home/issues/2958)

* Project.json dependency resolver algorithm doesn't support dependency inversion - [#2889](https://github.com/NuGet/Home/issues/2889)

* Consider moving the .nuget directory on Windows to a subdirectory of %AppData% - [#2861](https://github.com/NuGet/Home/issues/2861)

* Allow compression level to be set when running `nuget pack` - [#2852](https://github.com/NuGet/Home/issues/2852)

* Consider making project.json references case insensitive - [#2819](https://github.com/NuGet/Home/issues/2819)

* Shared packages are automatically deleted during uninstall (repositoryPath) - [#2780](https://github.com/NuGet/Home/issues/2780)

* Changelog summary and links for package manager updates - [#2753](https://github.com/NuGet/Home/issues/2753)

* IVsPackageInstallerEvents could do with some more events - [#2751](https://github.com/NuGet/Home/issues/2751)

* Updating packages applies assembly binding redirects to all projects, including class libraries - [#2748](https://github.com/NuGet/Home/issues/2748)

* nuget.exe should support a relative source path for push command line: "nuget push -Source" - [#2723](https://github.com/NuGet/Home/issues/2723)

* Make MSBuildNuGetProjectSystemUtility public - [#2702](https://github.com/NuGet/Home/issues/2702)

* Include information about the dependency chain when a package is unable to restore - [#2693](https://github.com/NuGet/Home/issues/2693)

* Impossible to target Xamarin.Mac .NET 4.5 Framework - [#2662](https://github.com/NuGet/Home/issues/2662)

* dotnet CLI app cannot have global.json in the same directory as the project.json - [#2581](https://github.com/NuGet/Home/issues/2581)

* NuGet command to purge/clean cache - [#2308](https://github.com/NuGet/Home/issues/2308)

* Add web caching for UI and gather scenarios - [#2287](https://github.com/NuGet/Home/issues/2287)

* Normalize use of loggers in UI - [#2286](https://github.com/NuGet/Home/issues/2286)

* CLI "delete" is actually "unlist" - [#2277](https://github.com/NuGet/Home/issues/2277)

* Finish up IVS APIs to facilitate package indexing - [#2268](https://github.com/NuGet/Home/issues/2268)

* Add support for pre/post restore scripts  - [#2260](https://github.com/NuGet/Home/issues/2260)

* Consider adding StopLoadingThreshold parameter to nuget.config - [#2259](https://github.com/NuGet/Home/issues/2259)

* Improve design of the package search item loader - [#2223](https://github.com/NuGet/Home/issues/2223)

* Need better test coverage for the search results merging algorithm - [#2222](https://github.com/NuGet/Home/issues/2222)

* All-sources feature implementation doesn't follow code conventions - [#2221](https://github.com/NuGet/Home/issues/2221)

* Improvements for uninstall in scenarios where restore doesn't work - [#2213](https://github.com/NuGet/Home/issues/2213)

* Enable preview or file count/list on Nuget installation - [#2207](https://github.com/NuGet/Home/issues/2207)

* ProjectModel support for reading and writing lock file and project.json files - [#1993](https://github.com/NuGet/Home/issues/1993)

* List command for xplat - [#1990](https://github.com/NuGet/Home/issues/1990)

* Need way to specify item metadata on contentFiles - [#1979](https://github.com/NuGet/Home/issues/1979)

* Public CI server? - [#1904](https://github.com/NuGet/Home/issues/1904)

* Debug Experience -- telling installer to use .symbols package when both exist - [#1854](https://github.com/NuGet/Home/issues/1854)

* Version Token in NuSpec file not being replaced properly when using version flag with csproj - [#1795](https://github.com/NuGet/Home/issues/1795)

* Filter for Installed shows all packages and not just from selected packages source. - [#1761](https://github.com/NuGet/Home/issues/1761)

* Make extracted /Content read-only - [#1729](https://github.com/NuGet/Home/issues/1729)

* UI: options should also appear in Tools -> Options - [#1655](https://github.com/NuGet/Home/issues/1655)

* Need to move guardrails out of restore - [#1623](https://github.com/NuGet/Home/issues/1623)

* Package Manager Console and nuget.exe UPDATE function are inconsistent - [#1591](https://github.com/NuGet/Home/issues/1591)

* nuget.exe install generates a lot of (parallel) network requests compared to UI or nuget.exe.2.* - [#1552](https://github.com/NuGet/Home/issues/1552)

* Need to make changes to NuGet when csproj moves to CPS - [#1515](https://github.com/NuGet/Home/issues/1515)

* Packages are missing when selected pre-release option! - [#1480](https://github.com/NuGet/Home/issues/1480)

* Consider adding a warning for users of nuget.targets - [#1411](https://github.com/NuGet/Home/issues/1411)

* Idea: Make nuget.config configuration options available as env vars - [#1396](https://github.com/NuGet/Home/issues/1396)

* Different TypeScript versions in the package - [#1339](https://github.com/NuGet/Home/issues/1339)

* NuGet.PackageManagement.dll should not use NuGet.Core and NuGet.Protocol.Core.v2 - [#1323](https://github.com/NuGet/Home/issues/1323)

* Error message on installing package into C++ project could be more helpful - [#1242](https://github.com/NuGet/Home/issues/1242)

* Solution packages aren't flagged as `developmentDependency="true"`. - [#1093](https://github.com/NuGet/Home/issues/1093)

* API to bulk install packages to project.json - [#1023](https://github.com/NuGet/Home/issues/1023)

* API to parse NuGetVersions - [#1022](https://github.com/NuGet/Home/issues/1022)

* 'nuget list all' should say why it only returns 30 packages - [#1000](https://github.com/NuGet/Home/issues/1000)

* Add quirking/package version to eliminate the "any" lib folder - [#496](https://github.com/NuGet/Home/issues/496)

* Make it easier to omit packages from TFVC - [#493](https://github.com/NuGet/Home/issues/493)

* Manage NuGet Packages dialog shows 'Authors' of package but not 'Owners' - [#442](https://github.com/NuGet/Home/issues/442)

* nuget pack -symbols: Missing source file error when the package contains XAML - [#304](https://github.com/NuGet/Home/issues/304)

