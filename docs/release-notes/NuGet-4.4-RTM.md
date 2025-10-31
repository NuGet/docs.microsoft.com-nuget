---
title: NuGet 4.4 RTM Release Notes
description: Release notes for NuGet 4.4 RTM including known issues, bug fixes, added features, and DCRs.
author: JonDouglas
ms.author: jodou
ms.date: 08/14/2017
ms.topic: release-notes
ms.reviewer: anangaur
---

# NuGet 4.4 Release Notes

[Visual Studio 2017 15.4 RTW](https://www.visualstudio.com/news/releasenotes/vs2017-relnotes) comes with NuGet 4.4 RTM.

## Summary: What's New in 4.4.0

## Summary: What's New in 4.4.2

* Security Fix: Permissions on files created inside ~/.nuget are too open [#7673](https://github.com/NuGet/Home/issues/7673) [CVE-2019-0757](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0757)

## Summary: What's New in 4.4.3

* Security Fix: Files inside of NUPKGs can have a relative path above the NUPKG directory [#7906](https://github.com/NuGet/Home/issues/7906)

## Known issues

### Issues with .NET Standard 2.0 with .NET Framework & NuGet 

.NET Standard & its tooling was designed such that projects targeting .NET Framework 4.6.1 can consume NuGet packages & projects targeting .NET Standard 2.0 or earlier. [This document](https://github.com/dotnet/standard/issues/481) summarizes the issues around that scenario, the plan for addressing them, and workarounds you can deploy with today's state of the tooling.

### While using Package Manager Console, 'Enter' key may not work

#### Issue

Occasionally, the enter key does not work in the Package Manager Console. If you see this, please check out the progress on the fix, and provide any additional helpful information about your repro steps. [NuGet#4204](https://github.com/NuGet/Home/issues/4204) [NuGet#4570](https://github.com/NuGet/Home/issues/4570)

#### Workaround

Restart Visual Studio and open the PMC before opening the solution. Alternatively, try deleting the `project.lock.json` and restoring again.

### You are unable to view, add, or update DotNetCLITools, using Nuget Package Manager

#### Issue

NuGet Package Manager does not display and does not allow add/update of DotNetCLITools. [NuGet#4256](https://github.com/NuGet/Home/issues/4256)

#### Workaround

DotNetCLIToolReferences must be manually edited in your project file.

### Retargeting target framework version may lead to incomplete Intellisense

#### Issue

Retargeting target framework version may lead to incomplete Intellisense, in Visual Studio. This happens when you are using PackageReferences as the package manager format. [NuGet#4216](https://github.com/NuGet/Home/issues/4216)

#### Workaround

Do a manual restore.

### A package in a .NET Core project that contains an assembly with an invalid signature, can trigger an infinite restore loop

#### Issue

Occasionally, when you use a package that contains an assembly with an invalid signature or when the package version is set with 'DateTime' ticker, it causes the package auto-restore to run in an infinite loop (dotnet/project-system#1457).

#### Workaround

There is no workaround at this time.

## Issues fixed in NuGet 4.4 RTM timeframe

[NuGet 4.3 RTM Release Notes](../release-notes/nuget-4.3-RTM.md) - Lists all the issues fixed for NuGet 4.3 RTM

### Features

- Support for Lightweight Solution Load in PMC and NuGet PM UI scenarios - [#5180](https://github.com/NuGet/Home/issues/5180)

- The msbuild pack target should have a public hook for running user targets before itself - [#5143](https://github.com/NuGet/Home/issues/5143)

- Feature: Add dependencyVersion switch to nuget install - [#1806](https://github.com/NuGet/Home/issues/1806)

- uap10.0.TODO.0 should map to .NET Standard 2.0 for NuGet - [#5684](https://github.com/NuGet/Home/issues/5684)

- Support Visual Studio Build Tools SKU with msbuild /t:restore - [#5562](https://github.com/NuGet/Home/issues/5562)

- During restore, generate an error if .NET 4.6.1 support for .NET Standard 2.0 is required but not installed - [#5325](https://github.com/NuGet/Home/issues/5325)

- Package ID prefix reservation client UI - [#5572](https://github.com/NuGet/Home/issues/5572)

- deliver localized nuget components to support dotnet.exe localization - [#4336](https://github.com/NuGet/Home/issues/4336)

### Bugs

- Different project path casings can cause restore to lose PackageReferences - [#5855](https://github.com/NuGet/Home/issues/5855)

- Move error codes with warning numbers to error range - [#5824](https://github.com/NuGet/Home/issues/5824)

- Misleading error when .NET Standard version is not known to be compatible with target framework  - [#5818](https://github.com/NuGet/Home/issues/5818)

- Test files with confusing licenses - [#5776](https://github.com/NuGet/Home/issues/5776)

- Missing license headers in EndToEnd test templates - [#5774](https://github.com/NuGet/Home/issues/5774)

- packages.config restore shows errors as NU1000 - [#5743](https://github.com/NuGet/Home/issues/5743)

- nuget.exe install should have DisableParallelProcessing on mono - [#5741](https://github.com/NuGet/Home/issues/5741)

- nuget.exe install incorrectly disables caching - [#5737](https://github.com/NuGet/Home/issues/5737)

- VS Running the restore command for packages.config when Restore is disabled displays incorrect message  - [#5718](https://github.com/NuGet/Home/issues/5718)

- VS; Running the restore command when Restore is disabled displays a confusing message - [#5659](https://github.com/NuGet/Home/issues/5659)

- GetRestoreDotnetCliToolsTask fails when missing version metadata - [#5716](https://github.com/NuGet/Home/issues/5716)

- dotnet
  - dotnetcore add package can clear empty lines from a csproj  - [#5697](https://github.com/NuGet/Home/issues/5697)

- Source names of credential settings in NuGet.Config are case sensitive - [#5695](https://github.com/NuGet/Home/issues/5695)

- Enabling GeneratePackageOnBuild deleted my entire history of packages - [#5676](https://github.com/NuGet/Home/issues/5676)

- Restore will not restore mono.cecil or semver packages, but all other packages get restored. - [#5649](https://github.com/NuGet/Home/issues/5649)

- Errors and Warnings - bad error when a source in unavailable.  - [#5644](https://github.com/NuGet/Home/issues/5644)

- [DesignConsistency] NuGet Installation status text doesn’t look correct on dark theme currently. - [#5642](https://github.com/NuGet/Home/issues/5642)

- Update packages at solution updates/installs for all the projects - [#5508](https://github.com/NuGet/Home/issues/5508)

- dotnet
  - dotnetcore pack behaves differently depending on TargetFramework vs TargetFrameworks - [#5281](https://github.com/NuGet/Home/issues/5281)

- Included DLLs inside Tools folder throw warnings - [#5020](https://github.com/NuGet/Home/issues/5020)

- NuGet.ContentModel consumes too much memory for string operations - [#4714](https://github.com/NuGet/Home/issues/4714)

- RuntimeEnvironmentHelper.IsLinux returns true for OSX - [#4648](https://github.com/NuGet/Home/issues/4648)

- 'dotnet pack' puts nuspec under obj instead of obj\Debug - [#4644](https://github.com/NuGet/Home/issues/4644)

- Nuget extremely slow package upgrade - [#4534](https://github.com/NuGet/Home/issues/4534)

- CPS out of sync with Restore with larger solutions that haven't turned on LSL (lightweight solution restore) - [#4307](https://github.com/NuGet/Home/issues/4307)

- SemVer 2.0 - nuget pack with provided version ignores metadata (3.5.0-rtm-1938) - [#3643](https://github.com/NuGet/Home/issues/3643)

- Nuget.exe (3.+) install package with Version number and ExcludeVersion flag doesn't update package to newer version - [#2405](https://github.com/NuGet/Home/issues/2405)

- Project.json restore should warn when top-level packages violate constraints - [#2358](https://github.com/NuGet/Home/issues/2358)

- -ConfigFile is not setting custom config on install command - [#1646](https://github.com/NuGet/Home/issues/1646)

- nuget.exe install does not honor '-DisableParallelProcessing' switch - [#1556](https://github.com/NuGet/Home/issues/1556)

- Disabled sources still used by DotNet.exe or msbuild.exe - [#5704](https://github.com/NuGet/Home/issues/5704)

- Fix hangs in LSL scenario - [#5685](https://github.com/NuGet/Home/issues/5685)

### DCRs

- nuget.exe install TargetFramework support - [#5736](https://github.com/NuGet/Home/issues/5736)

- Add different msbuild task UserAgent strings (netcore vs desktop msbuild) - [#5709](https://github.com/NuGet/Home/issues/5709)

- PackagePathResolver.GetPackageDirectoryName should be virtual - [#5700](https://github.com/NuGet/Home/issues/5700)

- [DesignConsistency] Confusing message when adding a NuGet package - [#5641](https://github.com/NuGet/Home/issues/5641)

- [Warnings and errors] NoWarn does not flow transitively through P2P references - [#5501](https://github.com/NuGet/Home/issues/5501)

- Lightweight Solution Load: Common Core for PM UI, PMC, and IVs- - [#5057](https://github.com/NuGet/Home/issues/5057)

- Lightweight Solution Load: Support - PMC - [#5053](https://github.com/NuGet/Home/issues/5053)

- Add support for pre-restore MSBuild target that Visual Studio triggers - [#4781](https://github.com/NuGet/Home/issues/4781)

- Add a public target to NuGet.targets that can be referenced using BeforeTargets - [#4634](https://github.com/NuGet/Home/issues/4634)

- Pack target can't create contentFiles with build actions correctly - [#4166](https://github.com/NuGet/Home/issues/4166)

- RestoreOperationLogger.Do blocks thread pool threads - [#5663](https://github.com/NuGet/Home/issues/5663)

### Docs

- Docs for Install command DependencyVersion and Framework flags - [#5858](https://github.com/NuGet/Home/issues/5858)

- Update to docs on NuGet warnings and errors - [#5857](https://github.com/NuGet/Home/issues/5857)

## Links to GitHub issues fixed in 4.4 RTM

[Issues List 1](https://github.com/NuGet/Home/issues?q=is:issue+is:closed+milestone:"4.4")

[Issues List 2](https://github.com/NuGet/Home/issues?q=is:issue+is:closed+milestone:%224.4+-+7%2F31+through+8%2F18%22)

[Issues List 3](https://github.com/NuGet/Home/issues?q=is:issue+is:closed+milestone:%224.4+-+7%2F10+through+7%2F28%22)
