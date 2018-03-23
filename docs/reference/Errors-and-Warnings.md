---
title: NuGet Errors and Warnings Reference | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 03/06/2018
ms.topic: reference
ms.prod: nuget
ms.technology: null
description: Complete reference for warnings and errors issued from NuGet during various NuGet operations.
keywords: NuGet errors, NuGet warnings, diagnostics
ms.reviewer:
- anangaur
- karann-msft
- unniravindranathan
ms.workload: 
 - "dotnet"
 - "aspnet"

f1_keywords:
  - "NU1000"
  - "NU1001"
  - "NU1002"
  - "NU1003"
  - "NU1100"
  - "NU1101"
  - "NU1102"
  - "NU1103"
  - "NU1104"
  - "NU1105"
  - "NU1106"
  - "NU1107"
  - "NU1108"
  - "NU1201"
  - "NU1202"
  - "NU1203"
  - "NU1401"
  - "NU1500"
  - "NU1501"
  - "NU1502"
  - "NU1503"
  - "NU1601"
  - "NU1602"
  - "NU1603"
  - "NU1604"
  - "NU1605"
  - "NU1608"
  - "NU1701"
  - "NU1801"
  - "NU3000"
  - "NU3001"
  - "NU3002"
  - "NU3004"
  - "NU3008"
  - "NU3018"
  - "NU3028"
---

# Errors and warnings

In NuGet 4.3.0+, errors and warnings are numbered as described in this topic and provide detailed information to help you address the issues involved.

The errors and warnings listed here are available only with [PackageReference-based](../consume-packages/package-references-in-project-files.md) projects and NuGet 4.3.0+. NuGet also honors MSBuild properties to suppress warnings or elevate them to errors. For more information, see [How to: Suppress Compiler Warnings](/visualstudio/ide/how-to-suppress-compiler-warnings) in the Visual Studio documentation.

**Errors**

| Group | Error Numbers |
| --- | --- |
| [Invalid input errors](#invalid-input-errors) | [NU1001](#nu1001), [NU1002](#nu1002), [NU1003](#nu1003) |
| [Missing package and project errors](#missing-package-and-project-errors) | [NU1100](#nu1100), [NU1101](#nu1101), [NU1102](#nu1102), [NU1103](#nu1103), [NU1104](#nu1104), [NU1105](#nu1105), [NU1106](#nu1106), [NU1107](#nu1107) (previously NU1607), [NU1108](#nu1108) (previously NU1606) |
| [Compatibility errors](#compatibility-errors) | [NU1201](#nu1201), [NU1202](#nu1202), [NU1203](#nu1203), [NU1401](#nu1401) |

**Warnings**

| Group | Warning numbers |
| --- | --- |
| [Invalid input warnings](#invalid-input-warnings) | [NU1501](#nu1501), [NU1502](#nu1502), [NU1503](#nu1503) |
| [Unexpected package version warnings](#unexpected-package-version-warnings) | [NU1601](#nu1601), [NU1602](#nu1602), [NU1603](#nu1603), [NU1604](#nu1604), [NU1605](#nu1605) |
| [Resolver conflict warnings](#resolver-conflict-warnings) | [NU1608](#nu1608) |
| [Package fallback warnings](#package-fallback-warnings) | [NU1701](#nu1701) |
| [Feed warnings](#feed-warnings) | [NU1801](#nu1801) |
| [NuGet internal errors and warnings](#nuget-internal-errors-and-warnings) | [NU1000](#nu1000), [NU1500](#nu1500) |
| [Signed packages (creation and verification)](#signed-packages-creation-and-verification)| [NU3000](#nu3000), [NU3001](#nu3001), [NU3002](#nu3002), [NU3004](#nu3004), [NU3008](#nu3008), [NU3018](#nu3018), [NU3028](#nu3028) |

## Invalid input errors

[NU1001](#nu1001) | [NU1002](#nu1002) | [NU1003](#nu1003)

### NU1001

| | |
| --- | --- |
| **Issue** | The project doesn't contain one or more frameworks. |
| **Example message** | *The project projA does not specify any target frameworks in c:\tmp\projA.csproj* |
| **Solution** | Add a `TargetFramework` or `TargetFrameworks` property to the specified project file. |

### NU1002

| | |
| --- | --- |
| **Issue** | Invalid combination of inputs along with a CLEAR keyword. |
| **Example message** | *'CLEAR' cannot be used in conjunction with other values* |
| **Solution** | Use CLEAR by itself and omit all other inputs. |

### NU1003

| | |
| --- | --- |
| **Issue** | `PackageTargetFallback` and `AssetTargetFallback` provide different behavior for selecting assets and cannot be used together. |
| **Example message** | *PackageTargetFallback and AssetTargetFallback cannot be used together. Remove PackageTargetFallback(deprecated) references from the project environment.* |
| **Solution** | Remove the deprecated `PackageTargetFallback` element from the project. |

## Missing package and project errors

[NU1100](#nu1100) | [NU1101](#nu1101) | [NU1102](#nu1102) | [NU1103](#nu1103) | [NU1104](#nu1104) | [NU1105](#nu1105) | [NU1106](#nu1106)

### NU1100

| | |
| --- | --- |
| **Issue** | A dependency group not be resolved. This is a generic issue for types that are not packages or projects. |
| **Example message** | *Unable to resolve System.Missing for net45* |
| **Solution** | Open the project file and examine the list of its dependencies. Check that each dependency exists on the package sources you're using, and that the package supports the project's target framework. |

### NU1101

| | |
| --- | --- |
| **Issue** | The package cannot be found on any sources. |
| **Example message** | *Unable to find package System.Missing. No packages exist with this id in source(s): dotnet-core, dotnet-roslyn, nuget.org* |
| **Solution** | Examine the project's dependencies in Visual Studio to be sure you're using the correct package identifier and version number. Also check that the [NuGet configuration](../consume-packages/Configuring-NuGet-Behavior.md) identifies the package sources your expect to be using. If you use packages that have [Semantic Versioning 2.0.0](https://docs.microsoft.com/en-us/nuget/reference/package-versioning#semantic-versioning-200), please make sure that you are using the [V3 feed](https://api.nuget.org/v3/index.json) in the [NuGet configuration](../consume-packages/Configuring-NuGet-Behavior.md). |

### NU1102

| | |
| --- | --- |
| **Issue** | The package identifier is found but a version within the specified dependency range cannot be found on any of the sources. The range might be specified by a package and not the user. |
| **Example message** | *Unable to find package NuGet.Versioning with version (>= 9.0.1)<br/>  - Found 30 version(s) in nuget.org [ Nearest version: 4.0.0 ]<br/>  - Found 10 version(s) in dotnet-buildtools [ Nearest version: 4.0.0-rc-2129 ]<br/>  - Found 9 version(s) in NuGetVolatile [ Nearest version: 3.0.0-beta-00032 ]<br/>  - Found 0 version(s) in dotnet-core<br/>  - Found 0 version(s) in dotnet-roslyn* |
| **Solution** | Edit the project file or `packages.config` to correct the package version. Also check that the [NuGet configuration](../consume-packages/Configuring-NuGet-Behavior.md) identifies the package sources your expect to be using. You may need to change the requeted version if this package is referenced by the project directly. |

### NU1103

| | |
| --- | --- |
| **Issue** | The project specified a stable version for the dependency range, but no stable versions were found in that range. Pre-release versions were found but are not allowed. |
| **Example message** | *Unable to find a stable package NuGet.Versioning with version (>= 3.0.0)<br/>  - Found 10 version(s) in dotnet-buildtools [ Nearest version: 4.0.0-rc-2129 ]<br/>  - Found 9 version(s) in NuGetVolatile [ Nearest version: 3.0.0-beta-00032 ]<br/>  - Found 0 version(s) in dotnet-core<br/>  - Found 0 version(s) in dotnet-roslyn* |
| **Solution** |  Edit the version range in the project file or `packages.config` to include pre-release versions. See [Package versioning](../reference/Package-Versioning.md). |

### NU1104

| | |
| --- | --- |
| **Issue** | A ProjectReference points to a file that doesn't exist. |
| **Example message** | *Project reference does not exist 'c:\a.csproj'. Check that the project reference is valid and that the project file exists.* |
| **Solution** | Edit the project file to either correct the path to the referenced project or to remove the reference altogether if it's no longer needed. |

### NU1105

| | |
| --- | --- |
| **Issue** | The project file exists but no restore information was provided for it. |
| **Example message** | *Unable to read project information for 'c:\a.csproj'. The project file may be invalid or missing targets required for restore.* |
| **Solution** | In Visual Studio, the error could mean that the project is unloaded, in which case reload the project. From the command line this could mean that the file is corrupt or that it doesn't contain the custom "after imports" target needed for restore to read the project. Check that the project file is valid and contains an "after imports" target. |

### NU1106

| | |
| --- | --- |
| **Issue** | Dependency constraints cannot be resolved. |
| **Example message** | *Unable to satisfy conflicting requests for {id}: {conflict path} Framework: {target graph}* 
| **Solution** | Edit the project file or `packages.config` to specify more open-ended ranges for the dependency rather than an exact version. |
|

<a name="nu1107"></a>

### NU1107 (Previously NU1607)

| | |
| --- | --- |
| **Issue** | Unable to resolve dependency constraints between packages. |
| **Example message** | *Version conflict detected for NuGet.Versioning. Reference the package directly from the project to resolve this issue.<br/>  NuGet.Packaging 3.5.0 -> NuGet.Versioning (= 3.5.0)<br/>  NuGet.Configuration 4.0.0 -> NuGet.Versioning (= 4.0.0)* |
| **Solution** | Packages with dependency constraints on exact versions do not allow other packages to increase the version if needed. Add a reference to the project directly (in the project file or `packages.config`) with the exact version required. |

<a name="nu1108"></a>

### NU1108 (Previously NU1606)

| | |
| --- | --- |
| **Issue** | A circular dependency was detected. |
| **Example message** | *Cycle detected: A -> B -> A* |
| **Solution** | The package is authored incorrectly; contact the package owner to correct the bug. |

## Compatibility errors

[NU1201](#nu1201) | [NU1202](#nu1202) | [NU1203](#nu1203) | [NU1401](#nu1401)

### NU1201

| | |
| --- | --- |
| **Issue** | A dependency project doesn't contain a framework compatible with the current project. Typically, the project's target framework is a higher version than the consuming project. |
| **Example message** | *Project ServerWeb is not compatible with netstandard1.3 (.NETStandard,Version=v1.3). Project ServerWeb supports:<br/>  - netstandard1.6 (.NETStandard,Version=v1.6)<br/>  - netcoreapp1.0 (.NETCoreApp,Version=v1.0)* |
| **Solution** | Change the project's target framework to an equal or lower version than the consuming project. |

### NU1202

| | |
| --- | --- |
| **Issue** | A dependency package doesn't contain any assets compatible with the project. |
| **Example message** | *Package System.ComponentModel.EventBasedAsync 4.0.11 is not compatible with netstandard1.3 (.NETStandard,Version=v1.3). Package System.ComponentModel.EventBasedAsync 4.0.11 supports:<br/>  - monoandroid10 (MonoAndroid,Version=v1.0)<br/>  - monotouch10 (MonoTouch,Version=v1.0)<br/>  - net45 (.NETFramework,Version=v4.5)<br/>  - netcore50 (.NETCore,Version=v5.0)<br/>  - netstandard1.0 (.NETStandard,Version=v1.0)<br/>  - portable-net45+win8+wp8+wpa81 (.NETPortable,Version=v0.0,Profile=Profile259)<br/>  - win8 (Windows,Version=v8.0)<br/>  - wp8 (WindowsPhone,Version=v8.0)<br/>  - wpa81 (WindowsPhoneApp,Version=v8.1)<br/>  - xamarinios10 (Xamarin.iOS,Version=v1.0)<br/>  - xamarinmac20 (Xamarin.Mac,Version=v2.0)<br/>  - xamarintvos10 (Xamarin.TVOS,Version=v1.0)<br/>  - xamarinwatchos10 (Xamarin.WatchOS,Version=v1.0)*|
| **Solution** | Change the project's target framework to one that the package supports. |

### NU1203

| | |
| --- | --- |
| **Issue** | The package doesn't support the project's `RuntimeIdentifier`. |
| **Example message** | *System.Example 1.0.0 provides a compile-time reference assembly for a.dll on net461, but there is no compatible run-time assembly.* |
| **Solution** | Change the `RuntimeIdentifier` values used in the project as needed. |

### NU1401

| | |
| --- | --- |
| **Issue** | The package requires features or frameworks not currently supported by the installed version of NuGet. |
| **Example message** | *The 'NuGet.Versioning' package requires NuGet client version '5.0.0' or above, but the current NuGet version is '4.3.0'.* |
| **Solution** | Install a newer version of NuGet. See [Installing NuGet client tools](../install-nuget-client-tools.md). |

## Invalid input warnings

[NU1501](#nu1501) | [NU1502](#nu1502) | [NU1503](#nu1503)

### NU1501

| | |
| --- | --- |
| **Issue** | The project restore is attempting to operate on was not found. |
| **Example message** | *The folder 'c:\projects\a' does not contain a project to restore.* |
| **Solution** | Run nuget restore in a folder that contains a project. |

### NU1502

| | |
| --- | --- |
| **Issue** | `RuntimeSupports` contains an invalid profile. Typically, the supports profile was not found in a `runtime.json` file from the current dependency packages.|
| **Example message** | *Unknown Compatibility Profile: aaa* |
| **Solution** | Check the `RuntimeSupports` value in your project. |

### NU1503

| | |
| --- | --- |
| **Issue** | A dependency project doesn't import NuGet's restore targets. This is similar to NU1105 but here the project is skipped and ignored instead of causing all of restore to fail. In complex solutions there are often other types of projects that may not support restore. |
| **Example message** | *Skipping restore for project 'c:\a.csproj'. The project file may be invalid or missing targets required for restore.* |
| **Solution** | This can happen for projects that do not import common props/targets which automatically import restore. If the project doesn't need to be restored this can be ignored. Otherwise, edit the affected project to add targets for restore. |

## Unexpected package version warnings

[NU1601](#nu1601) | [NU1602](#nu1602) | [NU1603](#nu1603) | [NU1604](#nu1604) | [NU1605](#nu1605)

### NU1601

| | |
| --- | --- |
| **Issue** | A direct project dependency was bumped to a higher version than the project specified. |
| **Example message** | *Dependency specified was NuGet.Versioning (>= 3.5.0) but ended up with NuGet.Versioning 4.0.0.* |
| **Solution** | Update the dependency in the project to an appropriate version. |

### NU1602

| | |
| --- | --- |
| **Issue** | A package dependency is missing a lower bound. This doesn't allow restore to find the *best match*. Each restore will float downwards trying to find a lower version that can be used. This means that restore goes online to check all sources each time instead of using the packages that already exist in the user package folder. |
| **Example message** | *NuGet.Packaging 4.0.0 does not provide an inclusive lower bound for dependency NuGet.Versioning (> 3.5.0). An approximate best match of 3.6.0 was resolved.* |
| **Solution** | This is usually a package authoring error. Contact the package author to resolve the issue. |

### NU1603

| | |
| --- | --- |
| **Issue** | A package dependency specified a version that could not be found. Typically, the package sources do not contain the expected lower bound version. A higher version was used instead, which differs from what the package was authored against.<br/><br/>This means that restore did not find the *best match*. Each restore will float downwards trying to find a lower version that can be used. This means that restore goes online to check all sources each time instead of using the packages that already exist in the user package folder. |
| **Example message** | NuGet.Packaging 4.0.0 depends on NuGet.Versioning (>= 4.0.0) but 4.0.0 was not found. An approximate best match of 5.0.0 was resolved. |
| **Solution** | If the package expected has not been released then this may be a package authoring error. Contact the package author to resolve the issue. If the package has been released, then check that it's available on the package sources you're using. If using a private source, you may need to update the package on that feed. |

### NU1604

| | |
| --- | --- |
| **Issue** | A project dependency doesn't define a lower bound.<br/><br/>This means that restore did not find the *best match*. Each restore will float downwards trying to find a lower version that can be used. This means that restore goes online to check all sources each time instead of using the packages that already exist in the user package folder. |
| **Example message** | *Project dependency NuGet.Versioning (<= 9.0.0) doe not contain an inclusive lower bound. Include a lower bound in the dependency version to ensure consistent restore results.* |
| **Solution** | Update the project's `PackageReference` `Version` attribute to include a lower bound. |

### NU1605

| | |
| --- | --- |
| **Issue** | A dependency package specified a version constraint on a higher version of a package than restore ultimately resolved. That is, because of the "nearest wins" rule when resolving packages, a nearer package in the graph may have overridden a distant package. |
| **Example message** | *Detected package downgrade: NuGet.Versioning from 4.0.0 to 3.5.0. Reference the package directly from the project to select a different version.<br/>  NuGet.Packaging 3.5.0 -> NuGet.Versioning 3.5.0<br/>  NuGet.Commands 4.0.0 -> NuGet.Configuration 4.0.0 -> NuGet.Versioning 4.0.0* |
| **Solution** | Add a direct reference to the project for the higher version of the package that you want to use. |

## Resolver conflict warnings

### NU1608

| | |
| --- | --- |
| **Issue** | A resolved package is higher than a dependency constraint allows. This means that a package referenced directly by a project overrides dependency constraints from other packages.|
| **Example message** | *Detected package version outside of dependency constraint: x 1.0.0 requires y (= 1.0.0) but version y 2.0.0 was resolved.* |
| **Solution** | In some cases this is intentional and the warning can be suppressed. Otherwise, change the project's reference to the package to widen its version constraints. |

## Package fallback warnings

### NU1701

| | |
| --- | --- |
| **Issue** | `PackageTargetFallback` / `AssetTargetFallback` was used to select assets from a package. The warning let users know that the assets may not be 100% compatible. |
| **Example message** | *Package 'NuGet.Versioning' was restored using 'portable-net45+win8' instead the project target framework 'netstandard1.5'. This package may not be fully compatible with your project.* |
| **Solution** | Change the project's target framework to one that the package supports. |

## Feed warnings

### NU1801

| | |
| --- | --- |
| **Issue** | An error occurred when reading the feed when `IgnoreFailedSources` is set to true, converting it to a non-fatal warning. This could contain any message and is generic. |
| **Example message** | n/a |
| **Solution** | Edit your configuration to specify valid sources. |

## NuGet internal errors and warnings

[NU1000](#nu1000) | [NU1500](#nu1500)

### NU1000

| | |
| --- | --- |
| **Issue** | A non specific internal error from NuGet. |
| **Solution** | Check the logs for more information |

### NU1500

| | |
| --- | --- |
| **Issue** | A non specific internal warning from NuGet. |
| **Solution** | Check the logs for more information |

## Signed packages (creation and verification)

*NuGet 4.6.0+*

[NU3000](#nu3000) | [NU3001](#nu3001) | [NU3002](#nu3002) | [NU3004](#nu3004) | [NU3008](#nu3008) | [NU3018](#nu3018) | [NU3028](#nu3028)

### NU3000

| | |
| --- | --- |
| **Issue** | A non-specific error related to package signing and signed package verification. |
| **Solution** | Check the logs for more information. |

### NU3001

| | |
| --- | --- |
| **Issue** | Invalid arguments to either the [sign command](../tools/cli-ref-sign.md) or the [verify command](../tools/cli-ref-verify.md). |
| **Solution** | Check and correct the arguments provided. |

### NU3002

| | |
| --- | --- |
| **Issue** | The `-Timestamper` option was not specified with the [nuget sign command](../tools/cli-ref-sign.md). |
| **Example message** | *The '-Timestamper' option was not provided. The signed package will not be timestamped.* |
| **Solution** | Specify the `-Timestamper` option with `nuget sign`. |

### NU3004

| | |
| --- | --- |
| **Issue** | An unsigned package was provided to the [nuget verify command](../tools/cli-ref-verify.md). |
| **Solution** | Run `nuget verify` with a signed package. See [Sign a package](../create-packages/Sign-a-Package.md). |

### NU3008

| | |
| --- | --- |
| **Issue** | The package integrity check failed, meaning that a signed package was tampered with since being signed. |
| **Solution** | Scan your computer with anti-virus software. Then remove the package from the computer, reinstall it, and try the operation again. If the problem persists, contact the owner of the package source and the package owner. |

### NU3018

| | |
| --- | --- |
| **Issue** | Certificate chain building failed for the primary signature. The primary signing certificate is untrusted, revoked, or revocation information for the certificate is unavailable. |
| **Example message** | *WARNING: NU3018: The revocation function was unable to check revocation for the certificate.* |
| **Solution** | Use a trusted and valid certificate. Check internet connectivity. |

### NU3028

| | |
| --- | --- |
| **Issue** | Certificate chain building failed for the timestamp signature. The timestamp signing certificate is untrusted, revoked, or revocation information for the certificate is unavailable. |
| **Example message** | *WARNING: NU3028: The revocation function was unable to check revocation for the certificate.* |
| **Solution** | Use a trusted and valid certificate. Check internet connectivity. |
