---
title: Catalog Resource, NuGet V3 API
description: The catalog is an index of all packages created and deleted on nuget.org.
author: joelverhagen
ms.author: jver
ms.date: 10/30/2017
ms.topic: reference
ms.reviewer: kraigb
---

# Catalog

The **catalog** is a resource that records all package operations on a package source, such as creations and deletions. The catalog resource has the `Catalog` type in the [service index](service-index.md). You can use this resource to [query for all published packages](../guides/api/query-for-all-published-packages.md).

> [!Note]
> Because the catalog is not used by the official NuGet client, not all package sources implement the catalog.

> [!Note]
> Currently, the nuget.org catalog is not available in China. For more details, see [NuGet/NuGetGallery#4949](https://github.com/NuGet/NuGetGallery/issues/4949).

## Versioning

The following `@type` value is used:

@type value   | Notes
------------- | -----
Catalog/3.0.0 | The initial release

## Base URL

The entry point URL for the following APIs is the value of the `@id` property associated with the aforementioned
resource `@type` values. This topic uses the placeholder URL `{@id}`.

## HTTP methods

All URLs found in the catalog resource support only the HTTP methods `GET` and `HEAD`.

## Catalog index

The catalog index is a document in a well-known location that has a list of catalog items, ordered chronologically. It is the entry point of the catalog resource.

The index is comprised of catalog pages. Each catalog page contains catalog items. Each catalog item represents an event concerning a single package at a point in time. A catalog item can represent a package that was created, unlisted, relisted, or deleted from the package source. By processing the catalog items in chronological order, the client can build an up-to-date view of every package that exists on the V3 package source.

In short, catalog blobs have the following hierarchical structure:

- **Index**: the entry point for the catalog.
- **Page**: a grouping of catalog items.
- **Leaf**: a document representing a catalog item, which is a snapshot of a single package's state.

Each catalog object has a property called the `commitTimeStamp` representing when the item was added to the catalog. Catalog items are added to a catalog page in batches called commits. All catalog items in the same commit have the same commit timestamp (`commitTimeStamp`) and commit ID (`commitId`). Catalog items placed in the same commit represent events that happened around the same point in time on the package source. There is no ordering within a catalog commit.

Because each package ID and version is unique, there will never be more than one catalog item in a commit. This ensures that catalog items for a single package can always be unambiguously ordered with respect to commit timestamp.

There is never be more than one commit to the catalog per `commitTimeStamp`. In other words, the `commitId` is redundant with the `commitTimeStamp`.

In contrast to the [package metadata resource](registration-base-url-resource.md), which is indexed by package ID, the catalog is indexed (and queryable) only by time.

Catalog items are always added to the catalog in a monotonically increasing, chronological order. This means that if a catalog commit is added at time X then no catalog commit will ever be added with a time less than or equal to X.

The following request fetches the catalog index.

```
GET {@id}
```

The catalog index is a JSON document that contains an object with the following properties:

Name            | Type             | Required | Notes
--------------- | ---------------- | -------- | -----
commitId        | string           | yes      | A unique ID associated with the most recent commit
commitTimeStamp | string           | yes      | A timestamp of the most recent commit
count           | integer          | yes      | The number of pages in the index
items           | array of objects | yes      | A array of objects, each object representing a page

Each element in the `items` array is an object with some minimal details about each page. These page objects do not
contain the catalog leaves (items). The order of the elements in this array is not defined. Pages can be ordered by the
client in memory using their `commitTimeStamp` property.

As new pages are introduced, the `count` will be incremented and new objects will appear in the `items` array.

As items are added to the catalog, the index's `commitId` will change and the `commitTimeStamp` will increase. These
two properties are essentially a summary over all page `commitId` and `commitTimeStamp` values in the `items` array.

### Catalog page object in the index

The catalog page objects found in the catalog index's `items` property have the following properties:

Name            | Type    | Required | Notes
--------------- | ------- | -------- | -----
@id             | string  | yes      | The URL to fetch catalog page
commitId        | string  | yes      | A unique ID associated with the most recent commit in this page
commitTimeStamp | string  | yes      | A timestamp of the most recent commit in this page
count           | integer | yes      | The number of items in the catalog page

In contrast to the [package metadata resource](registration-base-url-resource.md) which in some cases inlines leaves
into the index, catalog leaves are never inlined into the index and must always be fetched by using the page's `@id`
URL.

### Sample request

```
GET https://api.nuget.org/v3/catalog0/index.json
```

### Sample response

[!code-JSON [catalog-index.json](./_data/catalog-index.json)]

## Catalog page

The catalog page is a collection of catalog items. It is a document fetched using one of the `@id` values found in the
catalog index. The URL to a catalog page is not intended to be predictable and should be discovered using only the
catalog index.

New catalog items are added to the page in the catalog index only with the highest commit timestamp or to a
new page. Once a page with a higher commit timestamp is added to the catalog, older pages are never added to or
changed.

The catalog page document is a JSON object with the following properties:

Name            | Type             | Required | Notes
--------------- | ---------------- | -------- | -----
commitId        | string           | yes      | A unique ID associated with the most recent commit in this page
commitTimeStamp | string           | yes      | A timestamp of the most recent commit in this page
count           | integer          | yes      | The number of items in the page
items           | array of objects | yes      | The catalog items in this page
parent          | string           | yes      | A URL to the catalog index

Each element in the `items` array is an object with some minimal details about the catalog item. These item objects do
not contain all of the catalog item's data. The order of the items in the page's `items` array is not defined. Items
can be ordered by the client in memory using their `commitTimeStamp` property.

The number of catalog items in a page is defined by server implementation. For nuget.org, there are at most 550
items in each page, although the actual number may be smaller for some pages depending on the size of the next commit
batch at the point in time.

As new items are introduced, the `count` is incremented and new catalog item objects appear in the `items`
array.

As items are added to the page, the `commitId` changes and the `commitTimeStamp` increases. These two
properties are essentially a summary over all `commitId` and `commitTimeStamp` values in the `items` array.

### Catalog item object in a page

The catalog item objects found in the catalog page's `items` property have the following properties:

Name            | Type    | Required | Notes
--------------- | ------- | -------- | -----
@id             | string  | yes      | The URL to fetch the catalog item
@type           | string  | yes      | The type of the catalog item
commitId        | string  | yes      | The commit ID associated with this catalog item
commitTimeStamp | string  | yes      | The commit timestamp of this catalog item
nuget:id        | string  | yes      | The package ID that this leaf is related to
nuget:version   | string  | yes      | The package version that this leaf is related to

The `@type` value will be one of the following two values:

1. `nuget:PackageDetails`: this corresponds to `PackageDetails` type in the catalog leaf document.
1. `nuget:PackageDelete`: this corresponds to the `PackageDelete` type in the catalog leaf document.

For more details about what each type means, see the [corresponding items type](#item-types) below.

### Sample request

```
GET https://api.nuget.org/v3/catalog0/page2926.json
```

### Sample response

[!code-JSON [catalog-page.json](./_data/catalog-page.json)]

## Catalog leaf

The catalog leaf contains metadata about a specific package ID and version at some point in time. It is a document
fetched using the `@id` value found in a catalog page. The URL to a catalog leaf is not intended to be predictable
and should be discovered using only a catalog page.

The catalog leaf document is a JSON object with the following properties:

Name                    | Type                       | Required | Notes
----------------------- | -------------------------- | -------- | -----
@type                   | string or array of strings | yes      | The type(s) of the catalog item
catalog:commitId        | string                     | yes      | A commit ID associated with this catalog item
catalog:commitTimeStamp | string                     | yes      | The commit timestamp of this catalog item
id                      | string                     | yes      | The package ID of the catalog item
published               | string                     | yes      | The published date of the package catalog item
version                 | string                     | yes      | The package version of the catalog item

### Item types

The `@type` property is a string or array of strings. For convenience, if the `@type` value is a string, it should be
treated as any array of size one. Not all possible values for `@type` are documented. However, each catalog item has exactly one of the two following string type values:

1. `PackageDetails`: represents a snapshot of package metadata
1. `PackageDelete`: represents a package that was deleted

### Package details catalog items

Catalog items with the type `PackageDetails` contain a snapshot of package metadata for a specific package (ID and
version combination). A package details catalog item is produced when a package source encounters any of the
following scenarios:

1. A package is **pushed**.
1. A package is **listed**.
1. A package is **unlisted**.
1. A package is **deprecated**.
1. A package is **undeprecated**.
1. A package is **reflowed**.
1. A package is marked **vulnerable**.

A package reflow is an administrative gesture that essentially generates a fake push of an existing package with no
changes to the package itself. On nuget.org, a reflow is used after fixing a bug in one of the background jobs
which consume the catalog.

Clients consuming the catalog items should not attempt to determine which of these scenarios produced the catalog
item. Instead, the client should simply update any maintained view or index with the metadata contained in the
catalog item. Furthermore, duplicate or redundant catalog items should be handled gracefully (idempotently).

Package details catalog items have the following properties in addition to those
[included on all catalog leaves](#catalog-leaf).

Name                    | Type                       | Required | Notes
----------------------- | -------------------------- | -------- | -----
authors                 | string                     | no       |
created                 | string                     | no       | A timestamp of when the package was first created. Fallback property: `published`.
dependencyGroups        | array of objects           | no       | The dependencies of the package, grouped by target framework ([same format as the package metadata resource](registration-base-url-resource.md#package-dependency-group))
deprecation             | object                     | no       | The deprecation associated with the package ([same format as the package metadata resource](registration-base-url-resource.md#package-deprecation))
description             | string                     | no       |
iconUrl                 | string                     | no       |
isPrerelease            | boolean                    | no       | Whether or not the package version is prerelease. Can be detected from `version`.
language                | string                     | no       |
licenseUrl              | string                     | no       |
listed                  | boolean                    | no       | Whether or not the package is listed
minClientVersion        | string                     | no       |
packageHash             | string                     | yes      | The hash of the package, encoding using [standard base 64](https://tools.ietf.org/html/rfc4648#section-4)
packageHashAlgorithm    | string                     | yes      |
packageSize             | integer                    | yes      | The size of the package .nupkg in bytes
packageTypes            | array of objects           | no       | The package types specified by the author.
projectUrl              | string                     | no       |
releaseNotes            | string                     | no       |
requireLicenseAgreement | boolean                    | no       | Assume `false` if excluded
summary                 | string                     | no       |
tags                    | array of strings           | no       |
title                   | string                     | no       |
verbatimVersion         | string                     | no       | The version string as it's originally found in the .nuspec
vulnerabilities         | array of objects           | no       | The security vulnerabilities of the package

The package `version` property is the full version string after normalization. This means that SemVer 2.0.0 build data can
be included here.

The `created` timestamp is when the package was first received by the package source, which is typically a short
time before the catalog item's commit timestamp.

The `packageHashAlgorithm` is a string defined by the server implementation representing the hashing algorithm used to
produce the `packageHash`. nuget.org always used the `packageHashAlgorithm` value of `SHA512`.

The `packageTypes` property will only be present if a package type was specified by the author. If it is present, it will always have at least one (1) entry. Each item in the `packageTypes` array is a JSON object with the following properties:

Name      | Type    | Required | Notes
--------- | ------- | -------- | -----
name      | string  | yes      | The name of the package type.
version    | string  | no       | The version of the package type. Only present if the author explicitly specified a version in the nuspec.

The `published` timestamp is the time when the package was last listed.

> [!Note]
> On nuget.org, the `published` value is set to the year 1900 when the package is unlisted.

#### Vulnerabilities

An array of `vulnerability` objects. Each vulnerability has the following properties:

Name         | Type   | Required | Notes
------------ | ------ | -------- | -----
advisoryUrl  | string | yes      | Location of security advisory for the package
severity     | string | yes      | Severity of advisory: "0" = Low, "1" = Moderate, "2" = High, "3" = Critical

If the `severity` property contains values other than those listed here, the severity of the advisory is to be treated as Low.

#### Sample request

```
GET https://api.nuget.org/v3/catalog0/data/2015.02.01.11.18.40/windowsazure.storage.1.0.0.json
```

#### Sample response

[!code-JSON [catalog-package-details.json](./_data/catalog-package-details.json)]

### Package delete catalog items

Catalog items with the type `PackageDelete` contain a minimal set of information indicating to catalog clients that a
package has been deleted from the package source and is no longer available for any package operation (such as
restore).

> [!NOTE]
> It is possible for a package to be deleted and later republished using the same package ID and version. On nuget.org,
> this is a very rare case as it breaks the official client's assumption that a package ID and version imply a specific
> package content. For more information about package deletion on nuget.org, see
> [our policy](../nuget-org/policies/deleting-packages.md).

Package delete catalog items have no additional properties in addition to those
[included on all catalog leaves](#catalog-leaf).

The `version` property is the original version string found in the package .nuspec.

The `published` property is the time when package was deleted, which is typically as short time before the catalog
item's commit timestamp.

#### Sample request

```
GET https://api.nuget.org/v3/catalog0/data/2017.11.02.00.40.00/netstandard1.4_lib.1.0.0-test.json
```

#### Sample response

[!code-JSON [catalog-package-delete.json](./_data/catalog-package-delete.json)]

## Cursor

### Overview

This section describes a client concept that, although is not necessarily mandated by the protocol, should be part of
any practical catalog client implementation.

Because the catalog is an append-only data structure indexed by time, the client should store a **cursor** locally,
representing up to what point in time the client has processed catalog items. Note that this cursor value should never
be generated using the client's machine clock. Instead, the value should come from a catalog object's
`commitTimestamp` value.

Every time the client wants to process new events on the package source, it need only query the catalog for all
catalog items with a commit timestamp greater than its stored cursor. After the client successfully processes all new
catalog items, it records the latest commit timestamp of catalog items just processed as its new cursor value.

Using this approach, the client can be sure to never miss any package events that occurred on the package source.
Additionally, the client never has to reprocess old events prior to the cursor's recorded commit timestamp.

This powerful concept of cursors is used for many of nuget.org background jobs and is used to keep the V3 API itself
up-to-date. 

### Initial value

When the catalog client is starting for the very first time (and therefore has no cursor value), it should use a default
cursor value of .NET's `System.DateTimeOffset.MinValue` or some such analogous notion of minimum representable
timestamp.

### Iterating over catalog items

To query for the next set of catalog items to process, the client should:

1. Fetch the recorded cursor value from a local store.
1. Download and deserialize the catalog index.
1. Find all catalog pages with a commit timestamp *greater than* the cursor.
1. Declare an empty list of catalog items to process.
1. For each catalog page matched in step 3:
   1. Download and deserialized the catalog page.
   1. Find all catalog items with a commit timestamp *greater than* the cursor.
   1. Add all matching catalog items to the list declared in step 4.
1. Sort the catalog item list by commit timestamp.
1. Process each catalog item in sequence:
   1. Download and deserialize the catalog item.
   1. React appropriately to the catalog item's type.
   1. Process the catalog item document in a client-specific fashion.
1. Record the last catalog item's commit timestamp as the new cursor value.

With this basic algorithm, the client implementation can build up a complete view of all packages available on the
package source. The client need only execute this algorithm periodically to always be aware of the latest changes
to the package source.

> [!Note]
> This is the algorithm that nuget.org uses to keep the [Package Metadata](registration-base-url-resource.md),
> [Package Content](package-base-address-resource.md), [Search](search-query-service-resource.md) and
> [Autocomplete](search-autocomplete-service-resource.md) resources up to date.

### Dependent cursors

Suppose there are two catalog clients that have an inherent dependency where one client's output depends on another
client's output. 

#### Example

For example, on nuget.org a newly published package should not appear in the search resource before it appears in the
package metadata resource. This is because the "restore" operation performed by the official NuGet client uses the
package metadata resource. If a customer discovers a package using the search service, they should be able to
successfully restore that package using the package metadata resource. In other words, the search resource depends on
the package metadata resource. Each resource has a catalog client background job updating that resource. Each client
has its own cursor.

Since both resources are built off of the catalog, the cursor of the catalog client that updates the search resource
*must not go beyond* the cursor of the package metadata catalog client.

#### Algorithm

To implement this restriction, simply modify the algorithm above to be:

1. Fetch the recorded cursor value from a local store.
1. Download and deserialize the catalog index.
1. Find all catalog pages with a commit timestamp *greater than* the cursor **less than or equal to the
   dependency's cursor.**
1. Declare an empty list of catalog items to process.
1. For each catalog page matched in step 3:
   1. Download and deserialized the catalog page.
   1. Find all catalog items with a commit timestamp *greater than* the cursor **less than or equal to the
   dependency's cursor.**
   1. Add all matching catalog items to the list declared in step 4.
1. Sort the catalog item list by commit timestamp.
1. Process each catalog item in sequence:
   1. Download and deserialize the catalog item.
   1. React appropriately to the catalog item's type.
   1. Process the catalog item document in a client-specific fashion.
1. Record the last catalog item's commit timestamp as the new cursor value.

Using this modified algorithm, you can build a system of dependent catalog clients all producing their own specific
indexes, artifacts, etc.
