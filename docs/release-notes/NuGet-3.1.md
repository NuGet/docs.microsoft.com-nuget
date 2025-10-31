---
title: NuGet 3.1 Release Notes
description: Release notes for NuGet 3.1 including known issues, bug fixes, added features, and DCRs.
author: JonDouglas
ms.author: jodou
ms.date: 11/11/2016
ms.topic: release-notes
---

# NuGet 3.1 Release Notes

[NuGet 3.0 Release Notes](../release-notes/nuget-3.0.0.md) | [NuGet 3.1.1 Release Notes](../release-notes/nuget-3.1.1.md)

NuGet 3.1 was released on July 27, 2015 as a bundled extension to the Universal Windows Platform SDK for Visual Studio 2015. We delivered this release with the Windows Platform SDK so that the Windows development experience could take advantage of the NuGet cross-platform work that had been previously started. This NuGet extension version is only available for Visual Studio 2015.

We recommend those developers that have access to the Visual Studio gallery update to the latest version that is available, as we are always publishing updates with bug fixes and new features.

## NuGet Visual Studio Extension

Issues and features in this release are tagged on GitHub with the ["3.1 RTM UWP transitive support" milestone](https://github.com/NuGet/Home/issues?utf8=%E2%9C%93&q=is%3Aclosed+milestone%3A%223.1+RTM+UWP+transitive+support%22+)  In total, we closed 67 issues in the 3.1 release.

### New Features

* `project.json` support for Windows UWP and ASP.NET 5 support
* Transitive package installation

Description and definition of these features can be found elsewhere in the documentation.

### Deprecated

The following features are no longer available for Visual Studio 2015:

* Solution level packages can no longer be installed

The following features are no longer available for Visual Studio 2015 and projects that use the `project.json` specification

* `install.ps1` and `uninstall.ps1` - These scripts will be ignored during package install, restore, update, and uninstall
* Configuration transforms will be ignored
* Content will be carried, but not copied into a project.
    * The team is working to re-implement this feature, follow the discussion and progress at: [https://github.com/NuGet/Home/issues/627](https://github.com/NuGet/Home/issues/627)


### Known Issues

There were a number of known issues delivered with this release.

* Installation of the 3.1 release with the Windows 10 SDK will downgrade any version of NuGet extension that was previously installed

## NuGet Command-line

The NuGet command-line executable was updated and moved to a new distributable location so that historical versions of nuget.exe can continue to be made available.  You can download the 3.1 beta version of nuget.exe for Windows at: [https://dist.nuget.org/win-x86-commandline/v3.1.0-beta/nuget.exe](https://dist.nuget.org/win-x86-commandline/v3.1.0-beta/nuget.exe)

The new distributable location resides on the dist.nuget.org host, with a folder structure that follows this template:

```
{platform supported}/{version}/nuget.exe
```

### New Features

* nuget.exe can restore and install packages into projects that use a `project.json` file.
* nuget.exe can connect to and consume the NuGet v3 protocol at: [https://api.nuget.org/v3/index.json](https://api.nuget.org/v3/index.json)

## Known Issues ##

1.    Cannot execute pack against a `project.json` file - [928](https://github.com/NuGet/Home/issues/928)
2.    Is not supported on Mono - [1059](https://github.com/NuGet/Home/issues/1059)
3.    Is not localized - [1058](https://github.com/NuGet/Home/issues/1058),   [1057](https://github.com/NuGet/Home/issues/1057)
4.    Is not signed, just like the existing https://nuget.org/nuget.exe - [1073](https://github.com/NuGet/Home/issues/1073)
