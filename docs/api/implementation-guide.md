---
title: NuGet Server Implementation Guide
description: Guidelines and recommendations to anyone implementing the NuGet Server API in their own package repository
author: zivkan
ms.author: zivkan
ms.date: 07/29/2023
ms.topic: conceptual
---

# NuGet Server Implementation Guide

Prior to 2019, NuGet package repositories could copy the package metadata from the `nupkg`'s [`nuspec` file](../reference/nuspec.md), and return the results unmodified in [search](./search-query-service-resource.md) and [package metadata](./registration-base-url-resource.md) requests.
Since then, NuGet has evolved to provide developers with a richer experience, and this requires package repositories to do additional work in order to provide the additional value to developers.

## Embedded files

Package [icons](../reference/nuspec.md#icon), [license](../reference/nuspec.md#license), and [readme](../reference/nuspec.md#readme) files can be (and is recommended to be) embedded in the package.
These files need a URL endpoint, either extracted and put on a static file, or a URL that dynamically extracts the files from the `.nupkg` on request, so that they can be viewed without downloading the entire `nupkg`.
If your package repository provides package browsing and viewing package details, you can use the URLs to show customers the embedded content on your website.

Finally, the [package metadata resource](./registration-base-url-resource.md) and [search resource](./search-query-service-resource.md) must contain the hosted URL in the `iconUrl`, `licenseUrl`, and/or `readmeUrl` properties of the JSON response.
Packages (`.nupkg` files) are immutable and must not be modified.

Note that the license could be an SPDX expression, or an embedded file (but not both).
Packages that use a license expression, when represented in search and package metadata results, can have the `licenseUrl` set to the license expression, URL encoded, and appended to the end of https://licenses.nuget.org/.
For example, https://licenses.nuget.org/Apache-2.0.

## Owner field

The [package manifest file (`.nuspec`)](../reference/nuspec.md) have two fields, `<authors>` and `<owners>`.
Package authors who are packaging third-party content often put the third-party name in the `<authors>` field.
The `<owners>` field is intended to denote who published the package on a repository, and therefore who should be contacted in case of packing issues or questions.

The `<owners>` field has been deprecated from the `.nuspec` file, so if packages contain this metadata, it should be ignored.
Do not return the value of the `.nuspec` file's `<owners>` field in the `owners` property in the search resource or package metadata resource JSON response.

If your repository has per-package permissions, it is recommended to report the accounts that have permissions to publish new versions in the search and package metadata resources JSON responses.

## Known vulnerability and deprecation data

The [Package Metadata Resource](./registration-base-url-resource.md) can contain [deprecation](./registration-base-url-resource.md#package-deprecation) and [vulnerability](./registration-base-url-resource.md#vulnerabilities) information.
This allows customers browsing packages in Visual Studio's Package Manager User Interface, or equivalent in other IDEs, to be notified of important security or maintenance issues.

Additionally, in order to provide high-performance vulnerability scanning during package restore, NuGet also downloads the full list of known vulnerabilities from [the `VulnerabilityInfo` resource](./vulnerability-info.md).

If your package repository is "up-sourcing" packages from another repository, in order to mirror packages in your own feed, we recommend periodically checking the original source if there is deprecation or vulnerability data, and mirror that metadata in your own repository.
If your package repository is up-sourcing from nuget.org specifically, by keeping state of the last time you checked (a "cursor"), you can use [the `Catalog` resource](./catalog-resource.md) to efficiently check if there are any package updates for packages you're mirroring, without having to download a large number of package metadata JSON files from the upstream feed.

For the `VulnerabilityInfo` resource, nuget.org is providing vulnerability data for all GitHub reviewed advisories from the [GitHub Advisories database](https://github.com/advisories), even packages that are not hosted on nuget.org.
Therefore, if your package repository provides upsourcing from nuget.org, or is otherwise intended on being a feed that customers host all their packages on, not just their private packages, your [service index](./service-index.md) could contain a link to nuget.org's [`VulnerabilityInfo` index](./vulnerability-info.md#vulnerability-index).
However, if your repository is intended to be available on the customer's premises, and therefore not require a connection to the public internet, the vulnerability information should be mirrored.

If your package repository is hosting first-party packages, and you would like to provide vulnerability information to customers using your own feed, but don't yet have any disclosed package vulnerabilities, you should provide a [vulnerability index](./vulnerability-info.md#vulnerability-index) with one or more [vulnerability pages](./vulnerability-info.md#vulnerability-page) whose contents are an empty JSON array (`[]`).
