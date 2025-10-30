---
title: NuGet 5.4 Release Notes
description: Release notes for NuGet 5.4 including new features, bug fixes, and DCRs.
author: JonDouglas
ms.author: jodou
ms.date: 09/06/2019
ms.topic: release-notes
---

# NuGet 5.4 Release Notes

NuGet distribution vehicles:

| NuGet version | Available in Visual Studio version| Available in .NET SDK(s)|
|:---|:---|:---|
| [**5.4.0**](https://nuget.org/downloads) | [Visual Studio 2019 version 16.4](https://visualstudio.microsoft.com/downloads/) | [3.1.100](https://dotnet.microsoft.com/download/dotnet-core/3.1)<sup>1</sup> |

<sup>1</sup>Installed with Visual Studio 2019 with .NET Core workload

## Summary: What's New in 5.4

* Faster solution load time - Overhead running NuGet code during first solution load has been reduced via partial-ngen to reduce JIT cost - [#6007](https://github.com/NuGet/Home/issues/6007)

* New helper function - given a list of package ids and versions, get the likely top level packages. - [#8316](https://github.com/NuGet/Home/issues/8316)

* New [`nuget/setup-nuget`](https://github.com/marketplace/actions/setup-nuget-exe-for-use-with-actions) action for installing and configuring NuGet.exe on [GitHub Actions](https://github.com/features/actions). - [#8818](https://github.com/NuGet/Home/issues/8818)

### Issues fixed in this release

**Bugs**

* Plugin: Logging time accuracy is off on linux/Mac - [#8747](https://github.com/NuGet/Home/issues/8747)

* Disposing of a plugin can sometimes throw and fail the whole operation. - [#8732](https://github.com/NuGet/Home/issues/8732)

* Stop displaying version duplicates in allowed and blocked versions list in PMUI - [#8679](https://github.com/NuGet/Home/issues/8679)

* Lock File not properly generated - framework ordering should not impact the restore with lockedmode - [#8645](https://github.com/NuGet/Home/issues/8645)

* LockFile validation fails for projects with \<RuntimeIdentifiers\> set in SDK 3.0.100 - [#8639](https://github.com/NuGet/Home/issues/8639)

* Signing Validation will now properly reject signatures with timestamps which have 2 values under the same OID - [#8629](https://github.com/NuGet/Home/issues/8629)

* Update the license list - [#8544](https://github.com/NuGet/Home/issues/8544)

**DCRs**

* Onboarding diagnostic files to IFeedbackDiagnosticFileProvider - [#8535](https://github.com/NuGet/Home/issues/8535)

**[List of all issues fixed in this release - 5.4](https://github.com/nuget/home/issues?q=is%3Aissue+is%3Aclosed+milestone%3A%225.4")**
