---
title: NuGet 3.4 Release Notes
description: Release notes for NuGet 3.4 including known issues, bug fixes, added features, and DCRs.
author: JonDouglas
ms.author: jodou
ms.date: 11/11/2016
ms.topic: release-notes
---

# NuGet 3.4 Release Notes

[NuGet 3.4-RC Release Notes](../release-notes/nuget-3.4-RC.md) | [NuGet 3.4.1 Release Notes](../release-notes/nuget-3.4.1.md)

NuGet 3.4 was released March 30, 2016 as part of the Visual Studio 2015 Update 2 and Visual Studio 15 Preview Release and was built with a few tenets in minds:

* Cross-Platform support
* Performance improvements
* Minor UI improvements

The following features were previously added in the RC and have been updated or completed for the 3.4 release:

## New Features

* NuGet clients now support gzip content-encoding from repositories
* Support for PDBs from packages in xproj projects
* Support for iOS and Android build actions in the contentFiles element
* Support for the netstandard and netstandardapp framework monikers

## New User Interface Features

* Significant performance improvements especially on the Installed, Updates, and Consolidate tabs
* Aggregate 'All Package Sources' Source is available with proper search result merging
* Installed and Updates tabs are now sorted alphabetically
* Added a Refresh button that allows a search to be refreshed
* Latest Build options at the top of the Version list

## Updates and Improvements

* Packages referenced in `project.json` that have a floating version will not update on every build. Instead, they will update only when forced to restore, clean, rebuild, or modify `project.json`.
* nuget.org repository sources are no longer forced into a project configuration when you use the NuGet configuration UI.
* NuGet no longer restores packages in shared projects nor writes a lock file.
* We've improved network failure and retry handling for unreachable or slow-to-respond servers.
* Keyboard and mouse behaviors are improved in the Visual Studio Package Manager UI.
* We now support the latest `project.json` schema in DNX.

## Breaking Changes

* Package version numbers are now normalized to the format *major*.*minor*.*patch*-*prerelease*   Each of major, minor, and patch are treated as integers and drop any leading zeroes.  The prerelease information is treated as a string and no changes are applied to it. These numbers are used in queries by the NuGet clients and the search provided by the nuget.org service.  More details can be found in the NuGet Docs under [Prerelease Versions](../create-packages/prerelease-packages.md).

## Known Issues

* **Issue:** Windows 10 v1511 users may experience issues or even a Visual Studio crash with Powershell in Visual Studio in the following scenarios:
    * Installing / Uninstalling packages that have install.ps1 / uninstall.ps1 scripts
    * Loading projects that have an init.ps1 script (like EntityFramework)
    * Publishing web content

* **Workaround:** Ensure that your Windows 10 install has the latest patches applied, expecially the January 2016 (KB 3124263) or a later update.  More details are available on [GitHub issue #1638](https://github.com/nuget/home/issues/1638)

* **Issue:** NuGet v2 protocol redirects are broken.
Custom NuGet repositories that redirect requests to an alternative host do not honor the redirect request.
* **Workaround:**  To work around this issue, configure the package repository URI in settings to point to the redirected server location.
For more information, see [GitHub pull request #387](https://github.com/NuGet/NuGet.Client/pull/387).

We continue to track issues on our GitHub issues list which can be found at: [https://github.com/nuget/home/issues](https://github.com/nuget/home/issues)
