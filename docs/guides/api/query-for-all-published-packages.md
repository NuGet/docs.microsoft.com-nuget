---
title: Query for all packages published to nuget.org
description: Using the NuGet API, you can query for all packages published to nuget.org and stay up-to-date over time.
author: joelverhagen
ms.author: jver
ms.date: 11/02/2017
ms.topic: tutorial
ms.reviewer: kraigb
---

# Query for all packages published to nuget.org

One common query pattern on the legacy OData V2 API was enumerating all packages published to nuget.org, ordered by when the package was published. Scenarios requiring this kind of query against nuget.org vary widely:

- Replicating nuget.org entirely
- Detecting when packages have new versions released
- Finding packages that depend on your package

The legacy way of doing this typically depended on sorting the OData package entity by a timestamp and paging across the massive result set using `skip` and `top` (page size) parameters. Unfortunately, this approach has some drawbacks:

- Possibility of missing packages, because the queries are being made on data that is often changing order
- Slow query response time, because the queries are not optimized (the most optimized queries are ones that support a mainline scenario for the official NuGet client)
- Use of deprecated and undocumented API, meaning the support of such queries in the future is not guaranteed
- Inability to replay history in the exact order that it transpired

For this reason, the following guide can be followed to address the aforementioned scenarios in a more reliable and future-proof way.

## Overview

At the center of this guide is resource in the [NuGet API](../../api/overview.md) called the **catalog**. The catalog is an append-only API that allows the caller to see a full history of packages added to, modified, and deleted from nuget.org. If you are interested in all or even a subset of packages published to nuget.org, the catalog is a great way to stay up-to-date with the set of currently available packages as time goes on.

This guide is intended to be a high-level walk-through but if you are interested in the fine-grain details of the catalog, see its [API reference document](../../api/catalog-resource.md).

The following steps can be implemented in any programming language of your choice. If you want a full running sample, take a look at the [C# sample](#c-sample-code) mentioned below.

Otherwise, follow the guide below to build a reliable catalog reader.

## Initialize a cursor

The first step in building a reliable catalog reader is implementing a cursor. For full details about the design of a catalog cursor, see the [catalog reference document](../../api/catalog-resource.md#cursor). In short, cursor is a point in time up to which you have processed events in the catalog. Events in the catalog represent package publishes and other package changes. If you care about all packages ever published to NuGet (since the beginning of time), you would initialize your cursor to a "minimum value" timestamp (e.g. `DateTime.MinValue` in .NET). If you care only about packages published starting now, you would use the current timestamp as your initial cursor value.

For this guide, we'll initialize our cursor to a timestamp one hour ago. For now, just save that timestamp in memory.

```cs
DateTime cursor = DateTime.UtcNow.AddHours(-1);
```

## Determine catalog index URL

The location of every resource (endpoint) in the NuGet API should be discovered using the [service index](../../api/service-index.md). Because this guide focuses on nuget.org, we'll be using nuget.org's service index.

```
GET https://api.nuget.org/v3/index.json
```

The service document is JSON document containing all of the resources on nuget.org. Look for the resource having the `@type` property value of `Catalog/3.0.0`. The associated `@id` property value is the URL to the catalog index itself. 

## Find new catalog leaves

Using the `@id` property value found in the previous step, download the catalog index:

```
GET https://api.nuget.org/v3/catalog0/index.json
```

Deserialize the [catalog index](../../api/catalog-resource.md#catalog-index). Filter out all [catalog page objects](../../api/catalog-resource.md#catalog-page-object-in-the-index) with `commitTimeStamp` less than or equal to your current cursor value.

For each remaining catalog page, download the full document using the `@id` property.

```
GET https://api.nuget.org/v3/catalog0/page2926.json
```

Deserialize the [catalog page](../../api/catalog-resource.md#catalog-page). Filter out all [catalog leaf objects](../../api/catalog-resource.md#catalog-item-object-in-a-page) with `commitTimeStamp` less than or equal to your current cursor value.

After you have downloaded all of the catalog pages not filtered out, you have a set of catalog leaf objects representing packages that have been published, unlisted, listed, or deleted in the time between your cursor timestamp and now.

## Process catalog leaves

At this point, you can perform any custom processing you'd like on the catalog items. If all you need is the ID and version of the package, you can inspect the `nuget:id` and `nuget:version` properties on the catalog item objects found in the pages. Make sure to look at the `@type` property to know if the catalog item concerns an existing package or a deleted package.

If you are interested in the metadata about the package (such as the description, dependencies, .nupkg size, etc), you can fetch the [catalog leaf document](../../api/catalog-resource.md#catalog-leaf) using the `@id` property.

```
GET https://api.nuget.org/v3/catalog0/data/2015.02.01.11.18.40/windowsazure.storage.1.0.0.json
```

This document has all of the metadata included in the [package metadata resource](../../api/registration-base-url-resource.md), and more!

This step is where you implement your custom logic. The other steps in this guide are implemented in pretty much the same way not matter what you are doing with the catalog leaves.

### Downloading the .nupkg

If you are interested in downloading the .nupkg's for packages found in the catalog, you can use the [package content resource](../../api/package-base-address-resource.md). However, note that there is a short delay between when a package is found in catalog and when it's available in the package content resource. Therefore, if you encounter `404 Not Found` when attempting to download a .nupkg for a package that you found in the catalog, simply retry a short time later. Fixing this delay is tracked by GitHub issue [NuGet/NuGetGallery#3455](https://github.com/NuGet/NuGetGallery/issues/3455).

## Move the cursor forward

Once you have successfully processed the catalog items, you need to determine the new cursor value to save. To do this, find the maximum (latest chronologically) `commitTimeStamp` of all catalog items that you processed. This is your new cursor value. Save it to some persistent store, like a database, file system, or blob storage. When you want to get more catalog items, simply start from the [first step](#initialize-a-cursor) by initializing your cursor value from this persistent store.

If your application throws an exception or faults, don't move the cursor forward. Moving the cursor forward has the meaning that you never again need to process catalog items before your cursor.

If, for some reason, you have a bug in how you process catalog leaves, you can simply move your cursor backward in time and allow your code to reprocess the old catalog items.

## C# sample code

Because the catalog is a set of JSON documents available over HTTP, it can be interacted with using any programming language that has an HTTP client and JSON deserializer.

C# samples are available in the [NuGet/Samples repository](https://github.com/NuGet/Samples/tree/main/CatalogReaderExample).

```cli
git clone https://github.com/NuGet/Samples.git
```

### Catalog SDK

The easiest way to consume the catalog is to use the pre-release .NET catalog SDK package `NuGet.Protocol.Catalog`, which is available on Azure Artifacts using the following NuGet package source URL: `https://pkgs.dev.azure.com/dnceng/public/_packaging/nuget-build/nuget/v3/index.json`.

You can install this package to a project compatible with `netstandard1.3` or greater (such as .NET Framework 4.6).

A sample using this package is available on GitHub in the [NuGet.Protocol.Catalog.Sample project](https://github.com/NuGet/Samples/tree/main/CatalogReaderExample/NuGet.Protocol.Catalog.Sample).

#### Sample output

```output
2017-11-10T22:16:44.8689025+00:00: Found package details leaf for xSkrape.APIWrapper.REST 1.0.2.
2017-11-10T22:16:54.6972769+00:00: Found package details leaf for xSkrape.APIWrapper.REST 1.0.1.
2017-11-10T22:19:20.6385542+00:00: Found package details leaf for Platform.EnUnity 1.0.8.
...
2017-11-10T23:05:04.9695890+00:00: Found package details leaf for xSkrape.APIWrapper.Base 1.0.1.
2017-11-10T23:05:04.9695890+00:00: Found package details leaf for xSkrape.APIWrapper.Base 1.0.2.
2017-11-10T23:07:23.1303569+00:00: Found package details leaf for VeiculoX.Model 0.0.15.
Processing the catalog leafs failed. Retrying.
fail: NuGet.Protocol.Catalog.LoggerCatalogLeafProcessor[0]
      10 catalog commits have been processed. We will now simulate a failure.
warn: NuGet.Protocol.Catalog.CatalogProcessor[0]
      Failed to process leaf https://api.nuget.org/v3/catalog0/data/2017.11.10.23.07.23/veiculox.model.0.0.15.json (VeiculoX.Model 0.0.15, PackageDetails).
warn: NuGet.Protocol.Catalog.CatalogProcessor[0]
      13 out of 59 leaves were left incomplete due to a processing failure.
warn: NuGet.Protocol.Catalog.CatalogProcessor[0]
      1 out of 1 pages were left incomplete due to a processing failure.
2017-11-10T23:07:23.1303569+00:00: Found package details leaf for VeiculoX.Model 0.0.15.
2017-11-10T23:07:33.0212446+00:00: Found package details leaf for VeiculoX.Model 0.0.14.
2017-11-10T23:07:41.6621837+00:00: Found package details leaf for VeiculoX.Model 0.0.13.
2017-11-10T23:09:58.5728614+00:00: Found package details leaf for CreaSoft.Composition.Web.Extensions 1.1.0.
2017-11-10T23:09:58.5728614+00:00: Found package details leaf for DarkXaHTeP.Extensions.Configuration.Consul 0.0.4.
2017-11-10T23:09:58.5728614+00:00: Found package details leaf for xSkrape.APIWrapper.REST.Sample 1.0.3.
2017-11-10T23:10:09.0574930+00:00: Found package details leaf for Microsoft.VisualStudio.Imaging 15.4.27004.
2017-11-10T23:10:09.0574930+00:00: Found package details leaf for Microsoft.VisualStudio.Imaging.Interop.14.0.DesignTime 14.3.25407.
2017-11-10T23:10:09.0574930+00:00: Found package details leaf for Microsoft.VisualStudio.Language.Intellisense 15.4.27004.
2017-11-10T23:10:09.0574930+00:00: Found package details leaf for Microsoft.VisualStudio.Language.StandardClassification 15.4.27004.
2017-11-10T23:10:09.0574930+00:00: Found package details leaf for Microsoft.VisualStudio.ManagedInterfaces 8.0.50727.
2017-11-10T23:10:09.0574930+00:00: Found package details leaf for xSkrape.APIWrapper.REST.Sample 1.0.2.
2017-11-10T23:10:09.0574930+00:00: Found package details leaf for xSkrape.APIWrapper.REST.Sample 1.0.3.
```

### Minimal sample

For an example with fewer dependencies that illustrates the interaction with the catalog in more detail, see the [CatalogReaderExample sample project](https://github.com/NuGet/Samples/tree/main/CatalogReaderExample/CatalogReaderExample). The project targets `netcoreapp2.0` and depends on the [NuGet.Protocol 4.4.0](https://www.nuget.org/packages/NuGet.Protocol/4.4.0) (for resolving the service index) and [Newtonsoft.Json 9.0.1](https://www.nuget.org/packages/Newtonsoft.Json/9.0.1) (for JSON deserialization).

The main logic of the code is visible in the [Program.cs file](https://github.com/NuGet/Samples/blob/main/CatalogReaderExample/CatalogReaderExample/Program.cs).

#### Sample output

```output
No cursor found. Defaulting to 11/2/2017 9:41:28 PM.
Fetched catalog index https://api.nuget.org/v3/catalog0/index.json.
Fetched catalog page https://api.nuget.org/v3/catalog0/page2935.json.
Processing 69 catalog leaves.
11/2/2017 9:32:35 PM: DotVVM.Compiler.Light 1.1.7 (type is nuget:PackageDetails)
11/2/2017 9:32:35 PM: Momentum.Pm.Api 5.12.181-beta (type is nuget:PackageDetails)
11/2/2017 9:32:44 PM: Momentum.Pm.PortalApi 5.12.181-beta (type is nuget:PackageDetails)
11/2/2017 9:35:14 PM: Genesys.Extensions.Standard 3.17.11.40 (type is nuget:PackageDetails)
11/2/2017 9:35:14 PM: Genesys.Extensions.Core 3.17.11.40 (type is nuget:PackageDetails)
11/2/2017 9:35:14 PM: Halforbit.DataStores.FileStores.Serialization.Bond 1.0.4 (type is nuget:PackageDetails)
11/2/2017 9:35:14 PM: Halforbit.DataStores.FileStores.AmazonS3 1.0.4 (type is nuget:PackageDetails)
11/2/2017 9:35:14 PM: Halforbit.DataStores.DocumentStores.DocumentDb 1.0.6 (type is nuget:PackageDetails)
11/2/2017 9:35:14 PM: Halforbit.DataStores.FileStores.BlobStorage 1.0.5 (type is nuget:PackageDetails)
...
11/2/2017 10:23:54 PM: Cake.GitPackager 0.1.2 (type is nuget:PackageDetails)
11/2/2017 10:23:54 PM: UtilPack.NuGet 2.0.0 (type is nuget:PackageDetails)
11/2/2017 10:23:54 PM: UtilPack.NuGet.AssemblyLoading 2.0.0 (type is nuget:PackageDetails)
11/2/2017 10:26:26 PM: UtilPack.NuGet.Deployment 2.0.0 (type is nuget:PackageDetails)
11/2/2017 10:26:26 PM: UtilPack.NuGet.Common.MSBuild 2.0.0 (type is nuget:PackageDetails)
11/2/2017 10:26:36 PM: InstaClient 1.0.2 (type is nuget:PackageDetails)
11/2/2017 10:26:36 PM: SecureStrConvertor.VARUN_RUSIYA 1.0.0.5 (type is nuget:PackageDetails)
Writing cursor value: 11/2/2017 10:26:36 PM.
```
