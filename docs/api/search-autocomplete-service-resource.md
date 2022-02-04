---
title: Autocomplete, NuGet API
description: The search autocomplete service supports interactive discovery of package IDs and versions.
author: joelverhagen
ms.author: jver
ms.date: 10/26/2017
ms.topic: reference
ms.reviewer: kraigb
---

# Autocomplete

It is possible to build a package ID and version autocomplete experience using the V3 API. The resource used for making
autocomplete queries is the `SearchAutocompleteService` resource found in the [service index](service-index.md).

## Versioning

The following `@type` values are used:

@type value                          | Notes
------------------------------------ | -----
SearchAutocompleteService            | The initial release
SearchAutocompleteService/3.0.0-beta | Alias of `SearchAutocompleteService`
SearchAutocompleteService/3.0.0-rc   | Alias of `SearchAutocompleteService`
SearchAutocompleteService/3.5.0      | Includes support for `packageType` query parameter

### SearchAutocompleteService/3.5.0
This version introduces support for the `packageType` query parameter, allowing filtering by author defined package types. It is fully backwards compatible with queries to `SearchAutocompleteService`.

## Base URL

The base URL for the following APIs is the value of the `@id` property associated with one of the aforementioned
resource `@type` values. In the following document, the placeholder base URL `{@id}` will be used.

## HTTP Methods

All URLs found in the registration resource support the HTTP methods `GET` and `HEAD`.

## Search for package IDs

The first autocomplete API supports searching for part of a package ID string. This is great when you want to provide
a package typeahead feature in a user interface integrated with a NuGet package source.

A package with only unlisted versions will not appear in the results.

```
GET {@id}?q={QUERY}&skip={SKIP}&take={TAKE}&prerelease={PRERELEASE}&semVerLevel={SEMVERLEVEL}&packageType={PACKAGETYPE}
```

### Request parameters

Name        | In     | Type    | Required | Notes
----------- | ------ | ------- | -------- | -----
q           | URL    | string  | no       | The string to compare against package IDs
skip        | URL    | integer | no       | The number of results to skip, for pagination
take        | URL    | integer | no       | The number of results to return, for pagination
prerelease  | URL    | boolean | no       | `true` or `false` determining whether to include [pre-release packages](../create-packages/prerelease-packages.md)
semVerLevel | URL    | string  | no       | A SemVer 1.0.0 version string 
packageType | URL    | string  | no       | The package type to use to filter packages (added in `SearchAutocompleteService/3.5.0`)

The autocomplete query `q` is parsed in a manner that is defined by the server implementation. nuget.org supports
querying for the prefix of package ID tokens, which are pieces of the ID produced by splitting the original by camel
case and symbol characters.

The `skip` parameter defaults to 0.

The `take` parameter should be an integer greater than zero. The server implementation may impose a maximum value.

If `prerelease` is not provided, pre-release packages are excluded.

The `semVerLevel` query parameter is used to opt-in to
[SemVer 2.0.0 packages](https://github.com/NuGet/Home/wiki/SemVer2-support-for-nuget.org-%28server-side%29#identifying-semver-v200-packages).
If this query parameter is excluded, only package IDs with SemVer 1.0.0 compatible versions will be returned (with the 
[standard NuGet versioning](../concepts/package-versioning.md) caveats, such as version strings with 4 integer pieces).
If `semVerLevel=2.0.0` is provided, both SemVer 1.0.0 and SemVer 2.0.0 compatible packages will be returned. See the
[SemVer 2.0.0 support for nuget.org](https://github.com/NuGet/Home/wiki/SemVer2-support-for-nuget.org-%28server-side%29)
for more information.

The `packageType` parameter is used to further filter the autocomplete results to only packages that have at least one package type matching the package type name.
If the provided package type is not a valid package type as defined by the [Package Type document](https://github.com/NuGet/Home/wiki/Package-Type-%5BPacking%5D), an empty result will returned.
If the provided package type is empty, no filter will be applied. In other words, passing no value to the `packageType` parameter will behave as if the parameter was not passed.

### Response

The response is JSON document containing up to `take` autocomplete results.

The root JSON object has the following properties:

Name      | Type             | Required | Notes
--------- | ---------------- | -------- | -----
totalHits | integer          | yes      | The total number of matches, disregarding `skip` and `take`
data      | array of strings | yes      | The package IDs matched by the request

### Sample request

```
GET https://api-v2v3search-0.nuget.org/autocomplete?q=storage&prerelease=true
```

### Sample response

[!code-JSON [autocomplete-id-result.json](./_data/autocomplete-id-result.json)]

## Enumerate package versions

Once a package ID is discovered using the previous API, a client can use the autocomplete API to enumerate package
versions for a provided package ID.

A package version that is unlisted will not appear in the results.

```
GET {@id}?id={ID}&prerelease={PRERELEASE}&semVerLevel={SEMVERLEVEL}
```

### Request parameters

Name        | In     | Type    | Required | Notes
----------- | ------ | ------- | -------- | -----
id          | URL    | string  | yes      | The package ID to fetch versions for
prerelease  | URL    | boolean | no       | `true` or `false` determining whether to include [pre-release packages](../create-packages/prerelease-packages.md)
semVerLevel | URL    | string  | no       | A SemVer 2.0.0 version string 

If `prerelease` is not provided, pre-release packages are excluded.

The `semVerLevel` query parameter is used to opt-in to SemVer 2.0.0 packages. If this query parameter is excluded, only
SemVer 1.0.0 versions will be returned. If `semVerLevel=2.0.0` is provided, both SemVer 1.0.0 and SemVer 2.0.0 versions
will be returned. See the [SemVer 2.0.0 support for nuget.org](https://github.com/NuGet/Home/wiki/SemVer2-support-for-nuget.org-%28server-side%29)
for more information.

### Response

The response is JSON document containing all package versions of the provided package ID, filtering by the given query
parameters.

The root JSON object has the following property:

Name      | Type             | Required | Notes
--------- | ---------------- | -------- | -----
data      | array of strings | yes      | The package versions matched by the request

The package versions in the `data` array may contain SemVer 2.0.0 build metadata (e.g. `1.0.0+metadata`) if the
`semVerLevel=2.0.0` is provided in the query string.

### Sample request

```
GET https://api-v2v3search-0.nuget.org/autocomplete?id=nuget.protocol&prerelease=true
```

### Sample response

[!code-JSON [autocomplete-version-result.json](./_data/autocomplete-version-result.json)]
