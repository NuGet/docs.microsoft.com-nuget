---
title: README Uri Template, NuGet API
description: The README uri template allows clients to download the readme for a package, if available.
author: jgonz120
ms.author: jongonza
ms.date: 1/6/2025
ms.topic: reference
ms.reviewer: 
---

# README Uri Template

It is possible for a client to build a URL that can be used to download a README for a specific package.
This will enable the clients to render the package's README without downloading the entire package.

The resource used for building this URL is the `ReadmeUriTemplate` resource found in the
[service index](service-index.md).

## Versioning

The following `@type` values are used:

@type value                       | Notes
--------------------------------- | -----
ReadmeUriTemplate/6.13.0          | The initial release

## URL template

The URL for the following API is the value of the `@id` property associated with one of the aforementioned
resource `@type` values.

## HTTP methods

The constructed URL must support the HTTP method `GET`

## Construct the URL

Given a known package ID and version, the client implementation can construct a URL to download the README. 

The value of the `@id` is a URL string containing any of the following placeholder tokens:

### URL placeholders

Name        | Type    | Required | Notes
----------- | ------- | -------- | -----
`{lower_id}`      | string  | no       | The package ID, lowercased
`{lower_version}` | string  | no       | The package version, lowercased

Both `lower_id` and `lower_version` are lowercased using the rules implemented by .NET's
[`System.String.ToLowerInvariant()`](/dotnet/api/system.string.tolowerinvariant?view=netstandard-2.0#System_String_ToLowerInvariant&preserve-view=true)
method.

The `lower_version` is the desired package version normalized using NuGet's version
[normalization rules](../concepts/package-versioning.md#normalized-version-numbers). This means that build metadata
that is allowed by the SemVer 2.0.0 specification must be excluded in this case.

### Response body

If the package has a readme, a 200 status code is returned. The response body will be the readme
content itself.

If the package does not have a readme, a 404 status code is returned.
