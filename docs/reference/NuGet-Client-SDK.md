---
title: NuGet Client SDK
description: The API is evolving and not yet documented, but examples are available on Dave Glick's blog.
author: karann-msft
ms.author: karann
ms.date: 01/09/2018
ms.topic: conceptual
---

# NuGet Client SDK

The *NuGet Client SDK* refers to a group of NuGet packages:

* [`NuGet.Protocol`](https://www.nuget.org/packages/NuGet.Protocol) - Used to interact with HTTP and file-based NuGet feeds
* [`NuGet.Packaging`](https://www.nuget.org/packages/NuGet.Packaging) - Used to interact with NuGet packages. `NuGet.Protocol` depends on this package

You can find the source code for these packages in the [NuGet/NuGet.Client](https://github.com/NuGet/NuGet.Client) GitHub repository.

> [!Note]
> For documentation on the NuGet server protocol, please refer to the [NuGet Server API](~/api/overview.md).

## Getting started

### Install the package

```ps1
dotnet add package NuGet.Protocol
```

## Examples

You can find these examples on the [NuGet.Protocol.Samples](https://github.com/NuGet/Samples/tree/master/NuGetProtocolSamples) project on GitHub.

### List package versions

Find all versions of Newtonsoft.Json using the [NuGet V3 Package Content API](../api/package-base-address-resource.md#enumerate-package-versions):

[!code-csharp[ListPackageVersions](~/../nuget-samples/NuGetProtocolSamples/Program.cs?name=ListPackageVersions)]

### Download a package

Download Newtonsoft.Json v12.0.1 using the [NuGet V3 Package Content API](../api/package-base-address-resource.md):

[!code-csharp[DownloadPackage](~/../nuget-samples/NuGetProtocolSamples/Program.cs?name=DownloadPackage)]

### Get package metadata

Get the metadata for the "Newtonsoft.Json" package using the [NuGet V3 Package Metadata API](../api/registration-base-url-resource.md):

[!code-csharp[GetPackageMetadata](~/../nuget-samples/NuGetProtocolSamples/Program.cs?name=GetPackageMetadata)]

### Search packages

Search for "json" packages using the [NuGet V3 Search API](../api/search-query-service-resource.md):

[!code-csharp[SearchPackages](~/../nuget-samples/NuGetProtocolSamples/Program.cs?name=SearchPackages)]

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
