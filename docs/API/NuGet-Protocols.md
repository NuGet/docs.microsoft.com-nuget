---
# required metadata 

title: NuGet Protocols | Microsoft Docs
author: anangaur
ms.author: anangaur
manager: unnir
ms.date: 10/3/2017
ms.topic: article
ms.prod: nuget
ms.technology: null
ms.assetid: ba1d9742-9f1c-42ff-8c30-8e953e23c501

# optional metadata

description: The evolving nuget.org protocols to interact with nuget.org by NuGet clients.
ms.reviewer:
- kraigb
- karann

---
# NuGet Protocols

To interact with nuget.org, clients need to follow certain protocols. Because these protocols keep evolving, clients must identify the protocol version they use when calling nuget.org APIs. This allows nuget.org to introduce changes in a non-breaking way for the old clients. 

This topic lists various protocols as and when they come to existence.

## NuGet protocol version 4.1.0

This protocol specifies usage of verify-scope keys to interact with services other than nuget.org, to validate a package against a nuget.org account. 

Validation ensures that the user-created API keys are used only with nuget.org, and that other verification or validation from a third-party service is handled through a one-time use verify-scope keys. These verify-scope keys can be used to validate that the package belongs to a particular user (account) on nuget.org.

### Client requirement

If a client interacts with other services and needs to validate whether a package belongs to a particular user (account), it should use this protocol and use the verify scope keys and not the API keys from nuget.org.

Clients are required to pass the following header when they make API calls to **push** packages to nuget.org:

```
X-NuGet-Protocol-Version: 4.1.0
```

### Protocol usage details

#### API to request a verify scope key
This API is used to get a verify-scope key for a nuget.org author to validate a package owned by him/her.

```
POST api/v2/package/create-verification-key/{id}/{version}
```

Request parameters:

| Name | In | Type | Notes|
|:------------- |:-------------|:----|:-----|
| id    | URL | string | Required: the package identidier for which theverify scope key is requested. |
| version | URL | string | Optional: package version|
| X-NuGet-ApiKey | Header | string | Required: for example, `X-NuGet-ApiKey={USER_API_KEY}` |

Response:

```
Response:
{
    "Key": "{Verify scope key from nuget.org}",
    "Expires": "{Date}"
}

```

#### API to verify the verify scope key

This API is used to validate a verify-scope key for package owned by the nuget.org author

```
GET api/v2/package/verifykey/{id}/{version}
```

Request parameters:

| Name | In | Type | Notes|
|:------------- |:-------------|:----|:-----|
| id | URL | string | Required: the package identifier for which the verify scope key is requested. |
| version | URL | string | Optional: package version |
| X-NuGet-ApiKey | Header | string | Required: for example, `X-NuGet-ApiKey={VERIFY_SCOPE_KEY}` |

> [!Note]
> This verify scope API key expires in a day's time or on first use, whichever occurs first.
