---
title: Rate Limits | Microsoft Docs
author:
- cmanu
ms.author:
- cmanu
manager: skofman
ms.date: 03/20/2018
ms.topic: reference
ms.prod: nuget
ms.technology: null
description: The NuGet APIs will have enforced rate limits to prevent abuse.
keywords: NuGet API, rate limiting
ms.reviewer:
- skofman
- anangaur
ms.workload: 
 - "dotnet"
 - "aspnet"
---

# API Rate Limits

The NuGet.org API enforces rate limiting to prevent abuse. Requests that exceed the rate limit return the following error: 

  ~~~
    {
      "statusCode": 429,
      "message": "Rate limit is exceeded. Try again in 56 seconds."
    }
  ~~~

The following tables list the rate limits for the NuGet.org API.

## V1 OData API

For details on the following APIs, refer to the the [package search documentation](https://docs.microsoft.com/nuget/api/search-query-service-resource)

Api | Throttling Type | Throttling Values | Main use case | V3 APIs support
:-- |:-- |:-- |:-- | --
**GET** `/api/v1/Packages` | IP | 1000 / minute | Query NuGet package metadata via v1 OData `Packages` collection |
**GET** `/api/v1/Search()` | IP | 3000 / minute | Search for NuGet packages via v1 Search endpoint | 

## V2 OData API

| Api | Throttling Type | Throttling Values | Main use case 
|:--- |:--- |:--- | -- |
**GET** `/api/v2/Packages` | IP | 20000 / minute | Query NuGet package metadata via v2 OData `Packages` collection | 
**GET** `/api/v2/Packages/$count` | IP | 100 / minute | Query NuGet package count via v2 OData `Packages` collection | 

## V2 API

Api | Throttling Type | Throttling Values | Main use case 
-- | -- | -- | -- 
**PUT** `/api/v2/package` | API Key | 100 / minute | Upload a new NuGet package (version) via v2 push endpoint 
**DELETE** `/api/v2/package/{id}/{version}` | API Key | 100 / minute | Unlist a NuGet package (version) via v2 endpoint 
