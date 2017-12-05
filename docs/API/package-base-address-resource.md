---
# required metadata 

title: Package Content, NuGet API | Microsoft Docs
author:
- joelverhagen
- kraigb
ms.author:
- joelverhagen
- kraigb
manager: skofman
ms.date: 10/26/2017
ms.topic: reference
ms.prod: nuget
ms.technology: null
ms.assetid: ec68b5d1-a684-4995-b1a6-6210dbb24875

# optional metadata

description: The package base address is a simple interface for fetching the package itself.
keywords: NuGet flat container, NuGet package base address, NuGet nupkg API, NuGet API package versions, NuGet API unlisted packages, NuGet API download nuspec
ms.reviewer:
- karann
- unniravindranathan

---

# Package Content

It is possible to generate a URL to fetch an arbitrary package's content (the .nupkg file) using the V3 API. The
resource used for fetching package content is the `PackageBaseAddress` resource found in the
[service index](service-index.md). This resource also enables discovery of all versions of a package, listed or
unlisted.

This resource is commonly referred to as either the "package base address" or as the "flat container".

## Versioning

The following `@type` value is used:

@type value              | Notes
------------------------ | -----
PackageBaseAddress/3.0.0 | The initial release

## Base URL

The base URL for the following APIs is the value of the `@id` property associated with the aforementioned
resource `@type` value. In the following document, the placeholder base URL `{@id}` will be used.

## HTTP methods

All URLs found in the registration resource support the HTTP methods `GET` and `HEAD`.

## Enumerate package versions

If the client knows a package ID and wants to discover which package versions the package source has available, the
client can construct a predictable URL to enumerate all package versions. This list is meant to be a "directory
listing" for the package content API mentioned below.

> [!Note]
> This list contains both listed and unlisted package versions.

```
GET {@id}/{LOWER_ID}/index.json
```

### Request parameters

Name     | In     | Type    | Required | Notes
-------- | ------ | ------- | -------- | -----
LOWER_ID | URL    | string  | yes      | The package ID, lowercase

The `LOWER_ID` value is the desired package ID lowercased using the rules implemented by .NET's
[`System.String.ToLowerInvariant()`](https://msdn.microsoft.com/en-us/library/system.string.tolowerinvariant.aspx)
method.

### Response

If the package source has no versions of the provided package ID, a 404 status code is returned.

If the package source has one or more versions, a 200 status code is returned. The response body is a JSON object with
the following property:

Name     | Type             | Required | Notes
-------- | ---------------- | -------- | -----
versions | array of strings | yes      | The package IDs available

The strings in the `versions` array are all lowercased, 
[normalized NuGet version strings](../reference/package-versioning.md#normalized-version-numbers). The version
strings do not contain any SemVer 2.0.0 build metadata.

The intent is that the version strings found in this array can be used verbatim for the `LOWER_VERSION` tokens found
in the following endpoints.

### Sample request

```
GET https://api.nuget.org/v3-flatcontainer/owin/index.json
```

### Sample response

[!code-JSON [package-base-address-index.json](./_data/package-base-address-index.json)]

## Download package content (.nupkg)

If the client knows a package ID and version and wants to download the package content, they only need to construct the
following URL:

```
GET {@id}/{LOWER_ID}/{LOWER_VERSION}/{LOWER_ID}.{LOWER_VERSION}.nupkg
```

### Request parameters

Name          | In     | Type   | Required | Notes
------------- | ------ | ------ | -------- | -----
LOWER_ID      | URL    | string | yes      | The package ID, lowercase
LOWER_VERSION | URL    | string | yes      | The package version, normalized and lowercased

Both `LOWER_ID` and `LOWER_VERSION` are lowercased using the rules implemented by .NET's
[`System.String.ToLowerInvariant()`](https://msdn.microsoft.com/en-us/library/system.string.tolowerinvariant.aspx)
method.

The `LOWER_VERSION` is the desired package version normalized using NuGet's version
[normalization rules](../reference/package-versioning.md#normalized-version-numbers). This means that build metadata
that is allowed by the SemVer 2.0.0 specification must be excluded in this case.

### Response body

If the package exists on the package source, a 200 status code is returned. The response body will be the package
content itself.

If the package does not exist on the package source, a 404 status code is returned.

### Sample request

```
GET https://api.nuget.org/v3-flatcontainer/newtonsoft.json/9.0.1/newtonsoft.json.9.0.1.nupkg
```

### Sample response

The binary stream that is the .nupkg for Newtonsoft.Json 9.0.1.

## Download package manifest (.nuspec)

If the client knows a package ID and version and wants to download the package manifest, they only need to construct the
following URL:

```
GET {@id}/{LOWER_ID}/{LOWER_VERSION}/{LOWER_ID}.nuspec
```

### Request parameters

Name          | In     | Type    | Required | Notes
------------- | ------ | ------- | -------- | -----
LOWER_ID      | URL    | string  | yes      | The package ID, lowercase
LOWER_VERSION | URL    | integer | yes      | The package version, normalized and lowercased

Both `LOWER_ID` and `LOWER_VERSION` are lowercased using the rules implemented by .NET's
[`System.String.ToLowerInvariant()`](https://msdn.microsoft.com/en-us/library/system.string.tolowerinvariant.aspx)
method.

The `LOWER_VERSION` is the desired package version normalized using NuGet's version
[normalization rules](../reference/package-versioning.md#normalized-version-numbers). This means that build metadata
that is allowed by the SemVer 2.0.0 specification must be excluded in this case.

### Response body

If the package exists on the package source, a 200 status code is returned. The response body will be the package
manifest, which is the .nuspec contained in the corresponding .nupkg. The .nuspec is an XML document.

If the package does not exist on the package source, a 404 status code is returned.

### Sample request

```
GET https://api.nuget.org/v3-flatcontainer/newtonsoft.json/6.0.4/newtonsoft.json.nuspec
```

### Sample response

[!code-XML [newtonsoft.json.6.0.4.xml](./_data/newtonsoft.json.6.0.4.xml)]
