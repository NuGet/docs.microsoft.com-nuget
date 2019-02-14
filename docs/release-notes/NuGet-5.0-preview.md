---
title: NuGet 5.0-preview Release Notes
description: Release notes for NuGet 5.0 previews including known issues, bug fixes, new features, and DCRs.
author: anangaur
ms.author: anangaur
ms.date: 1/25/2019
ms.topic: conceptual
---

# NuGet 5.0 Preview Release Notes

## NuGet 5.0 Preview Releases

* February 13, 2019 - [NuGet 5.0 Preview 3](#summary-whats-new-in-50-preview-3)
* January 23, 2019 - [NuGet 5.0 Preview 2](#summary-whats-new-in-50-preview-2)

## Summary: What's New in NuGet 5.0 Preview 3

### Issues fixed in this release

**Bugs:**

* nuget.exe /? should list correct msbuild versions - [#7794](https://github.com/NuGet/Home/issues/7794)

* NuGet.targets(498,5): error : Could not find a part of the path '/tmp/NuGetScratch - on mono - [#7793](https://github.com/NuGet/Home/issues/7793)

* restore unnecessarily enumerates the contents of all versions of referenced package in the machine cache - [#7639](https://github.com/NuGet/Home/issues/7639)

* MSBuild auto-detection always selects 16.0 after installing VS 2019 Preview - [#7621](https://github.com/NuGet/Home/issues/7621)

* dotnet list package on a solution outputs duplicate entries for framework - [#7607](https://github.com/NuGet/Home/issues/7607)

* Exception "Empty path name is not legal" when calling IVsPackageInstaller.InstallPackage on old projects and packages folder does not exist. - [#5936](https://github.com/NuGet/Home/issues/5936)

* msbuild /t:restore minimal verbosity should be more minimal - [#4695](https://github.com/NuGet/Home/issues/4695)

**DCRs**

* Allow package authors to define build assets transitive behavior - [#6091](https://github.com/NuGet/Home/issues/6091)

* Enable restore in VS to succeed if a project is not part of solution or is not loaded, but has previously been restored - [#5820](https://github.com/NuGet/Home/issues/5820)


## Summary: What's New in 5.0 Preview 2

### Issues fixed in this release

**Bugs:**

* VS 16.0's NuGet UI has unreadable tabs due to color problems - [#7735](https://github.com/NuGet/Home/issues/7735)

* NuGet.Core & NuGet.Clients License.txt clarification - [#7629](https://github.com/NuGet/Home/issues/7629)

* Restore unnecessarily enumerates global package folder in attempt to determine type - [#7596](https://github.com/NuGet/Home/issues/7596)

* Errors from lock file enforcement should show up in Error List Window - [#7429](https://github.com/NuGet/Home/issues/7429)

* Fix NuGet.Configuration issues - [#7326](https://github.com/NuGet/Home/issues/7326)

* Adapt to MSBuild updating it's install location.  - [#7325](https://github.com/NuGet/Home/issues/7325)

* NuGet.Build.Tasks.Pack should be a development dependency - [#7249](https://github.com/NuGet/Home/issues/7249)

* Add pack extension point for including debug symbols - [#7234](https://github.com/NuGet/Home/issues/7234)

* dotnet pack should preserve dependency version range in the created nupkg. (even if floating version is used) - [#7232](https://github.com/NuGet/Home/issues/7232)

* dotnet restore fails on authenticated source when user-level config also has source - [#7209](https://github.com/NuGet/Home/issues/7209)

* Pack should not restrict the set of BuildActions for content files - [#7155](https://github.com/NuGet/Home/issues/7155)

* Using a projectreference which requires AssetTargetFallback to succeed, should warn. - [#7137](https://github.com/NuGet/Home/issues/7137)

* Deadlock due to threading issues when calling into CPS (CommonProjectSystem) - [#7103](https://github.com/NuGet/Home/issues/7103)

* dotnet add package won't use credentials from global config for a source specified in local config - [#6935](https://github.com/NuGet/Home/issues/6935)

* Threading issues with MEF being called on async codepaths - [#6771](https://github.com/NuGet/Home/issues/6771)

* Signing:  error reported twice and without call stack - [#6455](https://github.com/NuGet/Home/issues/6455)

* Installing a signed package with untrusted signing certificate should show error - [#6318](https://github.com/NuGet/Home/issues/6318)

* NuGet restore improperly NoOps when 2 projects are sharing obj directory - [#6114](https://github.com/NuGet/Home/issues/6114)

* Cannot use PAT with dotnet restore on Linux with packages from authenticated feed - [#5651](https://github.com/NuGet/Home/issues/5651)

* dotnet restore fails due to disabled machine wide feed - [#5410](https://github.com/NuGet/Home/issues/5410)

**DCRs**

* NuGet 5.0 assemblies to require .NET 4.7.2 (via TFM change) - [#7510](https://github.com/NuGet/Home/issues/7510)

* NuGetLicenseData from NuGet.Packaging should be a public type. Update license metadata ingested from spdx. - [#7471](https://github.com/NuGet/Home/issues/7471)

* Remove obsolete Settings APIs - [#7294](https://github.com/NuGet/Home/issues/7294)

* Workaround restore timeouts on systems with 1 cpu - [#6742](https://github.com/NuGet/Home/issues/6742)

* NuGet prefers NTLM auth even if there are credentials in NuGet.config - add config option to filter auth types for credentials - [#5286](https://github.com/NuGet/Home/issues/5286)

* Enable EmbedInteropTypes for PackageReference (matching Packages.Config capability) - [#2365](https://github.com/NuGet/Home/issues/2365)

[List of all issues fixed in this release 5.0.0-preview2](https://github.com/NuGet/Home/issues?q=is%3Aissue+is%3Aclosed+milestone%3A%224.9.2")


### Known issues

#### dotnet nuget push --interactive gives an error on Mac. - [#7519](https://github.com/NuGet/Home/issues/7519)
**Issue**
The `--interactive` argument is not being forwarded by the dotnet cli and results in the error `error: Missing value for option 'interactive'`
**Workaround**
Run any other dotnet command with the interactive option such as `dotnet restore --interactive` and authenticate. The authentication then might be cached by the credential provider. Then run `dotnet nuget push`.

#### Packages in FallbackFolders installed by .NET Core SDK are custom installed, and fail signature validation. - [#7414](https://github.com/NuGet/Home/issues/7414)
**Issue**
When using dotnet.exe 2.x to restore a project that multi-targets netcoreapp 1.x and netcoreapp 2.x, the fallback folder is treated as a file feed. This means, when restoring, NuGet will pick the package from the fallback folder and try to install it into the global packages folder and do the usual signing validation which fails.
**Workaround**
Disable the usage of the fallback folder by setting the `RestoreAdditionalProjectSources` to nothing. `<RestoreAdditionalProjectSources/>` Use this with caution as it will cause a lot of packages to be downloaded from NuGet.org which otherwise would be have been restored from the fallback folder.
