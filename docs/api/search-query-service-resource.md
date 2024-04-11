---
title: Search, NuGet API
description: The search service allows clients to query for packages by keyword and to filter results on certain package fields.
author: joelverhagen
ms.author: jver
ms.date: 10/26/2017
ms.topic: reference
ms.reviewer: kraigb
---

# Search

It is possible to search for packages available on a package source using the V3 API. The resource used for searching
is the `SearchQueryService` resource found in the [service index](service-index.md).

## Versioning

The following `@type` values are used:

@type value                   | Notes
----------------------------- | -----
SearchQueryService            | The initial release
SearchQueryService/3.0.0-beta | Alias of `SearchQueryService`
SearchQueryService/3.0.0-rc   | Alias of `SearchQueryService`
SearchQueryService/3.5.0      | Includes support for `packageType` query parameter

### SearchQueryService/3.5.0
This version introduces support for the `packageType` query parameter and the `packageTypes` response property, allowing filtering by author defined package types. It is fully backwards compatible with queries to `SearchQueryService`.

## Base URL

The base URL for the following API is the value of the `@id` property associated with one of the aforementioned
resource `@type` values. In the following document, the placeholder base URL `{@id}` will be used. The base URL may change based on implementation or infrastructure changes within the package source so it must be dynamically fetched from the [service index](service-index.md) by the client software.

## HTTP methods

All URLs found in the registration resource support the HTTP methods `GET` and `HEAD`.

## Search for packages

The search API allows a client to query for a page of packages matching a specified search query. The interpretation
of the search query (e.g. the tokenization of the search terms) is determined by the server implementation but the
general expectation is that the search query is used for matching package IDs, titles, descriptions, and tags. Other
package metadata fields may also be considered.

An unlisted package should never appear in search results.

```
GET {@id}?q={QUERY}&skip={SKIP}&take={TAKE}&prerelease={PRERELEASE}&semVerLevel={SEMVERLEVEL}&packageType={PACKAGETYPE}
```

### Request parameters

Name        | In     | Type    | Required | Notes
----------- | ------ | ------- | -------- | -----
q           | URL    | string  | no       | The search terms to used to filter packages
skip        | URL    | integer | no       | The number of results to skip, for pagination
take        | URL    | integer | no       | The number of results to return, for pagination
prerelease  | URL    | boolean | no       | `true` or `false` determining whether to include [pre-release packages](../create-packages/prerelease-packages.md)
semVerLevel | URL    | string  | no       | A SemVer 1.0.0 version string 
packageType | URL    | string  | no       | The package type to use to filter packages (added in `SearchQueryService/3.5.0`)

The search query `q` is parsed in a manner that is defined by the server implementation. nuget.org supports basic
filtering on a [variety of fields](../consume-packages/finding-and-choosing-packages.md#search-syntax). If no
`q` is provided, all packages should be returned, within the boundaries imposed by skip and take. This enables the
"Browse" tab in the NuGet Visual Studio experience.

The `skip` parameter defaults to 0.

The `take` parameter should be an integer greater than zero. The server implementation may impose a maximum value.

> [!Note]
> nuget.org limits the `skip` parameter to 3,000 and the `take` parameter to 1,000.

If `prerelease` is not provided, pre-release packages are excluded.

The `semVerLevel` query parameter is used to opt-in to
[SemVer 2.0.0 packages](https://github.com/NuGet/Home/wiki/SemVer2-support-for-nuget.org-%28server-side%29#identifying-semver-v200-packages).
If this query parameter is excluded, only packages with SemVer 1.0.0 compatible versions will be returned (with the 
[standard NuGet versioning](../concepts/package-versioning.md) caveats, such as version strings with 4 integer pieces).
If `semVerLevel=2.0.0` is provided, both SemVer 1.0.0 and SemVer 2.0.0 compatible packages will be returned. See the
[SemVer 2.0.0 support for nuget.org](https://github.com/NuGet/Home/wiki/SemVer2-support-for-nuget.org-%28server-side%29)
for more information.

The `packageType` parameter is used to further filter the search results to only packages that have at least one package type matching the package type name.
If the provided package type is not a valid package type as defined by the [Package Type document](https://github.com/NuGet/Home/wiki/Package-Type-%5BPacking%5D), an empty result will returned.
If the provided package type is empty, no filter will be applied. In other words, passing no value to the packageType parameter will behave as if the parameter was not passed.

### Response

The response is JSON document containing up to `take` search results. Search results are grouped by package ID.

The root JSON object has the following properties:

Name      | Type             | Required | Notes
--------- | ---------------- | -------- | -----
totalHits | integer          | yes      | The total number of matches, disregarding `skip` and `take`
data      | array of objects | yes      | The search results matched by the request

### Search result

Each item in the `data` array is a JSON object comprised of a group of package versions sharing the same package ID.
The object has the following properties:

Name           | Type                       | Required | Notes
-------------- | -------------------------- | -------- | -----
id             | string                     | yes      | The ID of the matched package
version        | string                     | yes      | The full SemVer 2.0.0 version string of the package (could contain build metadata)
description    | string                     | no       | 
versions       | array of objects           | yes      | All of the versions of the package matching the `prerelease` parameter
authors        | string or array of strings | no       | 
iconUrl        | string                     | no       | 
licenseUrl     | string                     | no       | 
owners         | string or array of strings | no       | A string represents a single owner's username.
projectUrl     | string                     | no       | 
registration   | string                     | no       | The absolute URL to the associated [registration index](registration-base-url-resource.md#registration-index)
summary        | string                     | no       | 
tags           | string or array of strings | no       | 
title          | string                     | no       | 
totalDownloads | integer                    | no       | This value can be inferred by the sum of downloads in the `versions` array
verified       | boolean                    | no       | A JSON boolean indicating whether the package is [verified](../nuget-org/id-prefix-reservation.md)
packageTypes   | array of objects           | yes      | The package types defined by the package author (added in `SearchQueryService/3.5.0`)

On nuget.org, a verified package is one which has a package ID matching a reserved ID prefix and owned by one of the
reserved prefix's owners. For more information, see the
[documentation about ID prefix reservation](../nuget-org/id-prefix-reservation.md).

The metadata contained in the search result object is taken from the latest package version. Each item in the
`versions` array is a JSON object with the following properties:

Name      | Type    | Required | Notes
--------- | ------- | -------- | -----
@id       | string  | yes      | The absolute URL to the associated [registration leaf](registration-base-url-resource.md#registration-leaf)
version   | string  | yes      | The full SemVer 2.0.0 version string of the package (could contain build metadata)
downloads | integer | yes      | The number of downloads for this specific package version

The `packageTypes` array will always consist of at least one (1) item. Package type for a given package ID is considered to be the package types defined by the latest version of the package with respect to the other search parameters. Each item in the `packageTypes` array is a JSON object with the following properties:

Name      | Type    | Required | Notes
--------- | ------- | -------- | -----
name      | string  | yes      | The name of the package type.

### Sample request

```
GET https://search-sample.nuget.org/query?q=NuGet.Versioning&prerelease=false&semVerLevel=2.0.0
```

Make sure to fetch the base URL (`https://search-sample.nuget.org/query` in this sample) from the service index as mentioned in the [base URL](#base-url) section.

### Sample response

[!code-JSON [search-result.json](./_data/search-result.json)]
