---
title: Rate Limits, NuGet API
description: The NuGet APIs will have enforced rate limits to prevent abuse.
author: cmanu
ms.author: cmanu
ms.date: 03/20/2018
ms.topic: reference
ms.reviewer: 
  - skofman
  - anangaur
  - kraigb
---

# Rate Limits

The NuGet.org API enforces rate limiting to prevent abuse. Requests that exceed the rate limit return the following error: 

  ~~~
    {
      "statusCode": 429,
      "message": "Rate limit is exceeded. Try again in 56 seconds."
    }
  ~~~

In addition to request throttling using rate limits, some APIs also enforce quota. Requests that exceed the quota return the following error:

  ~~~
    {
      "statusCode": 403,
      "message": "Quota exceeded."
    }
  ~~~

The following tables list the rate limits for the NuGet.org API.

## Package search

> [!Note]
> We recommend using NuGet.org's [V3 search APIs](search-query-service-resource.md) as it is not rate limited currently. For V1 and V2 search APIs, the following limits apply:

| API | Limit Type | Limit Value | API Use Case |
|:---|:---|:---|:---|
**GET** `/api/v1/Packages` | IP | 1000 / minute | Query NuGet package metadata via v1 OData `Packages` collection |
**GET** `/api/v1/Search()` | IP | 3000 / minute | Search for NuGet packages via v1 Search endpoint | 
**GET** `/api/v2/Packages` | IP | 20000 / minute | Query NuGet package metadata via v2 OData `Packages` collection | 
**GET** `/api/v2/Packages/$count` | IP | 100 / minute | Query NuGet package count via v2 OData `Packages` collection | 

## Package Push, unlist, relist, and set deprecation details

| API | Limit Type | Limit Value | API Use Case | 
|:---|:---|:---|:--- |
**PUT** `/api/v2/package` | API Key | 350 / hour | Upload a new NuGet package (version) via package publish endpoint 
**DELETE** `/api/v2/package/{id}/{version}` | API Key | 250 / hour | Unlist a NuGet package (version) via package publish endpoint 
**POST** `/api/v2/package/{id}/{version}` | API Key | 250 / hour | Relist a NuGet package (version) via package publish endpoint
**PUT** `/api/v2/package/{id}/deprecations` on multiple version | API Key | 1 / minute | Update the deprecation information on multiple package versions

## nuget.org website page views

If you are accessing the nuget.org web pages programmatically, consider investigating our documented [V3 APIs](overview.md). These endpoints allow for simpler access to package metadata and content. The V3 API has better availability and has higher performance than accessing the NuGet Gallery web pages, which are designed for web browser interaction.

| API | Limit Type | Limit Value | API Use Case | 
|:---|:---|:---|:--- |
**GET** `/package/{id}/{version}` | IP | 50 / minute | Display package (version) details page. 
