---
title: Service Index, NuGet API
description: The service index is the entry point of the NuGet HTTP API and enumerates the capabilities of the server.
author: joelverhagen
ms.author: jver
ms.date: 10/26/2017
ms.topic: reference
ms.reviewer: kraigb
---

# Service index

The service index is a JSON document that is the entry point for a NuGet package source and allows a client
implementation to discover the package source's capabilities. The service index is a JSON object with two required
properties: `version` (the schema version of the service index) and `resources`  (the endpoints or capabilities of the
package source).

nuget.org's service index is located at `https://api.nuget.org/v3/index.json`.

## Versioning

The `version` value is a SemVer 2.0.0 parseable version string which indicates the schema version of the service index. The API mandates that the version string has a major version number of `3`. As non-breaking changes are made to the service index schema, the version string's minor version will be increased.

Each resource in the service index is versioned independently from the service index schema version.

The current schema version is `3.0.0`. The `3.0.0` version is functionally equivalent to the older `3.0.0-beta.1`
version but should be preferred as it more clearly communicates the stable, defined schema.

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
comment       | string | no       | A human readable description of the resource

The `@id` is a URL that must be absolute and must either have the HTTP or HTTPS schema.

The `@type` is used to identify the specific protocol to use when interacting with resource. The type of the resource
is an opaque string but generally has the format:

```
{RESOURCE_NAME}/{RESOURCE_VERSION}
```

Clients are expected to hard code the `@type` values that they understand and look them up in a package source's
service index. The exact `@type` values in use today are enumerated on the individual resource reference documents
listed in the [API overview](overview.md#resources-and-schema).

For the sake of this documentation, the documentation about different resources will essentially be grouped by the
`{RESOURCE_NAME}` found in the service index which is analogous to grouping by scenario. 

There is no requirement that each resource has a unique `@id` or `@type`. It is up to the client implementation to
determine which resource to prefer over another. One possible implementation is that resources of the same or
compatible `@type` can be used in a round-robin fashion in case of connection failure or server error.

A resource can use a different host or domain than the service index, but this may cause issues in environments with strict network rules.
If you delegate resources to api.nuget.org, your feed will not work where access to nuget.org is blocked.
We recommend configuration to enable or disable features that use api.nuget.org.

### Sample request

```
GET https://api.nuget.org/v3/index.json
```

### Sample response

[!code-JSON [service-index.json](./_data/service-index.json)]
