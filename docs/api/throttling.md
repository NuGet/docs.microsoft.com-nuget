# NuGet API rate limiting

I order to prevent the extreme overload on NuGet Server resources the NuGet.org service API needs some configurable and enforceable upper limits on acceptable API usage. 

Initial planned throttling limits are based on the maximum load we've seen on these endpoints to date. Upon reaching the maximum allowed requests an error response returns. 

    {
      "statusCode": 429,
      "message": "Rate limit is exceeded. Try again in 56 seconds."
    }



# V1 OData API

Api | Throttling Type | Throttling Values | Main use case | V3 APIs support
-- | -- | -- | -- | --
**GET** `/api/v1/Packages` | IP | 1000 / minute | Query NuGet package metadata via v1 OData `Packages` collection | [package search documentation](https://docs.microsoft.com/en-us/nuget/api/search-query-service-resource)
**GET** `/api/v1/Search()?searchTerm=&targetFramework=&includePrerelease=` | IP | 3000 / minute | Search for NuGet packages via v1 Search endpoint | [package search documentation](https://docs.microsoft.com/en-us/nuget/api/search-query-service-resource)

# V2 OData API

Api | Throttling Type | Throttling Values | Main use case | V3 APIs support
-- | -- | -- | -- | --
**GET** `/api/v2/Packages` | IP | 20000 / minute | Query NuGet package metadata via v2 OData `Packages` collection | [package search documentation](https://docs.microsoft.com/en-us/nuget/api/search-query-service-resource)
**GET** `/api/v2/Packages/$count` | IP | 100 / minute | Query NuGet package count via v2 OData `Packages` collection | 

# V2 API

Api | Throttling Type | Throttling Values | Main use case 
-- | -- | -- | -- 
**PUT** `/api/v2/package` | API Key | 100 / minute | Upload a new NuGet package (version) via v2 push endpoint 
**DELETE** `/api/v2/package/{id}/{version}` | API Key | 100 / minute | Unlist a NuGet package (version) via v2 endpoint 
