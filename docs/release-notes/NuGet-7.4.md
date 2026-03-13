---
title: NuGet 7.4 Release Notes
description: Release notes for NuGet 7.4 including new features, bug fixes, and DCRs.
author: jebriede
ms.date: 3/12/2026
ms.topic: release-notes
---

# NuGet 7.4 Release Notes

NuGet distribution vehicles:

| NuGet version | Available in Visual Studio version | Available in .NET SDK(s) |
|:---|:---|:---|
| [**7.4.0**](https://nuget.org/downloads) | [Visual Studio 2026 version 18.4.0](https://visualstudio.microsoft.com/downloads/) | N/A

## Summary: What's New in 7.4.0

### Issues fixed in this release

* Ensure PM UI refreshes when audit sources are updated - [#14688](https://github.com/NuGet/Home/issues/14688)

* NU1701 Warning Not Suppressed by NoWarn=&quot;NU1701&quot; on PackageReference (dotnet restore / build) - [#14756](https://github.com/NuGet/Home/issues/14756)

[List of commits in this release](https://github.com/NuGet/NuGet.Client/compare/7.3.0.78...7.4.0.28)

### Community contributions

Thank you to all the contributors who helped make this NuGet release awesome!

* [nareshjo](https://github.com/NuGet/NuGet.Client/pull/7083)
  * [7083](https://github.com/NuGet/NuGet.Client/pull/7083) Reduce List resize allocations in PopulateItemGroups hot path
  * [7056](https://github.com/NuGet/NuGet.Client/pull/7056) Avoid LineInfoAnnotation allocations in MetadataFieldConverter JSON parsing
  * [7050](https://github.com/NuGet/NuGet.Client/pull/7050) Reduce allocation in GetNuGetProjects by providing capacity for List
  * [7037](https://github.com/NuGet/NuGet.Client/pull/7037) Reduce allocation in GetContentFileGroup by providing accurate capacity for List
  * [7035](https://github.com/NuGet/NuGet.Client/pull/7035) Reduce allocation in DownloadTimeoutStream.ReadAsync caused by avoida…
  * [7026](https://github.com/NuGet/NuGet.Client/pull/7026) Fix list capacity calculation in GetGraphItemAsync that is leading to allocations
