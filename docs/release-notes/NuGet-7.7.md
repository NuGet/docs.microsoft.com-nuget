---
title: NuGet 7.7 Release Notes
description: Release notes for NuGet 7.7 including new features, bug fixes, and DCRs.
author: kartheekp-ms
ms.date: 06/04/2026
ms.topic: release-notes
---

# NuGet 7.7 Release Notes

NuGet distribution vehicles:

| NuGet version | Available in Visual Studio version | Available in .NET SDK(s) |
|:---|:---|:---|
| [**7.7.0**](https://nuget.org/downloads) | [Visual Studio 2026 version 18.7.0](https://visualstudio.microsoft.com/downloads/) | N/A |

> [!NOTE]
> Visual Studio-only release.
> NuGet SDK packages are published quarterly with .NET.

## Summary: What's New in 7.7.0

* Add option to not write out dependency graph spec - [#14114](https://github.com/NuGet/Home/issues/14114)

### Issues fixed in this release

* Pin System.Security.Cryptography.Xml to 8.0.3 - [#14865](https://github.com/NuGet/Home/issues/14865)

* Test TSTInfo structure generation is not DER-compliant - [#14838](https://github.com/NuGet/Home/issues/14838)

* SourceRepositoryDependencyProvider.GetInstalledVersions returns null when packages may be available. - [#14837](https://github.com/NuGet/Home/issues/14837)

* [Bug Bash] [Unstable] The ‘Installed’ tab was not refreshed after fixing the NuGet package vulnerabilities with GitHub Copilot - [#14761](https://github.com/NuGet/Home/issues/14761)

[List of commits in this release](https://github.com/NuGet/NuGet.Client/compare/7.6.0.59...7.7.0.44)

### Community contributions

Thank you to all the contributors who helped make this NuGet release awesome!

* [jjonescz](https://github.com/NuGet/NuGet.Client/pull/7316)
  * [7316](https://github.com/NuGet/NuGet.Client/pull/7316) Switch file-based app integration tests to use dotnet.exe only
* [nareshjo](https://github.com/NuGet/NuGet.Client/pull/7266)
  * [7266](https://github.com/NuGet/NuGet.Client/pull/7266) Cache NUGET_DISABLE_PACKAGEID_VALIDATION env var read in PackageIdValidator.Validate to eliminate per-call allocations
