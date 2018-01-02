---
# required metadata 

title: Push and Delete, NuGet API | Microsoft Docs
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
ms.assetid: 1eaa403a-5c13-4c05-9352-2f791b98aa7e

# optional metadata

description: The publish service allows clients to publish new packages and unlist or delete existing packages.
keywords: NuGet API push package, NuGet API delete package, NuGet API unlist package, NuGet API upload package, NuGet API create package
ms.reviewer:
- karann
- unniravindranathan

---

# Push and Delete

It is possible to push and delete (or unlist, depending on the server implementation) packages using the NuGet V3 API.
Both operations are based off of the `PackagePublish` resource found in the [service index](service-index.md).

## Versioning

The following `@type` value is used:

@type value          | Notes
-------------------- | -----
PackagePublish/2.0.0 | The initial release

## Base URL

The base URL for the following APIs is the value of the `@id` property of the `PackagePublish/2.0.0` resource in the
package source's [service index](service-index.md). For the documentation below, nuget.org's URL is used. Consider 
`https://www.nuget.org/api/v2/package` as a placeholder for the `@id` value found in the service index.

Note that this URL points to the same location as the legacy V2 push endpoint since the protocol is the same.

## HTTP methods

The `PUT` and `DELETE` HTTP methods are supported by this resource. For which methods are supported on each endpoint,
see below.

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

## Delete a package

nuget.org interprets the package delete request as an "unlist". This means that the package is still available for
existing consumers of the package but the package no longer appears in search results or in the web interface. For
more information about this practice, see the
[Deleted Packages](../policies/deleting-packages.md) policy. Other server
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
