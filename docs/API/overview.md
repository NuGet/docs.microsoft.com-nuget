---
# required metadata 

title: Overview, NuGet API | Microsoft Docs
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
ms.assetid: 8c81f1ac-18c7-44d1-b2e3-584fe85dee6f

# optional metadata

description: The NuGet API is a set of HTTP endpoints that can be used to download packages, fetch metadata, publish new packages, etc.
keywords: NuGet V3 API, NuGet V2 API, NuGet JSON, NuGet registration API, NuGet API flat container, NuGet nupkg API, NuGet metadata API, NuGet search API, NuGet push API, NuGe publish API, NuGet delete API, NuGet unlist API, NuGet protocol
ms.reviewer:
- karann
- unniravindranathan

---

# NuGet API

The NuGet API is a set of HTTP endpoints that can be used to download packages, fetch metadata, publish new packages,
and perform most other operations available in the official NuGet clients.

This API is used by the NuGet client in Visual Studio, nuget.exe, and the .NET CLI to perform NuGet operations such as
[`dotnet restore`](/dotnet/articles/core/preview3/tools/dotnet-restore), search in the Visual
Studio UI, and [`nuget.exe push`](../tools/cli-ref-push.md).

Note in some cases, nuget.org has additional requirements that are not enforced by other package sources. These
differences are documented by the [nuget.org Protocols](nuget-protocols.md).

## Service index

The entry point for the API is a JSON document in a well known location. This document is called the **service index**.
The location of the service index for nuget.org is `https://api.nuget.org/v3/index.json`.

This JSON document contains a list of *resources* which provide different functionality and fulfill different
use cases.

Clients that support the API should accept one or more of these service index URL as the means of connecting to the
respective package sources.

For more information about the service index, see [its API reference](service-index.md).

## Versioning

The API is version 3 of NuGet's HTTP protocol. This protocol is sometimes referred to as "the V3 API." These reference
documents will refer to this version of the protocol simply as "the API."

The service index schema version is indicated by the `version` property in the service index. The API mandates that
the version string has a major version number of `3`. As non-breaking changes are made to the service index schema, the
version string's minor version will be increased.

Older clients (such as nuget.exe 2.x) do not support the V3 API and only support the older V2 API, which is not
documented here.

The NuGet V3 API is named as such because it's the successor of the V2 API, which was the OData-based protocol
implemented by the 2.x version of the official NuGet client. The V3 API was first supported by the 3.0 version of the
official NuGet client and is still the latest major protocol version supported by the NuGet client, 4.0 and on. 

Non-breaking protocol changes have been made to the API since it was first release.

## Resources and schema

The **service index** describes a variety of resources. The current set of supported resources are as follows:

Resource name                                                          | Required | Description
---------------------------------------------------------------------- | -------- | -----------
[`PackagePublish`](package-publish-resource.md)                        | yes      | Push and delete (or unlist) packages.
[`SearchQueryService`](search-query-service-resource.md)               | yes      | Filter and search for packages by keyword.
[`RegistrationsBaseUrl`](registration-base-url-resource.md)            | yes      | Get package metadata.
[`PackageBaseAddress`](package-base-address-resource.md)               | yes      | Get package content (.nupkg).
[`SearchAutocompleteService`](search-autocomplete-service-resource.md) | no       | Discover package IDs and versions by substring.
[`ReportAbuseUriTemplate`](report-abuse-resource.md)                   | no       | Construct a URL to access a "report abuse" web page.
[`Catalog`](catalog-resource.md)                                       | no       | Full record of all package events.

In general, all non-binary data returned by a API resource are serialized using JSON. The response schema
returned by each resource in the service index is defined individually for that resource. For more information about
each resource, see the topics listed above.

> [!Note]
> When a source does not implement `SearchAutocompleteService` any autocomplete behavior should be disabled
> gracefully. When `ReportAbuseUriTemplate` is not implemented, the official NuGet client falls back to nuget.org's
> report abuse URL (tracked by [NuGet/Home#4924](https://github.com/NuGet/Home/issues/4924)). Other clients may opt
> to simply not show a report abuse URL to the user.

## Timestamps

All timestamps returned by the API are UTC or are otherwise specified using
[ISO 8601](https://www.iso.org/iso-8601-date-and-time-format.html) representation. 

## HTTP methods

Verb   | Use
------ | -----------
GET    | Performs a read-only operation, typically retrieving data.
HEAD   | Fetches the response headers for the corresponding `GET` request.
PUT    | Creates a resource that doesn't exist or, if it does exist, updates it. Some resources may not support update.
DELETE | Deletes or unlists a resource.

## HTTP status codes

Code | Description
---- | -----
200  | Success, and there is a response body.
201  | Success, and the resource was created.
202  | Success, the request has been accepted but some work may still be incomplete and completed asynchronously.
204  | Success, but there is no response body.
301  | A permanent redirect.
302  | A temporary redirect.
400  | The parameters in the URL or in the request body aren't valid.
401  | The provided credentials are invalid.
403  | The action is not allowed given the provided credentials.
404  | The requested resource doesn't exist.
409  | The request conflicts with an existing resource.
500  | The service has encountered an unexpected error.
503  | The service is temporarily unavailable.

Any `GET` request made to a API endpoint may return an HTTP redirect (301 or 302). Clients should gracefully handle
such redirects by observing the `Location` header and issueing a subsequent `GET`. Documentation concerning specific
endpoints will not explicitly call out where redirects may be used.

In the case of a 500-level status code, the client can implement a reasonable retry mechanism. The official NuGet
client retries three times when encountering any 500-level status code or TCP/DNS error.

## HTTP request headers

Name                     | Description
------------------------ | -----------
X-NuGet-ApiKey           | Required for push and delete, see [`PackagePublish` resource](package-publish-resource.md)
X-NuGet-Client-Version   | **Deprecated** and replaced by `X-NuGet-Protocol-Version`
X-NuGet-Protocol-Version | Required in certain cases only on nuget.org, see [nuget.org protocols](NuGet-Protocols.md)

## Authentication

Authentication is left up to the package source implementation to define. For nuget.org, only the `PackagePublish`
resource requires authentication via a special API key header. See
[`PackagePublish` resource](package-publish-resource.md) for details.
