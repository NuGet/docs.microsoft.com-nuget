---
title: Owner Details URL Template, NuGet API
description: The owner details URL template allows clients to display a link to a package owner's profile page in their UI.
author: donnie-msft
ms.author: eagoodso
ms.date: 07/09/2026
ms.topic: reference
ai-usage: ai-generated
---

# Owner details URL template

It is possible for a client to build a URL that can be used by the user to see a package owner's profile page in their web browser.
This is useful when a package source wants to enable all client experiences (even third-party) to link to the profile page of a specific package owner.

The resource used for building this URL is the `OwnerDetailsUriTemplate` resource found in the
[service index](service-index.md).

## Versioning

The following `@type` values are used:

@type value                      | Notes
-------------------------------- | -----
OwnerDetailsUriTemplate/6.11.0   | The initial release

## URL template

The URL for the following API is the value of the `@id` property associated with one of the aforementioned resource `@type` values.

For example, nuget.org's owner details template looks like this:

```http
https://www.nuget.org/profiles/{owner}?_src=template
```

## HTTP methods

Although the client is not intended to make requests to the owner details URL on behalf of the user, the web page should support the `GET` method to allow a clicked URL to be easily opened in a web browser.

## Construct the URL

Given a known owner username, the client implementation can construct a URL used to access a web interface.
The client implementation should display this constructed URL (or clickable link) to the user allowing them to open a web browser to the URL and to learn more about the owner.
The contents of the owner details page is determined by the server implementation.

The URL must be an absolute URL and the scheme (protocol) must be HTTPS.

The value of the `@id` in the service index is a URL string containing the following placeholder token:

### URL placeholders

Name      | Type   | Required | Notes
--------- | ------ | -------- | -----
`{owner}` | string | yes      | The owner username to get details for

The client should retain the casing of the username in whatever form it was found in other API responses or in the package repository signature.
NuGet.org treats owner usernames in a case insensitive manner, but not all package sources may have this flexibility.

For example, if the client implementation needs to display a link to the owner details page for the owner `Microsoft`, it would produce the following URL and provide it to the user:

```http
https://www.nuget.org/profiles/Microsoft
```
