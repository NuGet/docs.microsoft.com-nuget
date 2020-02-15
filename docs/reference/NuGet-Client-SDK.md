---
title: NuGet Client SDK
description: The API is evolving and not yet documented, but examples are available on Dave Glick's blog.
author: karann-msft
ms.author: karann
ms.date: 01/09/2018
ms.topic: conceptual
---

# NuGet Client SDK

The *NuGet Client SDK* refers to a group of NuGet packages centered around the [NuGet.Protocol](https://www.nuget.org/packages/NuGet.Protocol) and [NuGet.Packaging](https://www.nuget.org/packages/NuGet.Packaging) packages from the [NuGet/NuGet.Client](https://github.com/NuGet/NuGet.Client) GitHub repository.

> [!Note]
>  For documentation on the NuGet server protocol, please refer to the [NuGet Server API](~/api/overview.md).

## Getting started

### Install the package

```
dotnet add NuGet.Protocol
```

## Examples

You can find these examples on the [NuGet.Protocol.Samples](TODO) project on GitHub.

### List package versions

Find all versions of Newtonsoft.Json using the [NuGet V3 Package Content API](../api/package-base-address-resource.md#enumerate-package-versions):

[!code-csharp[ListPackageVersions](~/../nuget-samples/NuGetProtocolSamples/Program.cs?name=ListPackageVersions)]

[!code-csharp[DownloadPackage](~/../nuget-samples/NuGetProtocolSamples/Program.cs?name=DownloadPackage)]

```csharp
ILogger logger = NullLogger.Instance;
CancellationToken cancellationToken = CancellationToken.None;

SourceCacheContext cache = new SourceCacheContext();
SourceRepository repository = Repository.Factory.GetCoreV3("https://api.nuget.org/v3/index.json");
FindPackageByIdResource resource = await repository.GetResourceAsync<FindPackageByIdResource>();

IEnumerable<NuGetVersion> versions = await resource.GetAllVersionsAsync(
    "Newtonsoft.Json",
    cache,
    logger,
    cancellationToken);

foreach (NuGetVersion version in versions)
{
    Console.WriteLine($"Found version {version}");
}
```

### Download a package

Download Newtonsoft.Json v12.0.1 using the [NuGet V3 Package Content API](../api/package-base-address-resource.md):

```csharp
ILogger logger = NullLogger.Instance;
CancellationToken cancellationToken = CancellationToken.None;

SourceCacheContext cache = new SourceCacheContext();
SourceRepository repository = Repository.Factory.GetCoreV3("https://api.nuget.org/v3/index.json");
FindPackageByIdResource resource = await repository.GetResourceAsync<FindPackageByIdResource>();

string packageId = "Newtonsoft.Json";
NuGetVersion packageVersion = new NuGetVersion("12.0.1");
using MemoryStream packageStream = new MemoryStream();

await resource.CopyNupkgToStreamAsync(
    packageId,
    packageVersion,
    packageStream,
    cache,
    logger,
    cancellationToken);

Console.WriteLine($"Downloaded package {packageId} {packageVersion}");

using PackageArchiveReader packageReader = new PackageArchiveReader(packageStream);
NuspecReader nuspecReader = await packageReader.GetNuspecReaderAsync(cancellationToken);

Console.WriteLine($"Tags: {nuspecReader.GetTags()}");
Console.WriteLine($"Description: {nuspecReader.GetDescription()}");
```

### Get package metadata 

Get the metadata for the "Newtonsoft.Json" package using the [NuGet V3 Package Metadata API](../api/registration-base-url-resource.md):

```csharp
ILogger logger = NullLogger.Instance;
CancellationToken cancellationToken = CancellationToken.None;

SourceCacheContext cache = new SourceCacheContext();
SourceRepository repository = Repository.Factory.GetCoreV3("https://api.nuget.org/v3/index.json");
PackageMetadataResource resource = await repository.GetResourceAsync<PackageMetadataResource>();

IEnumerable<IPackageSearchMetadata> packages = await resource.GetMetadataAsync(
    "Newtonsoft.Json",
    includePrerelease: true,
    includeUnlisted: false,
    cache,
    logger,
    cancellationToken);

foreach (IPackageSearchMetadata package in packages)
{
    Console.WriteLine($"Version: {package.Identity.Version}");
    Console.WriteLine($"Listed: {package.IsListed}");
    Console.WriteLine($"Tags: {package.Tags}");
    Console.WriteLine($"Description: {package.Description}");
}
```

### Search packages

Search for "json" packages using the [NuGet V3 Search API](../api/search-query-service-resource.md):

```csharp
var logger = NullLogger.Instance;
var cancellationToken = CancellationToken.None;

var repository = Repository.Factory.GetCoreV3("https://api.nuget.org/v3/index.json");
var search = await repository.GetResourceAsync<PackageSearchResource>();
var searchFilter = new SearchFilter(includePrerelease: true);

var results = await search.SearchAsync(
    "json",
    searchFilter,
    skip: 0,
    take: 20,
    logger,
    cancellationToken);

foreach (var result in results)
{
    Console.WriteLine($"Found package {result.Identity.Id} {result.Identity.Version}");
}
```

## Third-party documentation

You can find examples and documentation for some of the API in the following blog series by Dave Glick, published 2016:

- [Exploring the NuGet v3 Libraries, Part 1: Introduction and concepts](http://daveaglick.com/posts/exploring-the-nuget-v3-libraries-part-1)
- [Exploring the NuGet v3 Libraries, Part 2: Searching for packages](http://daveaglick.com/posts/exploring-the-nuget-v3-libraries-part-2)
- [Exploring the NuGet v3 Libraries, Part 3: Installing packages](http://daveaglick.com/posts/exploring-the-nuget-v3-libraries-part-3)

> [!Note]
> These blog posts were written shortly after the **3.4.3** version of the NuGet client SDK packages were released.
> Newer versions of the packages may be incompatible with the information in the blog posts.

Martin Björkström did a follow-up blog post to Dave Glick's blog series where he introduces a different approach on using the NuGet Client SDK to install NuGet packages:

- [Revisiting the NuGet v3 Libraries](https://martinbjorkstrom.com/posts/2018-09-19-revisiting-nuget-client-libraries)
