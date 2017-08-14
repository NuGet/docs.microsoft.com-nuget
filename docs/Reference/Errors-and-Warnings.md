---
# required metadata

title: NuGet Restore Errors and Warnings Reference (PREVIEW) | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 7/21/2017
ms.topic: article
ms.prod: nuget
#ms.service:
ms.technology: null
ms.assetid: b76b8a00-7155-4163-b533-894086d2ef78

# optional metadata

description: Complete reference for warnings and errors issued from NuGet during package restore
keywords: NuGet errors, NuGet warnings, diagnostics
#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer:
- anandr
- karann
- unnir
#ms.suite:
#ms.tgt_pltfrm:
#ms.custom:

---

# Errors and warnings (PREVIEW)

In NuGet 4.3.0 (currently in preview), errors and warnings are numbered as described in this topic and provide detailed information to help you address the issues involved. 

The errors and warnings listed here are available only with [PackageReference-based](../Consume-Packages/Package-References-in-Project-Files.md) projects and NuGet 4.3.0 (currently in preview). NuGet also honors MSBuild properties to suppress warnings or elevate them to errors. For more information, see [How to: Suppress Compiler Warnings](https://docs.microsoft.com/visualstudio/ide/how-to-suppress-compiler-warnings) in the Visual Studio documentation.

**Errors**

| Group | Error Numbers |
| --- | --- |
| [Invalid input errors](#invalid-input-errors) | [NU1001](#nu1001), [NU1002](#nu1002), [NU1003](#nu1003) |
| [Missing package and project errors](#missing-package-and-project-errors) | [NU1100](#nu1100), [NU1101](#nu1101), [NU1102](#nu1102), [NU1103](#nu1103), [NU1104](#nu1104), [NU1105](#nu1105), [NU1106](#nu1106) |
| [Compatibility errors](#compatibility-errors) | [NU1201](#nu1201), [NU1202](#nu1202), [NU1203](#nu1203), [NU1401](#nu1401) |

**Warnings**

| Group | Warning numbers |
| --- | --- |
| [Invalid input warnings](#invalid-input-warnings) | [NU1501](#nu1501), [NU1502](#nu1502), [NU1503](#nu1503) |
| [Unexpected package version warnings](#unexpected-package-version-warnings) | [NU1601](#nu1601), [NU1602](#nu1602), [NU1603](#nu1603), [NU1604](#nu1604), [NU1605](#nu1605) |
| [Resolver conflict warnings](#resolver-conflict-warnings) | [NU1606](#nu1606), [NU1607](#nu1607) |
| [Package fallback warnings](#package-fallback-warnings) | [NU1701](#nu1701) |
| [Feed warnings](#feed-warnings) | [NU1801](#nu1801) |
| [NuGet internal errors and warnings](#nuget-internal-errors-and-warnings) | [NU1000](#nu1000), [NU1500](#nu1500) |

## Invalid input errors

[NU1001](#nu1001) | [NU1002](#nu1002) | [NU1003](#nu1003)

### NU1001

| | |
| --- | --- |
| **Issue** | The project doesn't contain one or more frameworks. |
| **Common causes** | The project doesn't contain a `TargetFramework` or `TargetFrameworks` property. |
| **Example message** | *The project projA does not specify any target frameworks in c:\tmp\projA.csproj* |


### NU1002

| | |
| --- | --- |
| **Issue** | Invalid combination of inputs along with a CLEAR keyword. |
| **Common causes** | CLEAR may not be combined with other inputs. |
| **Example message** | *'CLEAR' cannot be used in conjunction with other values* |

### NU1003

| | |
| --- | --- |
| **Issue** | `PackageTargetFallback` and `AssetTargetFallback` provide different behavior for selecting assets and cannot be used together. |
| **Common causes** | Both `PackageTargetFallback` and `AssetTargetFallback` exist in the project. |
| **Example message** | *PackageTargetFallback and AssetTargetFallback cannot be used together. Remove PackageTargetFallback(deprecated) references from the project environment.* |


## Missing package and project errors

[NU1100](#nu1100) | [NU1101](#nu1101) | [NU1102](#nu1102) | [NU1103](#nu1103) | [NU1104](#nu1104) | [NU1105](#nu1105) | [NU1106](#nu1106)

### NU1100

| | |
| --- | --- |
| **Issue** | A dependency group not be resolved. This is a generic issue for types that are not packages or projects. |
| **Common causes** | The project contains a dependency on an item that doesn't exist. |
| **Example message** | *Unable to resolve System.Missing for net45* |


### NU1101

| | |
| --- | --- |
| **Issue** | The package cannot be found on any sources. |
| **Common causes** | The correct package source is missing or the package identifier is incorrect. |
| **Example message** | *Unable to find package System.Missing. No packages exist with this id in source(s): dotnet-core, dotnet-roslyn, NuGet.org* |
### NU1102

| | |
| --- | --- |
| **Issue** | The package identifier is found but a version within the specified dependency range cannot be found on any of the sources. |
| **Common causes** | The correct package source is missing or the dependency range is incorrect. The range might be specified by a package and not the user. The user may need to switch to an available version if this package is referenced by the project directly. |
| **Example message** | *Unable to find package NuGet.Versioning with version (>= 9.0.1)<br/>  - Found 30 version(s) in NuGet.org [ Nearest version: 4.0.0 ]<br/>  - Found 10 version(s) in dotnet-buildtools [ Nearest version: 4.0.0-rc-2129 ]<br/>  - Found 9 version(s) in NuGetVolatile [ Nearest version: 3.0.0-beta-00032 ]<br/>  - Found 0 version(s) in dotnet-core<br/>  - Found 0 version(s) in dotnet-roslyn* |

### NU1103

| | |
| --- | --- |
| **Issue** | No stable versions were found in the dependency range. Pre-release versions were found but are not allowed. |
| **Common causes** | The project specified a stable version for the dependency range. Users need to change the version range to include pre-release versions. |
| **Example message** | *Unable to find a stable package NuGet.Versioning with version (>= 3.0.0)<br/>  - Found 10 version(s) in dotnet-buildtools [ Nearest version: 4.0.0-rc-2129 ]<br/>  - Found 9 version(s) in NuGetVolatile [ Nearest version: 3.0.0-beta-00032 ]<br/>  - Found 0 version(s) in dotnet-core<br/>  - Found 0 version(s) in dotnet-roslyn* |


### NU1104

| | |
| --- | --- |
| **Issue** | A ProjectReference points to a file that doesn't exist. |
| **Common causes** | The project file is missing from disk or the reference is incorrect. |
| **Example message** | *Project reference does not exist 'c:\a.csproj'. Check that the project reference is valid and that the project file exists.* |

### NU1105

| | |
| --- | --- |
| **Issue** | The project file exists but no restore information was provided for it. |
| **Common causes** | In Visual Studio this could mean that the project is unloaded. From the command line this could mean that the file is corrupt or that it doesn't contain the custom after imports target needed for restore to read the project. |
| **Example message** | *Unable to read project information for 'c:\a.csproj'. The project file may be invalid or missing targets required for restore.* |

### NU1106

| | |
| --- | --- |
| **Issue** | Dependency constraints cannot be resolved. |
| **Common causes** | Packages contain dependency on exact versions of a package instead of open-ended ranges. |
| **Example message** | *Unable to satisfy conflicting requests for {id}: {conflict path} Framework: {target graph}* |

## Compatibility errors

[NU1201](#nu1201) | [NU1202](#nu1202) | [NU1203](#nu1203) | [NU1401](#nu1401)

### NU1201

| | |
| --- | --- |
| **Issue** | A dependency project doesn't contain a framework compatible with the current project. |
| **Common causes** | The project's target framework is a higher version than the consuming project. |
| **Example message** | *Project ServerWeb is not compatible with netstandard1.3 (.NETStandard,Version=v1.3). Project ServerWeb supports:<br/>  - netstandard1.6 (.NETStandard,Version=v1.6)<br/>  - netcoreapp1.0 (.NETCoreApp,Version=v1.0)* |


### NU1202

| | |
| --- | --- |
| **Issue** | A dependency package doesn't contain any assets compatible with the project. |
| **Common causes** | The package doesn't support the project's target framework. |
| **Example message** | *Package System.ComponentModel.EventBasedAsync 4.0.11 is not compatible with netstandard1.3 (.NETStandard,Version=v1.3). Package System.ComponentModel.EventBasedAsync 4.0.11 supports:<br/>  - monoandroid10 (MonoAndroid,Version=v1.0)<br/>  - monotouch10 (MonoTouch,Version=v1.0)<br/>  - net45 (.NETFramework,Version=v4.5)<br/>  - netcore50 (.NETCore,Version=v5.0)<br/>  - netstandard1.0 (.NETStandard,Version=v1.0)<br/>  - netstandard1.3 (.NETStandard,Version=v1.3)<br/>  - portable-net45+win8+wp8+wpa81 (.NETPortable,Version=v0.0,Profile=Profile259)<br/>  - win8 (Windows,Version=v8.0)<br/>  - wp8 (WindowsPhone,Version=v8.0)<br/>  - wpa81 (WindowsPhoneApp,Version=v8.1)<br/>  - xamarinios10 (Xamarin.iOS,Version=v1.0)<br/>  - xamarinmac20 (Xamarin.Mac,Version=v2.0)<br/>  - xamarintvos10 (Xamarin.TVOS,Version=v1.0)<br/>  - xamarinwatchos10 (Xamarin.WatchOS,Version=v1.0)*|


### NU1203

| | |
| --- | --- |
| **Issue** | The package doesn't support the project's `RuntimeIdentifier`. |
| **Common causes** | The package doesn't support the current `RuntimeIdentifier`. Change the `RuntimeIdentifier` values used in the project if needed. |
| **Example message** | *System.Example 1.0.0 provides a compile-time reference assembly for a.dll on net461, but there is no compatible run-time assembly.* |

### NU1401

| | |
| --- | --- |
| **Issue** | The package requires features or frameworks not currently supported by the installed version of NuGet. |
| **Common causes** | Upgrade NuGet to fix the issue. |
| **Example message** | *The 'NuGet.Versioning' package requires NuGet client version '5.0.0' or above, but the current NuGet version is '4.3.0'. To upgrade NuGet, please go to http://docs.nuget.org/consume/installing-nuget.* |

## Invalid input warnings

[NU1501](#nu1501) | [NU1502](#nu1502) | [NU1503](#nu1503)

### NU1501

| | |
| --- | --- |
| **Issue** | The project restore is attempting to operate on was not found. |
| **Common causes** | The project is missing. |
| **Example message** | *The folder 'c:\projects\a' does not contain a project to restore.* |

### NU1502

| | |
| --- | --- |
| **Issue** | `RuntimeSupports` contains an invalid profile. |
| **Common causes** | The supports profile was not found in a `runtime.json` file from the current dependency packages. |
| **Example message** | *Unknown Compatibility Profile: aaa* |

### NU1503

| | |
| --- | --- |
| **Issue** | A dependency project doesn't import NuGet's restore targets. This is similar to NU1105 but here the project is skipped and ignored instead of causing all of restore to fail. In complex solutions there are often other types of projects that may not support restore. |
| **Common causes** | This can happen for projects that do not import common props/targets which automatically import restore. If the project doesn't need to be restored this can be ignored. |
| **Example message** | *Skipping restore for project 'c:\a.csproj'. The project file may be invalid or missing targets required for restore.* |

## Unexpected package version warnings

[NU1601](#nu1601) | [NU1602](#nu1602) | [NU1603](#nu1603) | [NU1604](#nu1604) | [NU1605](#nu1605)

### NU1601

| | |
| --- | --- |
| **Issue** | A direct project dependency was bumped to a higher version than the project specified. |
| **Common causes** | Another dependency package required a higher version and bumped the package up. |
| **Example message** | *Dependency specified was NuGet.Versioning (>= 3.5.0) but ended up with NuGet.Versioning 4.0.0.* |

### NU1602

| | |
| --- | --- |
| **Issue** | A package dependency is missing a lower bound. This doesn't allow restore to find the *best match*. Each restore will float downwards trying to find a lower version that can be used. This means that restore goes online to check all sources each time instead of using the packages that already exist in the user package folder. |
| **Common causes** | This is usually a package authoring error. |
| **Example message** | *NuGet.Packaging 4.0.0 does not provide an inclusive lower bound for dependency NuGet.Versioning (> 3.5.0). An approximate best match of 3.6.0 was resolved.* |

### NU1603

| | |
| --- | --- |
| **Issue** | A package dependency specified a version that could not be found. A higher version was used instead, which differs from what the package was authored against.<br/><br/>This means that restore did not find the *best match*. Each restore will float downwards trying to find a lower version that can be used. This means that restore goes online to check all sources each time instead of using the packages that already exist in the user package folder. |
| **Common causes** | The package sources do not contain the expected lower bound version. If the package expected has not been released then this may be a package authoring error. |
| **Example message** | NuGet.Packaging 4.0.0 depends on NuGet.Versioning (>= 4.0.0) but 4.0.0 was not found. An approximate best match of 5.0.0 was resolved. |

### NU1604

| | |
| --- | --- |
| **Issue** | A project dependency doesn't define a lower bound.<br/><br/>This means that restore did not find the *best match*. Each restore will float downwards trying to find a lower version that can be used. This means that restore goes online to check all sources each time instead of using the packages that already exist in the user package folder. |
| **Common causes** | The project's *PackageReference* *Version* attribute should be updated to include a lower bound. |
| **Example message** | *Project dependency NuGet.Versioning (<= 9.0.0) doe not contain an inclusive lower bound. Include a lower bound in the dependency version to ensure consistent restore results.* |

### NU1605

| | |
| --- | --- |
| **Issue** | A dependency package specified a version constraint on a higher version of a package than restore ultimately resolved. |
| **Common causes** | Nearest wins when resolving packages. A nearer package in the graph may have overridden a distant package. |
| **Example message** | *Detected package downgrade: NuGet.Versioning from 4.0.0 to 3.5.0. Reference the package directly from the project to select a different version.<br/>  NuGet.Packaging 3.5.0 -> NuGet.Versioning 3.5.0<br/>  NuGet.Commands 4.0.0 -> NuGet.Configuration 4.0.0 -> NuGet.Versioning 4.0.0* |

## Resolver conflict warnings

[NU1606](#nu1606) | [NU1607](#nu1607)

### NU1606

| | |
| --- | --- |
| **Issue** | A circular dependency was detected. |
| **Common causes** | A package is authored incorrectly. |
| **Example message** | *Cycle detected: A -> B -> A* |

### NU1607

| | |
| --- | --- |
| **Issue** | Unable to resolve dependency constraints between packages. |
| **Common causes** | Packages with dependency constraints on exact versions do not allow other packages to increase the version if needed. |
| **Example message** | *Version conflict detected for NuGet.Versioning. Reference the package directly from the project to resolve this issue.<br/>  NuGet.Packaging 3.5.0 -> NuGet.Versioning (= 3.5.0)<br/>  NuGet.Configuration 4.0.0 -> NuGet.Versioning (= 4.0.0)* |

## Package fallback warnings

[NU1701](#nu1701)

### NU1701

| | |
| --- | --- |
| **Issue** | *PackageTargetFallback* was used to select assets from a package. This is a warning to let the user know that the assets may not be 100% compatible. |
| **Common causes** | The package doesn't support the project framework. |
| **Example message** | *Package 'NuGet.Versioning' was restored using 'portable-net45+win8' instead the project target framework 'netstandard1.5'. This package may not be fully compatible with your project.* |

## Feed warnings

[NU1801](#nu1801)

### NU1801

| | |
| --- | --- |
| **Issue** | An error occurred when reading the feed when `IgnoreFailedSources` is set to true, converting it to a non-fatal warning. This could contain any message and is generic. |
| **Common causes** | The source is invalid. |
| **Example message** | n/a |


## NuGet internal errors and warnings

[NU1000](#nu1000) | [NU1500](#nu1500)

### NU1000

| | |
| --- | --- |
| **Issue** | A non specific internal error from NuGet. |
| **Common causes** | Check the logs for more information |

### NU1500

| | |
| --- | --- |
| **Issue** | A non specific internal warning from NuGet. |
| **Common causes** | Check the logs for more information |
