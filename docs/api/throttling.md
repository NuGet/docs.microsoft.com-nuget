The NuGet.org service API needs some configurable and enforceable upper limits on acceptable API usage. 

Initial planned throttling limits are based on the maximum load we've seen on these endpoints to date. Upon reaching the maximum allowed requests a 429 (too many messages) error code will be returned. 

# V1 OData API

Throttling Type: **based on IP**

Api | Throttling Values | Main use case | V3 APIs support
-- | -- | -- | --
**GET** `/api/v1/Packages` | 1000 / minute | Query NuGet package metadata via v1 OData `Packages` collection | [package search documentation](https://docs.microsoft.com/en-us/nuget/api/search-query-service-resource)
**GET** `/api/v1/Search()?searchTerm=&targetFramework=&includePrerelease=` | 3000 / minute | Search for NuGet packages via v1 Search endpoint | [package search documentation](https://docs.microsoft.com/en-us/nuget/api/search-query-service-resource)

# V2 OData API

Throttling Type: **based on IP**

Api | Throttling Values | Main use case | V3 APIs support
-- | -- | -- | --
**GET** `/api/v2/Packages` | 20000 / minute | Query NuGet package metadata via v2 OData `Packages` collection | [package search documentation](https://docs.microsoft.com/en-us/nuget/api/search-query-service-resource)
**GET** `/api/v2/Packages/$count` | 100 / minute | Query NuGet package count via v2 OData `Packages` collection | 
**PUT** `/api/v2/package` | 100 / minute | Upload a new NuGet package (version) via v2 push endpoint | 
**DELETE** `/api/v2/package/{id}/{version}` | 100 / minute | Unlist a NuGet package (version) via v2 endpoint |
