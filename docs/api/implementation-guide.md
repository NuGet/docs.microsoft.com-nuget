---
title: NuGet Server Implementation Guide
description: Guidelines and recommendations to anyone implementing the NuGet Server API in their own package repository
author: zivkan
ms.author: zivkan
ms.date: 07/29/2023
ms.topic: conceptual
---

# NuGet Server Implementation Guide

In some cases, you may want to implement your own NuGet package feed.
Many [existing implementations exist](../hosting-packages/Overview.md) which allow you to host your own feed in a variety way, but the protocol between the official NuGet client software and a package feed is documented allowing you to build your own feed implementation from scratch.

The protocol does evolve over time and this guide is aimed at those that wish to or already have implemented a NuGet package server.

Since the initial release of the NuGet V3 protocol in 2015, NuGet has evolved to provide developers with a richer experience, and this requires package repositories to do additional work in order to provide the additional value their package consumers, beyond simply exacting metadata from hosted packages and returning the metadata in various forms.
For example, the search and package metadata endpoints contain more than just metadata found in the nupkg's nuspec file.

Note that this guide is focused on the NuGet V3 protocol since the V2 protocol is essentially undocumented and, since 2015, the recommended protocol for NuGet client and server communication is the V3 protocol. For more information read about protocol versioning.

## Chronology

To assist authors of existing NuGet repositories keep up to date with NuGet's newest features, here is the chronology of the relevant features mentioned in the remainder of the document.

|Year|Feature|
|--|--|
|2013|[A blog post explaining how to manage package owners on nuget.org](https://devblogs.microsoft.com/nuget/managing-package-owners/) clarified the owners shown on the website are the accounts that have permission to upload new versions, and therefore [the `owners` metadata in the package is ignored](#owner-field)|
|2017|[Added `verified` to `SearchQueryService` responses.](#verified-search-response-field)|
||[Semantic Versioning 2.0.0 support](#semantic-versioning-200-support)
|2018|[Embedded licenses](#embedded-files)|
|2019|[Embedded icons](#embedded-files)|
||[Package deprecation in `RegistrationBaseUrl` (package metadata resource)](#known-vulnerability-and-deprecation-data)|
|2020|[Package vulnerability information in `RegistrationsBaseUrl` (package metadata resource)](#known-vulnerability-and-deprecation-data)|
||Added `packageTypes` query parameter to `SearchQueryService` requests|
|2021|[Embedded readme](#embedded-files)|
|2023|[PreAuthenticate authenticated requests](#url-structure-for-authenticated-feeds) <br/> [`VulnerabilityInfo` resource](#known-vulnerabilities-database-vulnerabilityinfo)|
|2025|[PreAuthenticate authenticated requests](#url-structure-for-authenticated-feeds) <br/> [`VulnerabilityInfo` resource](#known-vulnerabilities-database-vulnerabilityinfo) <br/> [Enable embedded README downloads](#enable-embedded-readme-downloads)|

## Owner field

Consider two of the [package manifest file (`.nuspec`)](../reference/nuspec.md) fields, `<authors>` and `<owners>`.
Package authors who are packaging third-party content often put the third-party name in the `<authors>` field.
The `<owners>` field was intended to denote who published the package on a repository, and therefore who should be contacted in case of packing issues or questions.

This was explained in [a blog post from 2013](https://devblogs.microsoft.com/nuget/managing-package-owners/), so the `<owners>` field is considered deprecated in the `.nuspec` file.
If package's manifest contain this metadata, it should be ignored.
Do not return the value of the `.nuspec` file's `<owners>` field in the `owners` property in the [search resource](./search-query-service-resource.md) or [package metadata resource](./registration-base-url-resource.md) JSON response.

If your repository has per-package permissions, it is recommended to report the accounts that have permissions to publish new versions in the `owner` metadata for search and package metadata resources JSON responses.

## `verified` search response field

Visual Studio's Package Manager UI shows a blue checkmark next to packages in [search service](./search-query-service-resource.md) results, when a new field `verified` is set to `true`.

NuGet.org uses this with package prefix data (server side data, not part of the NuGet API), so that this checkmark is only shown to customers when the account that owns the package uploaded the package.
For example, any package with prefix `microsoft.*` is verified only when the package is owned by [the Microsoft account on nuget.org](https://www.nuget.org/profiles/Microsoft).
Anyone who uploaded a package with package id starting with `microsoft.` before reserved prefixes were implemented, will not have this verified checkmark.
NuGet.org also allows prefixes not to be exclusive, so that anyone can upload a package under `Contoso.ToolWithPlugins.Community.*`, but will not get a verified checkmark.

## Semantic Versioning 2.0.0 support

NuGet supports [a hybrid between `System.Version` and Semantic Version](../concepts/Package-Versioning.md#where-nugetversion-diverges-from-semantic-versioning), but support for Semantic Version 2.0.0 was added in 2017.
Therefore, NuGet API resources that return versions to client versions lower than 3.6.0 must not return packages that use Semantic 2.0.0 features that are incompatible with Semantic Versioning 1.0.0.

The most important differences between the two versions are the pre-release labels, and metadata string.
The [Semantic Versioning 1.0.0 spec](https://semver.org/spec/v1.0.0.html) provides `[0-9A-Za-z-]` as a sample Regular Expression string for the only characters allowed as part of the pre-release label, and does not support metadata strings.
The [Semantic Versioning 2.0.0 spec](https://semver.org/spec/v2.0.0.html) allows pre-release identifiers to be separated by `.` characters (and forbids a numeric identifier from having a leading zero), and additionally allows build metadata to be added following a `+`.

In the [package metadata resource (`RegistrationsBaseUrl`)](./registration-base-url-resource.md), resource versions below 3.6.0 must only return packages that comply with .NET's `System.Version` or Semantic Versioning 1.0.0.
This means packages whose versions are only compliant with Semantic Versioning 2.0.0 are invisible to these client versions.

Similarly, the [search query service (`SearchQueryService`)](./search-query-service-resource.md) and [autocomplete service (`SearchAutocompleteService`)](./search-autocomplete-service-resource.md) added `&semVerLevel={version}` query parameters.
When `semVerLevel` is missing, assume the value `1.0.0`.
Like the package metadata resource, packages whose version is compatible only with Semantic Versioning 2.0.0 must not be returned when the `semVerLevel` value is below 2.0.0.

## Embedded files

Package [icons](../reference/nuspec.md#icon), [license](../reference/nuspec.md#license), and [readme](../reference/nuspec.md#readme) files can be (and are recommended to be) embedded in the package.
These files need a URL endpoint, either extracted and put on a static file server, or a URL that dynamically extracts the files from the `.nupkg` on request, so that they can be viewed without downloading the entire `nupkg`.
If your package repository provides package browsing and viewing package details, you can use the URLs to show customers the embedded content on your website.

Finally, the [package metadata resource](./registration-base-url-resource.md) and [search resource](./search-query-service-resource.md) must contain the hosted URL in the `iconUrl`, `licenseUrl`, and/or `readmeUrl` properties of the JSON response.
Packages (`.nupkg` files) must not be modified, as client features (lock files and signed packages) will detect modifications as the package having been tampered with.

Note that the license could be an SPDX expression, or an embedded file (but not both).
Packages that use a license expression, when represented in search and package metadata results, can have the `licenseUrl` set to the license expression, URL encoded, and appended to the end of https://licenses.nuget.org/.
For example, https://licenses.nuget.org/Apache-2.0.
The NuGet.org server team have additional [documentation on licenses.nuget.org](../nuget-org/licenses.nuget.org.md).

## Known vulnerability and deprecation data

### Package Metadata Resource (`RegistrationsBaseUrl`)

The [Package Metadata Resource](./registration-base-url-resource.md) can contain [deprecation](./registration-base-url-resource.md#package-deprecation) and [vulnerability](./registration-base-url-resource.md#vulnerabilities) information.
This allows customers browsing packages in Visual Studio's Package Manager User Interface, or equivalent in other IDEs, to be notified of important security or maintenance issues.

If your package repository is "up-sourcing" packages from another repository, in order to mirror packages in your own feed, we recommend periodically checking the original source if there is deprecation or vulnerability data, and mirror that metadata in your own repository.
If your package repository is up-sourcing from nuget.org specifically, by keeping state of the last time you checked (a "cursor"), you can use [the `Catalog` resource](./catalog-resource.md) to efficiently check if there are any package updates for packages you're mirroring, without having to download a large number of package metadata JSON files from the upstream feed.
There is [a guide on using the catalog resource](../guides/api/query-for-all-published-packages.md) with sample code that can help you get started.

### Known Vulnerabilities Database (`VulnerabilityInfo`)

In order to provide high-performance vulnerability scanning during package restore, NuGet downloads the full list of known vulnerabilities from [the `VulnerabilityInfo` resource](./vulnerability-info.md).
Nuget.org provides vulnerability data for all GitHub reviewed advisories from the [GitHub Advisories database](https://github.com/advisories), which includes packages that are not hosted on nuget.org.

If your package repository is hosting first-party packages, and you would like to provide vulnerability information to customers using your own feed, but don't yet have any disclosed package vulnerabilities, you should provide a [vulnerability index](./vulnerability-info.md#vulnerability-index) with one or more [vulnerability pages](./vulnerability-info.md#vulnerability-page) whose contents are an empty JSON array (`[]`).

If your package repository is intended to be used by apps as the default repository (instead of nuget.org), you can use nuget.org's vulnerability data.
One option is to use nuget.org's vulnerability index URL in your service index.
Another option is to periodically check nuget.org's `VulnerabilityInfo` index, and download any changed pages to mirror locally.

## `packageTypes` search query

The .NET CLI allows searching for .NET tool packages with the `dotnet tool search` command.
This is implemented by adding a `&packageTypes={value}` query parameter to the [search query resource](./search-query-service-resource.md), that reads values from the package's `.nuspec` file `<packageTypes>` field.

## URL structure for authenticated feeds

As described in [the overview of the NuGet API](./overview.md), the starting URL for all NuGet server communication is [the service index](./service-index.md).
This document contains the URLs for all other resources that NuGet clients will query.
As of NuGet 6.7 (Visual Studio & MSBuild 17.7, and .NET SDK 7.0.400), NuGet uses [.NET's `HttpClientHandler.PreAuthenticate`](/dotnet/api/system.net.http.httpclienthandler.preauthenticate), which only avoids anonymous HTTP requests  when subsequent URLs are in the same virtual directory, or a subdirectory, of a URL that has previously been authenticated.
This will dramatically reduce the number of unauthenticated HTTP requests sent to the server, and therefore will reduce your server workload.

Here are some examples:

|URL|Will PreAuthenticate?|
|--|--|
|https://pkgs.contoso.com/nuget/v3/feed/index.json|N/A, this is the service index.|
|https://pkgs.contoso.com/nuget/v3/search|No, not in the same or sub-directory as the service index.|
|https://search.pkgs.contoso.com/nuget/v3/feed/|No, not on the same host name as the service index.|
|https://pkgs.contoso.com/nuget/v3/feed/search|Yes, in the same directory as the service index.|
|https://pkgs.contoso.com/nuget/v3/registration/|No, not in a subdirectory of the service index.|
|https://pkgs.contoso.com/nuget/v3/feed/registration/|Yes, in a subdirectory of the service index.|
|https://pkgs.contoso.com/nuget/v3/{guid}/registration/|See below|

In the last example, the server might have a canonical (in this example a guid) name, and have one or more aliases. 
If the service index request was authenticated on a non-canonical URL (the "friendly" name, in our example `feed`), then no, any requests to resources under the canonical URL will not match `HttpClientHandler`'s rules for `PreAuthenticate`.
However, if the non-canonical URL is an HTTP redirection to the canonical URL, https://pkgs.contoso.com/nuget/v3/{guid}/index.json, then this URL will be used in `HttpClientHandler`'s credential cache.
In this case, every request to the service index will have additional latency, due to the redirection.

While NuGet's V3 API was designed to work on a static file server, the search resource is the exception that always requires a dynamic web service to process requests.
If you wish to host search, or indeed any other NuGet API resource, on different servers, in order to benefit from `HttpClientHandler`'s `PreAuthenticate`, you will need to use a reverse proxy to ensure all customer facing URLs in the service index meet the "same or subdirectory" rule.

## Enable embedded README downloads

A [new resource](./readme-template-resource.md) was documented for constructing a URL that can be used to download a README for a given package. This will allow client, like the Package Management UI in VS, to display the embedded README for packages which haven't been previously installed by the user. The client will construct this URL and attempt to download the README, using the response to the request to determine if a README is available. This means servers should expect multiple requests to the constructed endpoint as users navigate the PM UI. 