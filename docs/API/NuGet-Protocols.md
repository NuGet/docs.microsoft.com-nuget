---
# required metadata 

title: NuGet Protocols | Microsoft Docs
author: anangaur
ms.author: anangaur
manager: unnir
ms.date: 10/3/2017
ms.topic: article
ms.prod: nuget
#ms.service:
ms.technology: null

ms.assetid: 91c3d1f7-d8fb-4967-9d0f-99a0248d89a5

# optional metadata

description: This documents the evolving NuGet.org protocols to interact with NuGet.org by the NuGet clients.
#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer:
- kraigb
- karann
#ms.suite:
#ms.tgt_pltfrm:
#ms.custom:
---

In order to interact with NuGet.org, clients need to follow certain protocols. Since these protocols keep evolving, it is required for the clients to identify the protocol version they use when they call NuGet.org APIs. This allows NuGet.org to introduce changes in a non-breaking way for the old clients. 

This document lists out various protocols as and when they come to existence.

## NuGet protocol version 4.1.0

### Protocol summary
This protocol speicifes usage of temporary verify-scope keys to interact with services other than NuGet.org, to validate a package against a NuGet.org account. 

This ensures that the user created API keys are used only used with NuGet.org and other verification/validation from a third-party service is handled through a one-time use verify-scope keys. These verify-scope keys can be used to validate that the package belongs to a particular user (account) on NuGet.org.

### Client requirement

If a client interacts with other services and needs to validate whether a package belongs to a particular user (account), it should use this protocol and use the verify scope keys and not the API keys from NuGet.org.

Clients are required to pass the following header when they make API calls to **push** packages to NuGet.org:

```
X-NuGet-Protocol-Version: 4.1.0
```

### Protocol usage details

#### API to request a verify scope key
This API is used to get a verify-scope key for a NuGet.org author to validate a package owned by him/her.

```
POST api/v2/package/create-verification-key/{id}/{version}
```
Request parameters

| Name | In | Type | Notes|
|:------------- |:-------------|:----|:-----|
| id    | URL | string | Required. This is the package id for which verify scope key is requested for|
| version | URL | string | Optional. Package version|
| X-NuGet-ApiKey | Header | string | Required. Eg. X-NuGet-ApiKey={USER_API_KEY} |

Response
```
Response:
{
    "Key": "{Verify scope key from NuGet.org}",
    "Expires": "{Date}"
}

```

#### API to verify the verify scope key

This API is used to validate a verify-scope key for package owned by the NuGet.org author

```
GET api/v2/package/verifykey/{id}/{version}
```

Request parameters

| Name | In | Type | Notes|
|:------------- |:-------------|:----|:-----|
| id    | URL | string | Required: This is the package id for which verify scope key is requested for|
| version | URL | string | Optional: Package version|
| X-NuGet-ApiKey | Header | string | Required. Eg. X-NuGet-ApiKey={VERIFY_SCOPE_KEY} |

!Note
> This verify scope API key expires in a day's time or on first use, whichever occurs first.


