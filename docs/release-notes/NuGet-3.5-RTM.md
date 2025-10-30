---
title: NuGet 3.5 RTM Release Notes
description: Release notes for NuGet 3.5 including known issues, bug fixes, added features, and DCRs.
author: JonDouglas
ms.author: jodou
ms.date: 11/11/2016
ms.topic: release-notes
---

# NuGet 3.5 Release Notes

[NuGet 3.5-RC Release Notes](../release-notes/nuget-3.5-RC.md) | [NuGet 4.0 RC Release Notes](../release-notes/nuget-4.0-RC.md)

## Bug Fixes

* Pack doesn't use MSBuild 14.1 on mono - [#3550](https://github.com/NuGet/Home/issues/3550)

* Update tab doesn't select the latest available version to update instead select current installed version - [#3498](https://github.com/NuGet/Home/issues/3498)

* Fix Crash after authenticating a private v2 MyGet feed and clicking "Show x more results" - [#3469](https://github.com/NuGet/Home/issues/3469)

* Log messages seem to be in reverse order for UI - [#3446](https://github.com/NuGet/Home/issues/3446)

* v3.4.4 - Nuget restore throws "The given path's format is not supported" - [#3442](https://github.com/NuGet/Home/issues/3442)

* NuGet cmdLine 3.6 beta does not honor -Prop Configuration = Release - [#3432](https://github.com/NuGet/Home/issues/3432)

* Nuget IKVM slow install on large project - [#3428](https://github.com/NuGet/Home/issues/3428)

* nuget.exe Update -Self keeps on updating itself - [#3395](https://github.com/NuGet/Home/issues/3395)

* 3.5 install/restore from UNC share has performance Regression from 3.4.4 - [#3355](https://github.com/NuGet/Home/issues/3355)

* Error when installing Moq from the Package management UI for a net451 project - [#3349](https://github.com/NuGet/Home/issues/3349)

* Install tab at solution level doesn't show package's version - [#3339](https://github.com/NuGet/Home/issues/3339)

* xproj `project.json` update from Installed tab loses state - [#3303](https://github.com/NuGet/Home/issues/3303)

* NuGet pack on `.csproj` ignores empty files element in `.nuspec` file - [#3257](https://github.com/NuGet/Home/issues/3257)

* Website projects hosted in IIS should not cause restore to fail - [#3235](https://github.com/NuGet/Home/issues/3235)

* Credentials not retrieved from Nuget.Config when v3 endpoint redirects to v2 - [#3179](https://github.com/NuGet/Home/issues/3179)

* NuGet pack fails to resolve assembly when retrieving portable assembly metadata - [#3128](https://github.com/NuGet/Home/issues/3128)

* Nuget can't find `msbuild.exe` on Mono - [#3085](https://github.com/NuGet/Home/issues/3085)

* nuget.exe pack doesn't allow a pre-release tag which begins with numbers - [#1743](https://github.com/NuGet/Home/issues/1743)

* nuget package install fails on VS2015E - [#1298](https://github.com/NuGet/Home/issues/1298)

* allowedVersions filter not working at solution level - [#333](https://github.com/NuGet/Home/issues/333)

* Restore randomly fails with An item with the same key has already been added. - [#2646](https://github.com/NuGet/Home/issues/2646)

* Cannot install Nuget.Common in `.csproj` - [#2635](https://github.com/NuGet/Home/issues/2635)

* When using the UI to search a V2 source, FindPackagesById is called twice for each ID - [#2517](https://github.com/NuGet/Home/issues/2517)

* Packages cannot depend on projects - [#2490](https://github.com/NuGet/Home/issues/2490)

* nuget.exe pack -Exclude is documented but not supported - [#2284](https://github.com/NuGet/Home/issues/2284)

* Issues with error messages when 'contentFiles' section of `.nuspec` is invalid - [#1686](https://github.com/NuGet/Home/issues/1686)

* Push always sends entire package twice with authenticated package sources - [#1501](https://github.com/NuGet/Home/issues/1501)

* No information was given when calling nuget.exe update *.csproj while the project does not have a `packages.config` - [#1496](https://github.com/NuGet/Home/issues/1496)

* `packages.config` restore does not retry on 5xx status codes from V2 sources - [#1217](https://github.com/NuGet/Home/issues/1217)

* Double dot in file src in `.nuspec` doesn't work - [#2947](https://github.com/NuGet/Home/issues/2947)

* CoreCLR restore needs to ignore feeds with encryption - [#2942](https://github.com/NuGet/Home/issues/2942)

* nuget.exe push 403 handling - Incorrectly prompting for credentials - [#2910](https://github.com/NuGet/Home/issues/2910)

* NuGet update through package manager removes properties from the `project.json` - [#2888](https://github.com/NuGet/Home/issues/2888)

* NuGet.PackageManagement.VisualStudio try to load "NuGet.TeamFoundationServer14", but that DLL name changed to "NuGet.TeamFoundationServer" - [#2857](https://github.com/NuGet/Home/issues/2857)

* Package manager UI doesn't show newly updated version - [#2828](https://github.com/NuGet/Home/issues/2828)

* update-package trying to use packageid,version instead of package.version - [#2771](https://github.com/NuGet/Home/issues/2771)

* nuget restore csproj should error if the project isn't using nuget (`packages.config` or `project.json`) - [#2766](https://github.com/NuGet/Home/issues/2766)

* TFS Error "[file]not be found in your workspace, or you do not have permission to access it"  during upgrade or uninstall when solution/project is bound to TFS source control - [#2739](https://github.com/NuGet/Home/issues/2739)

* update package doesn't get dependencies for non-target packages - [#2724](https://github.com/NuGet/Home/issues/2724)

* There is no way to set logs verbosity level for Nuget package manager UI actions - [#2705](https://github.com/NuGet/Home/issues/2705)

* nuget configuration is invalid - VS 2015 VSIX (v3.4.3) - [#2667](https://github.com/NuGet/Home/issues/2667)

* DefaultPushSource in `NuGetDefaults.Config` (`ProgramData\NuGet`) doesn't work - [#2653](https://github.com/NuGet/Home/issues/2653)

* nuget 3.4.3 release -  getting Value cannot be null on package build - [#2648](https://github.com/NuGet/Home/issues/2648)

* restore is not using stored credentials from Nuget.Config for VSTS feeds - [#2647](https://github.com/NuGet/Home/issues/2647)

* [dotnet restore] --configfile is relative to project dir instead of the cmd dir - [#2639](https://github.com/NuGet/Home/issues/2639)

* Excessive allocations in version comparsion code - [#2632](https://github.com/NuGet/Home/issues/2632)

* Multiple instances of nuget.exe trying to install the same package in parallel causes a double write - [#2628](https://github.com/NuGet/Home/issues/2628)

* Dependency information is not cached for multi-project operations - [#2619](https://github.com/NuGet/Home/issues/2619)

* Install and update download packages without checking the packages folder first - [#2618](https://github.com/NuGet/Home/issues/2618)

* If package source list is empty, cannot add package source via UI (NuGet 3.4.x) - [#2617](https://github.com/NuGet/Home/issues/2617)

* Misleading error when attempting to install package that depends on design-time facades - [#2594](https://github.com/NuGet/Home/issues/2594)

* Installing a package from PackageManager console with setting "All" tries only first source - [#2557](https://github.com/NuGet/Home/issues/2557)

* Latest beta not unzipping ModernHttpClient - [#2518](https://github.com/NuGet/Home/issues/2518)

* VS2015 crash at startup with self-built NuGet 3.4.1 - [#2419](https://github.com/NuGet/Home/issues/2419)

* Update command might be a bit more verbose if i ask it to be so... - [#2418](https://github.com/NuGet/Home/issues/2418)

* VSIX built locally should have the same DLLs and files as the CI build. - [#2401](https://github.com/NuGet/Home/issues/2401)

* Fix NuGet downgrade warnings in the build - [#2396](https://github.com/NuGet/Home/issues/2396)

* Failing to authenticate package source (3 times) is blocked forever - [#2362](https://github.com/NuGet/Home/issues/2362)

* Package content is not restored correctly when installing a package from a nuget v3.3+ feed with the argument -NoCache when the package contains `.nupkg` files - [#2354](https://github.com/NuGet/Home/issues/2354)

* Nuget Install with All Package Sources, but package missing from 1 source, fails - [#2322](https://github.com/NuGet/Home/issues/2322)

* [PerfWatson] UIDelay: nuget.packagemanagement.visualstudio.dll!NuGet.PackageManagement.VisualStudio.VSMSBuildNuGetProjectSystem+*lt;&gt;c__DisplayClass_0+&lt;&lt;AddReference&gt;b__&gt;d.MoveNext - [#2285](https://github.com/NuGet/Home/issues/2285)

* Install blocks if a single source fails authorization - [#2034](https://github.com/NuGet/Home/issues/2034)

* `.nuspec` version range should override -IncludeReferencedProjects version - [#1983](https://github.com/NuGet/Home/issues/1983)

* Update-Package super slow - "Attempting to gather dependencies information" - [#1909](https://github.com/NuGet/Home/issues/1909)

* NuGet stealth downgrades package when batch updating its dependencies - [#1903](https://github.com/NuGet/Home/issues/1903)

* nuget.exe update drops the assembly strong name and Private attribute. - [#1778](https://github.com/NuGet/Home/issues/1778)

* Relative file path for "DefaultPushSource" - [#1746](https://github.com/NuGet/Home/issues/1746)

* Improve resolver failure messages - [#1373](https://github.com/NuGet/Home/issues/1373)

* update-package in v3 fails with packages not in the specified source - [#1013](https://github.com/NuGet/Home/issues/1013)

* Using relative paths for package sources is problematic to use - [#865](https://github.com/NuGet/Home/issues/865)

* Missing dependency in NUPKG-file generated from project if indirect dependency already exists with a lower version requirement - [#759](https://github.com/NuGet/Home/issues/759)

* Deleting a project closes corresponding UI window, but, renaming a project does not rename the UI window. Note that PMC listens to project rename and project remove events - [#670](https://github.com/NuGet/Home/issues/670)

* [Willow Web Workload] Creating Razor v3 WSP hangs - [#3241](https://github.com/NuGet/Home/issues/3241)

* Install/restore of a particular package fails with "Package contains multiple nuspec files." - [#3231](https://github.com/NuGet/Home/issues/3231)

* Lowercase IDs & `packages.config` scenarios - [#3209](https://github.com/NuGet/Home/issues/3209)

* [3.5-beta2] Package restore fails to restore "legacy" packages - [#3208](https://github.com/NuGet/Home/issues/3208)

* nuget pack forcefully adds .tt files to content folder no matter what - [#3203](https://github.com/NuGet/Home/issues/3203)

* update-package of ASP.NET web app generates warning related to file: source - [#3194](https://github.com/NuGet/Home/issues/3194)

* nuget pack csproj (with `project.json`) crashes if there are no packOptions and owner in JSON file - [#3180](https://github.com/NuGet/Home/issues/3180)

* nuget pack for `project.json` ignores packOptions tags like summary, authors , owners etc - [#3161](https://github.com/NuGet/Home/issues/3161)

* NullReferenceException via NuGet.Packaging.PhysicalPackageFile.GetStream - [#3160](https://github.com/NuGet/Home/issues/3160)

* NuGet pack ignores dependencies in output `.nuspec` for `project.json` - [#3145](https://github.com/NuGet/Home/issues/3145)

* Updating multiple packages with rollback leaves the project in a broken state - [#3139](https://github.com/NuGet/Home/issues/3139)

* ContentFiles under any are not added for netstandard projects - [#3118](https://github.com/NuGet/Home/issues/3118)

* Cannot package library targeting .Net Standard correctly - [#3108](https://github.com/NuGet/Home/issues/3108)

* File -> New Project -> Class Library (Portable) project fails in VS2015 and Dev15 - [#3094](https://github.com/NuGet/Home/issues/3094)

* nuGet error - 1.0.0-* is not a valid version string - [#3070](https://github.com/NuGet/Home/issues/3070)

* Find-Package fails to display but Install-Package works - [#3068](https://github.com/NuGet/Home/issues/3068)

* Error when "Install-Package jquery.validation" on dev15 - [#3061](https://github.com/NuGet/Home/issues/3061)

* nuget pack of xproj is defaulting to invalid target path - [#3060](https://github.com/NuGet/Home/issues/3060)

* When installed VS 2015 update 3 on a VS that uses NuGet version 3.5.0 error occurs - [#3053](https://github.com/NuGet/Home/issues/3053)

* "Blocked by packages.config" in `project.json` (UWP, a.k.a build integrated) project - [#3046](https://github.com/NuGet/Home/issues/3046)

* update dotnet cli installed by build script to preview2-003121, which is the official preview2 build. - [#3045](https://github.com/NuGet/Home/issues/3045)

* Package manager UI: Doesn't display new version after updating a package - [#3041](https://github.com/NuGet/Home/issues/3041)

* -ApiKey on delete command line is not read/sent in 3.5.0-beta - [#3037](https://github.com/NuGet/Home/issues/3037)

* Incorrect string: A stable release of a package should not have on a prerelease dependency. - [#3030](https://github.com/NuGet/Home/issues/3030)

* OptimizedZipPackage cache leaves empty folders - [#3029](https://github.com/NuGet/Home/issues/3029)

* Creating PCL (net46 and windows 10) project get NullRef exception. - [#3014](https://github.com/NuGet/Home/issues/3014)

* Nuget update should provide informative message when a higher version is restricted by allowedVersions constraint - [#3013](https://github.com/NuGet/Home/issues/3013)

* Nuget v3 restore issues - [#2891](https://github.com/NuGet/Home/issues/2891)

* Credential plugin exited with error -1 / error downloading package when using credential providers with multiple sources - [#2885](https://github.com/NuGet/Home/issues/2885)

* `project.json` nuget restore causes recompilation when nothing changed - [#2817](https://github.com/NuGet/Home/issues/2817)

* Symbols packages should not ever be used in install or update - [#2807](https://github.com/NuGet/Home/issues/2807)

* VS doesn't support environment variables in repositoryPath (nuget.exe does) - [#2763](https://github.com/NuGet/Home/issues/2763)

* Label the unlabeled UIElements in Package Manager UI for accessibility - [#2745](https://github.com/NuGet/Home/issues/2745)

* Portable frameworks with hyphenated profiles are rejected. - [#2734](https://github.com/NuGet/Home/issues/2734)

* NuGet package manager should make it clear that options list in packages detail does not apply to `project.json` - [#2665](https://github.com/NuGet/Home/issues/2665)

* nuget.exe push/delete won't use API Key  - [#2627](https://github.com/NuGet/Home/issues/2627)

* Remove the locked property from the lock file - [#2379](https://github.com/NuGet/Home/issues/2379)

* NuGet 3.3.0 update fails with 'An additional constraint ... defined in packages.config prevents this operation.' - [#1816](https://github.com/NuGet/Home/issues/1816)

* Installing package from a local source that doesn't exist throws a bogus message - [#1674](https://github.com/NuGet/Home/issues/1674)

* "Upgrade available" filter shows upgrades that violate the version constraint - [#1094](https://github.com/NuGet/Home/issues/1094)

* Unable to update native packages - [#1291](https://github.com/NuGet/Home/issues/1291)


## Features

* Support setting CopyLocal to false on references added by NuGet - [#329](https://github.com/NuGet/Home/issues/329)

* nuget.exe support for MSBuild 15 - [#1937](https://github.com/NuGet/Home/issues/1937)

* Pack support for .`csproj` + `project.json` - [#1689](https://github.com/NuGet/Home/issues/1689)

* Disable user action when there are user actions being executed - [#1440](https://github.com/NuGet/Home/issues/1440)

* NuGet should add support for `runtimes/{rid}/nativeassets/{txm}/` - [#2782](https://github.com/NuGet/Home/issues/2782)

* Add framework compatibilities missing in NuGet 2.x (which are already in 3.x) - [#2720](https://github.com/NuGet/Home/issues/2720)

* Support for fallback package folders - [#2899](https://github.com/NuGet/Home/issues/2899)

* Design and implement a notion of package type to support tool packages - [#2476](https://github.com/NuGet/Home/issues/2476)

* Add an API to get the path to the global packages folder - [#2403](https://github.com/NuGet/Home/issues/2403)

* Enable SemVer 2.0.0 in pack - [#3356](https://github.com/NuGet/Home/issues/3356)

## DCRs

* nuget.exe push - timeout parameter doesn't work  - [#2785](https://github.com/NuGet/Home/issues/2785)

* Package Description text should be selectable - [#1769](https://github.com/NuGet/Home/issues/1769)

* Enable nuget.exe to produce `.props` and `.targets` files for `.nuproj` projects [#2711](https://github.com/NuGet/Home/issues/2711)

* Add extensibility API to compare frameworks with imports - [#2633](https://github.com/NuGet/Home/issues/2633)

* Hide dependency options when using `project.json` - [#2486](https://github.com/NuGet/Home/issues/2486)

* Print out nuget.exe version header in detailed output - [#1887](https://github.com/NuGet/Home/issues/1887)

* NuGet needs to let users know that upgrading/installing in a dotnet tfm based PCL could cause issues - [#3138](https://github.com/NuGet/Home/issues/3138)

* Warn bad install/upgrade for project w/ tfm="dotnet" - [#3137](https://github.com/NuGet/Home/issues/3137)

* Fix performance issues with ReShaper and NuGet for Update - [#3044](https://github.com/NuGet/Home/issues/3044)

* Add netcoreapp11 and netstandard17 support - [#2998](https://github.com/NuGet/Home/issues/2998)

* Leverage AssemblyMetadata attribute for `.nuspec` token replacements - [#2851](https://github.com/NuGet/Home/issues/2851)
