---
title: NuGet 5.0 RTM Release Notes
description: Release notes for NuGet 5.0 including known issues, bug fixes, new features, and DCRs.
author: JonDouglas
ms.author: jodou
ms.date: 04/02/2019
ms.topic: release-notes
---

# NuGet 5.0 Release Notes

NuGet distribution vehicles:

| NuGet version | Available in Visual Studio version| Available in .NET SDK(s)|
|:---|:---|:---|
| [**5.0.0**](https://nuget.org/downloads) | [Visual Studio 2019 version 16.0](https://visualstudio.microsoft.com/downloads/) | [2.1.602](https://dotnet.microsoft.com/download/dotnet-core/2.1)<sup>1</sup>, [2.2.202](https://dotnet.microsoft.com/download/dotnet-core/2.2)<sup>2</sup> |
| [**5.0.2**](https://nuget.org/downloads) | [Visual Studio 2019 version 16.0.4](https://visualstudio.microsoft.com/downloads/) | [2.1.60X](https://dotnet.microsoft.com/download/dotnet-core/2.1)<sup>1</sup>, [2.2.20X](https://dotnet.microsoft.com/download/dotnet-core/2.2)<sup>2</sup> |

<sup>1</sup>Installed with Visual Studio 2019 with .NET Core workload 

<sup>2</sup>Available as an optional install with Visual Studio 2019 with .NET Core workload

## Summary: What's New in 5.0

* Support for restoring [filtered solutions](/visualstudio/ide/filtered-solutions) in Visual Studio 2019 - [#5820](https://github.com/NuGet/Home/issues/5820)
* `BuildTransitive` folder enables packages to transitively contribute targets/props to the host project - [#6091](https://github.com/NuGet/Home/issues/6091)
* Better support for PackageReference scenarios in NuGet IVs APIs - [#7005](https://github.com/NuGet/Home/issues/7005), [#7493](https://github.com/NuGet/Home/issues/7493)
* `nuget.exe pack project.json` has been deprecated - [#7928](https://github.com/NuGet/Home/issues/7928)
* Gen 1 Credential Provider plugin has been superseded by [Gen 2](../reference/extensibility/nuget-cross-platform-authentication-plugin.md) and will soon be deprecated - [#7819](https://github.com/NuGet/Home/issues/7819)

## Issues fixed in this release

**Bugs**

* When doing a NoOp restore, avoid *.dgspec.json write in obj directory - [#7854](https://github.com/NuGet/Home/issues/7854)

* Permissions on files created inside ~/.nuget are too open - [#7673](https://github.com/NuGet/Home/issues/7673)

* `dotnet list package --outdated` doesn't work with sources that need auth - [#7605](https://github.com/NuGet/Home/issues/7605)

* NuGet.VisualStudio.IVsPackageInstaller - calling on a project with no package references always uses packages.config, even if the default is set to PackageReference - [#7005](https://github.com/NuGet/Home/issues/7005)

* PMC: Update-Package reinstall fails ("Unable to find package") on delisted packages. - [#7268](https://github.com/NuGet/Home/issues/7268)

* Add third party notice in our repo and VSIX - [#7409](https://github.com/NuGet/Home/issues/7409)

* NuGet.VisualStudio.IVsPackageInstaller.InstallPackage should install latest version when no version given - [#7493](https://github.com/NuGet/Home/issues/7493)

* --interactive support for dotnet nuget push - [#7519](https://github.com/NuGet/Home/issues/7519)

* When restoring with lock file, NU1603 warning shouldn't be raised. - [#7529](https://github.com/NuGet/Home/issues/7529)

* NuGet should not print project path during restore with minimal logging - [#7647](https://github.com/NuGet/Home/issues/7647)

* --interactive support for dotnet remove package - [#7727](https://github.com/NuGet/Home/issues/7727)

* Add back NuGet.Packaging.Core with TypeForwardedTo attrs - [#7768](https://github.com/NuGet/Home/issues/7768)

* plugins_cache needs shorter path to work well - [#7770](https://github.com/NuGet/Home/issues/7770)

* Prefer path for msbuild discovery if user didn't ask for specific msbuild version - [#7786](https://github.com/NuGet/Home/issues/7786)

* `nuget.exe /?` should list correct msbuild versions - [#7794](https://github.com/NuGet/Home/issues/7794)

* NuGet.targets(498,5): error : Could not find a part of the path '/tmp/NuGetScratch - on mono - [#7793](https://github.com/NuGet/Home/issues/7793)

* restore unnecessarily enumerates the contents of all versions of referenced package in the machine cache - [#7639](https://github.com/NuGet/Home/issues/7639)

* MSBuild auto-detection always selects 16.0 after installing VS 2019 Preview - [#7621](https://github.com/NuGet/Home/issues/7621)

* dotnet list package on a solution outputs duplicate entries for framework - [#7607](https://github.com/NuGet/Home/issues/7607)

* Exception "Empty path name is not legal" when calling IVsPackageInstaller.InstallPackage on old projects and packages folder does not exist. - [#5936](https://github.com/NuGet/Home/issues/5936)

* msbuild /t:restore minimal verbosity should be more minimal - [#4695](https://github.com/NuGet/Home/issues/4695)

* VS 16.0's NuGet UI has unreadable tabs due to color problems - [#7735](https://github.com/NuGet/Home/issues/7735)

* NuGet.Core & NuGet.Clients License.txt clarification - [#7629](https://github.com/NuGet/Home/issues/7629)

* Restore unnecessarily enumerates global package folder in attempt to determine type - [#7596](https://github.com/NuGet/Home/issues/7596)

* Errors from lock file enforcement should show up in Error List Window - [#7429](https://github.com/NuGet/Home/issues/7429)

* Fix NuGet.Configuration issues - [#7326](https://github.com/NuGet/Home/issues/7326)

* Adapt to MSBuild updating its install location - [#7325](https://github.com/NuGet/Home/issues/7325)

* NuGet.Build.Tasks.Pack should be a development dependency - [#7249](https://github.com/NuGet/Home/issues/7249)

* Add pack extension point for including debug symbols - [#7234](https://github.com/NuGet/Home/issues/7234)

* `dotnet pack` should preserve dependency version range in the created nupkg (even if floating version is used) - [#7232](https://github.com/NuGet/Home/issues/7232)

* `dotnet restore` fails on authenticated source when user-level config also has source - [#7209](https://github.com/NuGet/Home/issues/7209)

* Pack should not restrict the set of BuildActions for content files - [#7155](https://github.com/NuGet/Home/issues/7155)

* Using a ProjectReference which requires AssetTargetFallback to succeed, should warn. - [#7137](https://github.com/NuGet/Home/issues/7137)

* Deadlock due to threading issues when calling into CPS (CommonProjectSystem) - [#7103](https://github.com/NuGet/Home/issues/7103)

* `dotnet add package` doesn't use credentials from global config for a source specified in local config - [#6935](https://github.com/NuGet/Home/issues/6935)

* Threading issues with MEF being called on async code paths - [#6771](https://github.com/NuGet/Home/issues/6771)

* Signing: error reported twice and without call stack - [#6455](https://github.com/NuGet/Home/issues/6455)

* Installing a signed package with untrusted signing certificate should show error - [#6318](https://github.com/NuGet/Home/issues/6318)

* NuGet restore improperly NoOps when 2 projects are sharing obj directory - [#6114](https://github.com/NuGet/Home/issues/6114)

* Cannot use PAT with `dotnet restore` on Linux with packages from authenticated feed - [#5651](https://github.com/NuGet/Home/issues/5651)

* dotnet restore fails due to disabled machine wide feed - [#5410](https://github.com/NuGet/Home/issues/5410)

**DCRs**

* Warn of future removal of "dotnet pack project.json" - [#7928](https://github.com/NuGet/Home/issues/7928)
 
* Add a deprecation warning for Gen1 credential plugin - [#7819](https://github.com/NuGet/Home/issues/7819)
 
* Signing: Enabled Repo to require client verification of every package as repo signed -- via RepositorySignatures/5.0.0 resource - [#7759](https://github.com/NuGet/Home/issues/7759)

* limit http request number per source through NuGet.Config - [#4538](https://github.com/NuGet/Home/issues/4538)

* NuGet should target Net472 (to help Cleanup the 16.0 build of the VSIX) - [#7143](https://github.com/NuGet/Home/issues/7143)

* PMC: Remove OpenPackagePage command - [#7384](https://github.com/NuGet/Home/issues/7384)

* Make NetCoreApp 3.0 map to NetStandard 2.1 - [#7762](https://github.com/NuGet/Home/issues/7762)

* Add netstandard2.0 support to NuGet.* packages - [#6516](https://github.com/NuGet/Home/issues/6516)

* Allow package authors to define build assets transitive behavior - [#6091](https://github.com/NuGet/Home/issues/6091)

* Support VS 2019 Solution Filter feature. Also supports project not in solution, or unloaded projects. Need to restore complete solution (via CLI or VS) first - [#5820](https://github.com/NuGet/Home/issues/5820)

* NuGet 5.0 assemblies to require .NET 4.7.2 (via TFM change) - [#7510](https://github.com/NuGet/Home/issues/7510)

* NuGetLicenseData from NuGet.Packaging should be a public type. Update license metadata ingested from spdx. - [#7471](https://github.com/NuGet/Home/issues/7471)

* Remove obsolete Settings APIs - [#7294](https://github.com/NuGet/Home/issues/7294)

* Workaround restore timeouts on systems with 1 cpu - [#6742](https://github.com/NuGet/Home/issues/6742)

* NuGet prefers NTLM auth even if there are credentials in NuGet.config - add config option to filter auth types for credentials - [#5286](https://github.com/NuGet/Home/issues/5286)

* Enable EmbedInteropTypes for PackageReference (matching Packages.Config capability) - [#2365](https://github.com/NuGet/Home/issues/2365)

**[List of all issues fixed in this release - 5.0 RTM](https://github.com/NuGet/Home/milestone/84?closed=1)**

## Summary: What's New in 5.0.2

* Security (when run via dotnet.exe or mono.exe) - The obj folder should be created with correct permissions [#7908](https://github.com/NuGet/Home/issues/7908)

* nuget.exe restore on mono/MacOS fails with custom nuget.config and `PackageSignatureValidity: False` [#8011](https://github.com/NuGet/Home/issues/8011)


## Known issues

### Packages in FallbackFolders installed by .NET Core SDK are custom installed, and fail signature validation. - [#7414](https://github.com/NuGet/Home/issues/7414)
#### Issue
When using dotnet.exe 2.x to restore a project that multi-targets netcoreapp 1.x and netcoreapp 2.x, the fallback folder is treated as a file feed. This means, when restoring, NuGet will pick the package from the fallback folder and try to install it into the global packages folder and do the usual signing validation which fails.<br>
#### Workaround
Disable the usage of the fallback folder by setting the `RestoreAdditionalProjectSources` to nothing:

`<RestoreAdditionalProjectSources></RestoreAdditionalProjectSources>`

Use this with caution as packages that would be restored from the fallback folder will now be downloaded from NuGet.org.
