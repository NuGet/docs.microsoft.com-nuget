---
title: Update Packages, NuGet API
description: The publish service allows clients to publish new packages, unlist/delete, or deprecate existing packages.
author: joelverhagen
ms.author: jver
ms.date: 10/26/2017
ms.topic: reference
ms.reviewer: kraigb
---

# Push, delete/unlist, relist, deprecate

It is possible to push, delete (or unlist, depending on the server implementation), and relist packages using the NuGet
V3 API. These operations are based off of the `PackagePublish` resource found in the [service index](service-index.md).

Many package feeds do not allow the modification of existing package content. However some metadata about a package can be modified after upload, e.g. listed status and deprecation information. Please refer to documentation for your specific package source as needed. This document generally describes generic behavior and behavior on nuget.org.

## Versioning

The following `@type` value is used:

@type value                    | Notes
------------------------------ | -----
PackagePublish/2.0.0           | The initial release
PackagePublish/3.0.0-preview.1 | Include support for deprecating packages (**contract may change in the future**)

## Base URL

The base URL for the following APIs is the value of the `@id` property of the `PackagePublish` resource in the
package source's [service index](service-index.md). For the documentation below, nuget.org's URL is used. Consider 
`https://www.nuget.org/api/v2/package` as a placeholder for the `@id` value found in the service index.

Note that this URL points to the same location as the legacy V2 push endpoint since the protocol is the same.

## HTTP methods

The `PUT`, `POST` and `DELETE` HTTP methods are supported by this resource. For which methods are supported on each
endpoint, see below.

## Push a package

> [!Note]
> nuget.org has [additional requirements](NuGet-Protocols.md) for interacting with the push endpoint.

nuget.org supports pushing new packages using the following API. If the package with the provided ID and version
already exists, nuget.org will reject the push. Other package sources may support replacing an existing package.

```
PUT https://www.nuget.org/api/v2/package
```

### Request parameters

Name           | In     | Type   | Required | Notes
-------------- | ------ | ------ | -------- | -----
X-NuGet-ApiKey | Header | string | yes      | For example, `X-NuGet-ApiKey: {USER_API_KEY}`

The API key is an opaque string gotten from the package source by the user and configured into the client. No
particular string format is mandated but the length of the API key should not exceed a reasonable size for HTTP header
values.

### Request body

The request body must come in the following form:

#### Multipart form data

The request header `Content-Type` is `multipart/form-data` and the first item in the request body is the raw bytes of
the .nupkg being pushed. Subsequent items in the multipart body are ignored. The file name or any other headers of the
multipart items are ignored.

### Response

Status Code | Meaning
----------- | -------
201, 202    | The package was successfully pushed
400         | The provided package is invalid
409         | A package with the provided ID and version already exists

Server implementations vary on the success status code returned when a package is successfully pushed.

## Delete (or unlist) a package

nuget.org interprets the package delete request as an "unlist". This means that the package is still available for
existing consumers of the package but the package no longer appears in search results or in the web interface. For
more information about this practice, see the
[Deleted Packages](../nuget-org/policies/deleting-packages.md) policy. Other server
implementations are free to interpret this signal as a hard delete, soft delete, or unlist. For example,
[NuGet.Server](https://www.nuget.org/packages/NuGet.Server) (a server implementation only supporting the older V2 API)
supports handling this request as either an unlist or a hard delete based on a configuration option.

```
DELETE https://www.nuget.org/api/v2/package/{ID}/{VERSION}
```

### Request parameters

Name           | In     | Type   | Required | Notes
-------------- | ------ | ------ | -------- | -----
ID             | URL    | string | yes      | The ID of the package to delete
VERSION        | URL    | string | yes      | The version of the package to delete
X-NuGet-ApiKey | Header | string | yes      | For example, `X-NuGet-ApiKey: {USER_API_KEY}`

### Response

Status Code | Meaning
----------- | -------
204         | The package was deleted
404         | No package with the provided `ID` and `VERSION` exists

## Relist a package

If a package is unlisted, it is possible to make that package once again visible in search results using the "relist"
endpoint. This endpoint has the same shape as the [delete (unlist) endpoint](#delete-or-unlist-a-package) but uses the `POST`
HTTP method instead of the `DELETE` method.

If the package is already listed, the request still succeeds.

```
POST https://www.nuget.org/api/v2/package/{ID}/{VERSION}
```

### Request parameters

Name           | In     | Type   | Required | Notes
-------------- | ------ | ------ | -------- | -----
ID             | URL    | string | yes      | The ID of the package to relist
VERSION        | URL    | string | yes      | The version of the package to relist
X-NuGet-ApiKey | Header | string | yes      | For example, `X-NuGet-ApiKey: {USER_API_KEY}`

### Response

Status Code | Meaning
----------- | -------
200         | The package is now listed
404         | No package with the provided `ID` and `VERSION` exists

## Deprecate or undeprecate a package

> [!Warning]
> This API is currently in public preview. The API contract may change in the future. To ensure the package source still supports this contract, verify the `PackagePublish/3.0.0-preview.1` resource is listed in the [service index](service-index.md).

> [!Note]
> For nuget.org, create an API key with the ability to **unlist**. This is the scope checked for this operation.

A package can be marked as deprecated or have deprecation details modified and removed using this endpoint. This endpoint uses the `PUT` HTTP method and expects a JSON request body.

If the package already has deprecation details matching the request body, the request still succeeds.

To mark a package as deprecated, set one or more of `isLegacy`, `hasCriticalBugs`, or `isOther` (generally called deprecation statuses or deprecation reasons) to `true`. To undeprecate a package (i.e. remove deprecation details), exclude all of the deprecation status booleans from the request body allowing them to default to `false`.

A request body with only the required `versions` property will remove any deprecation information from all specified versions, since the deprecation status (reason) booleans default to `false`.

You can mark the versions as unlisted at the same time by including the `listedVerb` property set to the `Unlist` string.

For more information on the user experience of package deprecation, see [deprecating packages](../nuget-org/Deprecate-packages.md).

```
PUT https://www.nuget.org/api/v2/package/{ID}/deprecations
```

### Request parameters

Name                    | In           | Type             | Required | Notes
----------------------- | ------------ | ------           | -------- | -----
ID                      | URL          | string           | yes      | The ID of the package to modify deprecation information on
X-NuGet-ApiKey          | Header       | string           | yes      | For example, `X-NuGet-ApiKey: {USER_API_KEY}`
User-Agent              | Header       | string           | yes      | A user agent string is required, optionally include a URL to your project if possible, e.g. `Spoon-Knife/1.0.0 (+https://github.com/octocat/Spoon-Knife)`
versions                | Request body | array of strings | yes      | The package versions to apply the provided deprecation details to
isLegacy                | Request body | boolean          | no       | If set to true, mark the package version(s) as legacy, defaults to false
hasCriticalBugs         | Request body | boolean          | no       | If set to true, mark the package version(s) as having critical bugs, defaults to false
isOther                 | Request body | boolean          | no       | If set to true, mark the package version(s) as having some other deprecation reason, defaults to false
alternatePackageId      | Request body | string           | no       | An alternate package ID to refer package consumer to, in lieu of this package, required if `alternatePackageVersion` is specified 
alternatePackageVersion | Request body | string           | no       | A specific version for the alternate package ID
message                 | Request body | string           | no       | A user-facing, plain text message to include with the other deprecation details, required if `isOther` is true
listedVerb              | Request body | string           | no       | Changes the listed status with `Unlist` (mark all specified versions as unlisted), `Relist` (mark all specified versions as listed), or `Unchanged` (leave listed status as-is), defaults to `Unchanged`

### Response

Status Code | Meaning
----------- | -------
200         | The package deprecation information has been successfully modified
400         | The request is invalid, check the HTTP reason phrase for details
404         | No package with the provided `ID`, `versions`, or alternate package does not exist

### Sample request

This request marks NuGet.Core 2.12.0 as deprecated with the "Legacy" reason. The NuGet.Protocol 6.4.0 package is used as the alternate package.

```
PUT https://www.nuget.org/api/v2/package/NuGet.Core/deprecations
User-Agent: Spoon-Knife/1.0.0
X-NuGet-ApiKey: {USER_API_KEY}

{
    "versions": [ "2.12.0" ],
    "isLegacy": true,
    "alternatePackageId": "NuGet.Protocol",
    "alternatePackageVersion": "6.4.0",
    "message": "NuGet.Core has been replaced by NuGet client v3 and later APIs."
}
```

### Sample request

This request marks NuGet.Core 2.12.0, 2.13.0, and 2.14.0 as deprecated with the "Legacy" and "Other" reasons displaying the provided message. All three versions will be unlisted at the same time as being marked as deprecated.

```
PUT https://www.nuget.org/api/v2/package/NuGet.Core/deprecations
User-Agent: Spoon-Knife/1.0.0
X-NuGet-ApiKey: {USER_API_KEY}

{
    "versions": [ "2.12.0", "2.13.0", "2.14.0" ],
    "isLegacy": true,
    "isOther": true
    "message": "NuGet.Core has been replaced by NuGet client v3 and later APIs.",
    "listedVerb": "Unlist"
}
```
