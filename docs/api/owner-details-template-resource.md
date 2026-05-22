---
title: Owner URL Template, NuGet API
description: The Owner URL template allows clients to display in their UI a web link
author: martinrrm
ms.author: mruizmares
ms.date: 4/15/2024
ms.topic: reference
ms.reviewer:
---

# Owner URL template

It is possible for a client to build a URL that can be used by the user to see owner's page in their web browser. 
This is useful when a package source wants to show additional information about an owner that may not fit within the scope of what the NuGet client application shows.

The resource used for building this URL is the `OwnerDetailsUriTemplate` resource found in the
[service index](service-index.md).

## Versioning

The following `@type` values are used:

@type value                     | Notes
------------------------------- | -----
OwnerDetailsUriTemplate/6.11.0  | The initial release

## URL template

The URL for the following API is the value of the `@id` property associated with one of the aforementioned resource `@type` values.

## HTTP methods

Although the client is not intended to make requests to the owner URL on behalf of the user, the web page should support the `GET` method to allow a clicked URL to be easily opened in a web browser.

## Construct the URL

Given a known owner ID, the client implementation can construct a URL used to access a web interface.
The client implementation should display this constructed URL (or clickable link) to the user allowing them to open a web browser to the URL and to learn more about the owner.
The contents of the owner page is determined by the server implementation.

The URL must be an absolute URL and the scheme (protocol) must be HTTPS.

The value of the `@id` in the service index is a URL string containing any of the following placeholder tokens:

### URL placeholders

Name        | Type    | Required | Notes
----------- | ------- | -------- | -----
`{owner}`   | string  | no       | The owner ID to get details for

The casing requirements of {owner} should be defined by the package source.
The client should retain the casing of the username in whatever form it was found in other API responses or in the package repository signature.

NuGet.org treats owner usernames in a case insensitive manner but not all package source may have this flexibility.

For example, nuget.org's package details template looks like this:

```http
https://www.nuget.org/profile/{owner}
```

If the client implementation needs to display a link to the package details for NuGet.Versioning 4.3.0, it would produce the following URL and provide it to the user:

```http
https://www.nuget.org/profiles/nuget
```
