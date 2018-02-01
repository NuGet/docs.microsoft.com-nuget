---
title: Service Index, NuGet API | Microsoft Docs
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
description: The service index is the entry point of the NuGet HTTP API and enumerates the capabilities of the server.
keywords: NuGet API entry point, NuGetA PI endpoint discovery
ms.reviewer:
- karann
- unnir
---

# Service index

The service index is a JSON document that is the entry point for a NuGet package source and allows a client
implementation to discover the package source's capabilities. The service index is a JSON object with two required
properties: `version` (the schema version of the service index) and `resources`  (the endpoints or capabilities of the
package source).

nuget.org's service index is located at `https://api.nuget.org/v3/index.json`.

## Versioning

The `version` value is a SemVer 2.0.0 parseable version string which indicates the schema version of the service index.
The API mandates that the version string has a major version number of `3`. As non-breaking changes are made to the
service index schema, the version string's minor version will be increased.

Each resource in the service index is versioned independently from the service index schema version.

The current schema version is `3.0.0-beta.1`.

## HTTP methods

The service index is accessible using HTTP methods `GET` and `HEAD`.

## Resources

The `resources` property contains an array of resources supported by this package source.

### Resource

A resource is an object in the `resources` array. It represents a versioned capability of a package source. A
resource has the following properties:

Name          | Type   | Required | Notes
------------- | ------ | -------- | -----
@id           | string | yes      | The URL to the resource
@type         | string | yes      | A string constant representing the resource type
clientVersion | string | no       | The client version that this resource applies to
comment       | string | no       | A human readable description of the resource

The `@id` is a URL that must be absolute and must either have the HTTP or HTTPS schema.

The `@type` is used to identify the specific protocol to use when interacting with resource. The type of the resource
is an opaque string but generally has the format:

    {RESOURCE_NAME}/{RESOURCE_VERSION}

Clients are expected to hard code the `@type` values that they understand and look them up in a package source's
service index. The exact `@type` values in use today are enumerated on the individual resource reference documents
listed in the [API overview](overview.md#resources-and-schema).

For the sake of this documentation, the documentation about different resources will essentially be grouped by the
`{RESOURCE_NAME}` found in the service index which is analogous to grouping by scenario. 

There is no requirement that each resource has a unique `@id` or `@type`. It is up to the client implementation to
determine which resource to prefer over another. One possible implementation is that resources of the same or
compatible `@type` can be used in a round-robin fashion in case of connection failure or server error.

### Sample request

GET https://api.nuget.org/v3/index.json

### Sample response

[!code-JSON [service-index.json](./_data/service-index.json)]

### Client version

A resource `@type` can optionally have the following form:

```
{RESOURCE_NAME}/Versioned
```

When the `@type` is suffixed with the `/Versioned` string, an additional property is required on the resource:
`clientVersion`.

This value is the minimum client version compatible with this resource. The client version is SemVer 2.0.0 compliant
version string. A client implementation knows its own version (also a SemVer 2.0.0 version) and compares it to the
resource `clientVersion` value using standard SemVer 2.0.0 comparison rules. If the client's own version is greater
than or equal to the resource `clientVersion` and if the `{RESOURCE_NAME}` is known, this resource should be considered
consumable by the client implementation.

For this `clientVersion` model to work, there needs to be some agreement between the server and client implementors
about what protocol each `{RESOURCE_NAME}` and `clientVersion` combination implies.

This feature allows server implementations to backport resources to clients that have already shipped. When no
`clientVersion` is specified on the resource, there is no way for a client that is already shipped to discover the
resource since the `@type` values that client knows about are hard coded.

> [!Note]
> For nuget.org, the client versions in the service index align with the official NuGet client.

> [!Note]
> This client version feature was first supported by version 4.0.0 of the official NuGet client.
