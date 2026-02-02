---
title: nuget.org Protocols
description: The evolving nuget.org protocols to interact with NuGet clients.
author: anangaur
ms.author: anangaur
ms.date: 01/21/2021
ms.topic: reference
ms.reviewer: kraigb
---

# nuget.org protocols

To interact with nuget.org, clients need to follow certain protocols. Because these protocols keep evolving, clients
must identify the protocol version they use when calling specific nuget.org APIs. This allows nuget.org to introduce
changes in a non-breaking way for the old clients.

> [!Note]
> The APIs documented on this page are specific to nuget.org and there is no expectation for other NuGet server
> implementations to introduce these APIs. 

For information about the NuGet API implemented broadly across the NuGet ecosystem, see the
[API overview](overview.md).

This topic lists various protocols as and when they come to existence.

## NuGet protocol version 4.1.0

The 4.1.0 protocol specifies usage of verify-scope keys to interact with services other than nuget.org, to validate a
package against a nuget.org account. Note that the `4.1.0` version number is an opaque string but happens to coincide
with the first version of the official NuGet client that supported this protocol.

Validation ensures that the user-created API keys are used only with nuget.org, and that other verification or
validation from a third-party service is handled through a one-time use verify-scope keys. These verify-scope keys can
be used to validate that the package belongs to a particular user (account) on nuget.org.

### Client requirement

Clients are required to pass the following header when they make API calls to **push** packages to nuget.org:

```
X-NuGet-Protocol-Version: 4.1.0
```

Note that the `X-NuGet-Client-Version` header has similar semantics but is reserved to only be used by the official
NuGet client. Third party clients should use the `X-NuGet-Protocol-Version` header and value.

The **push** protocol itself is described in the documentation for the
[`PackagePublish` resource](package-publish-resource.md).

If a client interacts with external services and needs to validate whether a package belongs to a particular user
(account), it should use the following protocol and use the verify-scope keys and not the API keys from nuget.org.

### API to request a verify-scope key

This API is used to get a verify-scope key for a nuget.org author to validate a package owned by him/her.

```
POST api/v2/package/create-verification-key/{ID}/{VERSION}
```

#### Request parameters

Name           | In     | Type   | Required | Notes
-------------- | ------ | ------ | -------- | -----
ID             | URL    | string | yes      | The package identidier for which the verify scope key is requested
VERSION        | URL    | string | no       | The package version
X-NuGet-ApiKey | Header | string | yes      | For example, `X-NuGet-ApiKey: {USER_API_KEY}`

#### Response

```json
{
    "Key": "{Verify scope key from nuget.org}",
    "Expires": "{Date}"
}
```

### API to verify the verify scope key

This API is used to validate a verify-scope key for package owned by the nuget.org author.

```
GET api/v2/verifykey/{ID}/{VERSION}
```

#### Request parameters

Name           | In     | Type   | Required | Notes
-------------  | ------ | ------ | -------- | -----
ID             | URL    | string | yes      | The package identifier for which the verify scope key is requested
VERSION        | URL    | string | no       | The package version
X-NuGet-ApiKey | Header | string | yes      | For example, `X-NuGet-ApiKey: {VERIFY_SCOPE_KEY}`

> [!Note]
> This verify scope API key expires in a day's time or on first use, whichever occurs first.

#### Response

Status Code | Meaning
----------- | -------
200         | The API key is valid
403         | The API key is invalid or not authorized to push against the package
404         | The package referred to by `ID` and `VERSION` (optional) does not exist
