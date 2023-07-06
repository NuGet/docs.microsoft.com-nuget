---
title: NuGet 5.11 Release Notes
description: Release notes for NuGet 5.11 including new features, bug fixes, and DCRs.
author: erdembayar
ms.author: eryondon
ms.date: 8/10/2021
ms.topic: conceptual
---

# NuGet 5.11 Release Notes

NuGet distribution vehicles:

| NuGet version | Available in Visual Studio version | Available in .NET SDK(s) |
|:---|:---|:---|
| [**5.11.0**](https://nuget.org/downloads) | [Visual Studio 2019 version 16.11](https://visualstudio.microsoft.com/downloads/) | [5.0.400](https://dotnet.microsoft.com/download/dotnet-core/5.0)<sup>1</sup> |
| [**5.11.2**](https://nuget.org/downloads) | [Visual Studio 2019 version 16.11.16](https://visualstudio.microsoft.com/downloads/) | N/A |
| [**5.11.3**](https://nuget.org/downloads) | N/A | N/A |
| [**5.11.5**](https://nuget.org/downloads) | N/A | N/A |

<sup>1</sup> Installed with Visual Studio 2019 with .NET Core workload
  
> [!NOTE]
> Visual Studio 16.11, MSBuild 16.11, and .NET 5.0.400+ requires NuGet.exe 5.11 or later.

## Summary: What's New in 5.11.5

* [Security]: Microsoft Security Advisory CVE-2023-29337 | NuGet Client Remote Code Execution Vulnerability - [#12653](https://github.com/NuGet/Home/issues/12653)

> [!NOTE]
> There is a behavior breaking change on Linux. The temp folder location, where NuGet stores temporary files during its various operations, has changed from `/tmp/NuGetScratch` to `/tmp/NuGetScratch<username>` on Linux. e.g. for user User1, the temp folder will be `/tmp/NuGetScratchUser1`.

## Summary: What's New in 5.11.3

* [Security]: Microsoft Security Advisory CVE-2022-41032 | .NET Elevation of Privilege Vulnerability - [#12149](https://github.com/NuGet/Home/issues/12149)

## Summary: What's New in 5.11.2

* [Security]: Microsoft Security Advisory CVE-2022-30184 | .NET Information Disclosure Vulnerability - [#11883](https://github.com/NuGet/Home/issues/11883)

## Summary: What's New in 5.11

### Issues fixed in this release

**Bugs:**

* Tools -> Options -> NuGet Package Manager string is truncated - [#10779](https://github.com/NuGet/Home/issues/10779)

* Hang when the PackagesMissingStatusChanged event is fired - [#10854](https://github.com/NuGet/Home/issues/10854)

* The NuGet client ignores the NO_PROXY setting - [#10902](https://github.com/NuGet/Home/issues/10902)

**[List of all issues fixed in this release - 5.11](https://app.zenhub.com/workspaces/nuget-client-team-55aec9a240305cf007585881/reports/release?release=Z2lkOi8vcmFwdG9yL1JlbGVhc2UvNTk5MDE)**

**[List of commits in this release - 5.11](https://github.com/NuGet/NuGet.Client/compare/5.10.0.7240...5.11.0.17)**

## Feedback welcome

Your feedback is important to us.  If there are any problems with this release, check our
[GitHub Issues](https://github.com/NuGet/Home/issues) and
[Visual Studio Developer Community](https://developercommunity.visualstudio.com/)
for existing issues.  For new issues within NuGet, please report a
[GitHub Issue](https://github.com/NuGet/Home/issues/new).
For general NuGet experience issues, let us know via the
[Report a Problem](/visualstudio/ide/how-to-report-a-problem-with-visual-studio)
option found in your favorite IDE under **Help > Report a Problem**.
