---
title: Deleting NuGet Packages from nuget.org
description: Policies for unlisting packages from nuget.org; permanent deletion is not supported except when packages violate other policies.
author: JonDouglas
ms.author: jodou
ms.date: 01/18/2018
ms.topic: conceptual
---

# Deleting packages

nuget.org does not support permanent deletion of packages. Doing so would break every project depending on the availability of the package, especially with build workflows that involve package restore.

nuget.org does support [unlisting a package](#unlisting-a-package), which can be done in the package management page on the web site. Unlisted packages don't appear on nuget.org or in the Visual Studio UI, and do not appear in search results. Unlisted packages, however, can still be downloaded and installed by using an exact version number, which supports package restore. In addition, unlisted packages may still be discovered in the following specific scenarios:

- Package restore using floating versions (for example, `1.0.0-*`), if the latest available package matching the version or dependency constraints is an unlisted package.
- Replication of packages through the catalog (as the catalog also contains unlisted packages).

## Exceptions

In exceptional situations such as copyright infringement and potentially harmful content, packages can be deleted manually by the NuGet team. You can report a package using the "Report abuse" button on the NuGet.org package details page. If you are the package owner, login to your NuGet.org account to reach NuGet support using the "Contact support" button on the NuGet.org package details page.

## Prohibited use

Packages that meet any of the following criteria are not allowed on the public NuGet gallery and will be immediately removed without discussion. Package owners will, however, be notified of the removal.

- Contains malware, adware, or any kind of spyware.
- Are designed to harm a developer's workstation or their organization.
- Infringes copyrights or violates licenses.
- Contains illegal content.
- Are being used to squat on package identifiers, including packages that have zero productive content. Packages must contain code or the owners must concede the identifier to someone who actually has a product to ship.
- Attempt to make the gallery do something that it's not explicitly designed to do.
- Violates the [nuget.org Terms of Use or Code of Conduct](https://www.nuget.org/policies/Terms) in any way. Terms such as "unexpected", "discriminatory", "hateful", and "abusive" are evaluated and decided at the sole discretion of the NuGet team.

If you find a package that is in violation of any of these items, click the **Report Abuse** link on the package details page and submit a report.

Note that the NuGet team and the .NET Foundation reserves the right to change these criteria at any time.

## Unlisting a package
Unlisting a package version hides it from search and from nuget.org package details page. This allows existing users of the package to continue using it but reduces new adoption since the package is not visible in search.

Steps to unlist a package:

1. Select `Your account name` (at the top right corner) >`Manage packages` > `Published packages`
1. Select the "Manage package" icon
1. Expand the "Listing" section and select the package version
1. Uncheck “List in search results” and select "Save"

The specific package version has now been unlisted. In order to verify this, logout of your account and navigate to the package page (without the version part) e.g.: ```https://www.nuget.org/packages/YOUR-PACKAGE-NAME/```. You will see all versions of that package that have **not** been unlisted. However, the package owner, when logged in, can see all versions and their listing status.

It's also possible to deprecate a package version (in case you can't delete a package version). For more information about deprecating package versions, see [Deprecating packages](../deprecate-packages.md).
