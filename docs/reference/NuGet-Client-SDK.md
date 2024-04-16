---
title: NuGet Client SDK
description: The API is evolving and not yet documented, but examples are available on Dave Glick's blog.
author: JonDouglas
ms.author: jodou
ms.date: 01/09/2018
ms.topic: conceptual
---

# NuGet Client SDK

The *NuGet Client SDK* refers to a group of NuGet packages:

* [`NuGet.Indexing`](https://www.nuget.org/packages/NuGet.Indexing) - NuGet's indexing library for the Visual Studio client search functionality.
* [`NuGet.Commands`](https://www.nuget.org/packages/NuGet.Commands) - Complete commands common to command-line and GUI NuGet clients.
* [`NuGet.Common`](https://www.nuget.org/packages/NuGet.Common) - Common utilities and interfaces for all NuGet libraries.
* [`NuGet.Configuration`](https://www.nuget.org/packages/NuGet.Configuration) - NuGet's configuration settings implementation.
* [`NuGet.Credentials`](https://www.nuget.org/packages/NuGet.Credentials) - NuGet client's authentication models.
* [`NuGet.DependencyResolver.Core`](https://www.nuget.org/packages/NuGet.DependencyResolver.Core) - NuGet's PackageReference dependency resolver implementation.
* [`NuGet.Frameworks`](https://www.nuget.org/packages/NuGet.Frameworks) - NuGet's understanding of target frameworks.
* [`NuGet.LibraryModel`](https://www.nuget.org/packages/NuGet.LibraryModel) - NuGet's types and interfaces for understanding dependencies.
* [`NuGet.Localization`](https://www.nuget.org/packages/NuGet.Localization) - NuGet localization package.
* [`NuGet.PackageManagement`](https://www.nuget.org/packages/NuGet.PackageManagement) - NuGet Package Management functionality for Visual Studio installation flow.
* [`NuGet.Packaging`](https://www.nuget.org/packages/NuGet.Packaging) - Provides a set of APIs to interact with `.nupkg` and `.nuspec` files from a stream. `NuGet.Protocol` depends on this package.
* [`NuGet.ProjectModel`](https://www.nuget.org/packages/NuGet.ProjectModel) - NuGet's core types and interfaces for PackageReference-based restore, such as lock files, assets file and internal restore models.
* [`NuGet.Protocol`](https://www.nuget.org/packages/NuGet.Protocol) - Provides a set of APIs interact with HTTP and file-based NuGet feeds.
* [`NuGet.Resolver`](https://www.nuget.org/packages/NuGet.Resolver) - NuGet's dependency resolver for packages.config based projects.
* [`NuGet.Versioning`](https://www.nuget.org/packages/NuGet.Versioning) - NuGet's implementation of Semantic Versioning.

You can find the source code for these packages in the [NuGet/NuGet.Client](https://github.com/NuGet/NuGet.Client) GitHub repository.

> [!Note]
> For documentation on the NuGet server protocol, please refer to the [NuGet Server API](~/api/overview.md).

## Support policy
The most recent version of NuGet Client SDK is fully supported and can be relied on for bug fixes, updates, and enhancements.

The recommendation is to use the latest versions of NuGet Client SDK packages, and to examine your project for dependencies on deprecated NuGet Client SDK packages.

Out-of-support NuGet Client SDK packages will be [deprecated on nuget.org](../nuget-org/Deprecate-packages.md#client-experience-for-deprecated-packages).

### Patch Releases

Patched versions of NuGet Client SDK will be released exclusively when critical bugs or security fixes are required for a long-term support (LTS) version of Visual Studio or .NET SDK.

All security bugs should be reported to the Microsoft Security Response Center (MSRC) at [https://aka.ms/opensource/security/create-report].
Also see the [security policy in the NuGet.Client repo](https://github.com/NuGet/NuGet.Client/blob/dev/SECURITY.md).

### Package Deprecation

As of January 31, 2024, older versions of NuGet Client SDK packages that are not tied to an LTS version of either Visual Studio or .NET have been deprecated.

NuGet's package maintenance approach will align with the [.NET Package Maintenance (deprecation)](https://github.com/dotnet/announcements/issues/217) guidance.

## NuGet.Protocol

Install the `NuGet.Protocol` package to interact with HTTP and folder-based NuGet package feeds:

```ps1
dotnet add package NuGet.Protocol
```

You can find the source code for these examples on the [NuGet.Protocol.Samples](https://github.com/NuGet/Samples/tree/main/NuGetProtocolSamples) project on GitHub.

> [!Tip]
> `Repository.Factory` is defined in the `NuGet.Protocol.Core.Types` namespace, and the `GetCoreV3` method is an extension method defined in the `NuGet.Protocol` namespace. Therefore, you will need to add `using` statements for both namespaces.

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

### Push a package

Push a package using the [NuGet V3 Push and Delete API](../api/package-publish-resource.md):

[!code-csharp[PushPackage](~/../nuget-samples/NuGetProtocolSamples/Program.cs?name=PushPackage)]

### Delete a package

Delete a package using the [NuGet V3 Push and Delete API](../api/package-publish-resource.md):

> [!Note]
> NuGet servers are free to interpret a package delete request as a "hard delete", "soft delete", or "unlist".
> For example, nuget.org interprets the package delete request as an "unlist". For more information about this
> practice, see the [Deleting Packages](../nuget-org/policies/deleting-packages.md) policy.

[!code-csharp[DeletePackage](~/../nuget-samples/NuGetProtocolSamples/Program.cs?name=DeletePackage)]

### Work with authenticated feeds

Use [`NuGet.Protocol`](https://www.nuget.org/packages/NuGet.Protocol) to work with authenticated feeds.

[!code-csharp[AuthenticatedFeed](~/../nuget-samples/NuGetProtocolSamples/Program.cs?name=AuthenticatedFeed)]

## NuGet.Packaging

Install the `NuGet.Packaging` package to interact with `.nupkg` and `.nuspec` files from a stream:

```ps1
dotnet add package NuGet.Packaging
```

### Create a package

Create a package, set metadata, and add dependencies using [`NuGet.Packaging`](https://www.nuget.org/packages/NuGet.Packaging).

> [!IMPORTANT]
> It is strongly recommended that NuGet packages are created using the official NuGet tooling and **not** using this
> low-level API. There are a variety of characteristics important for a well-formed package and the latest version of
> tooling helps incorporate these best practices.
>
> For more information about creating NuGet packages, see the overview of the
> [package creation workflow](../create-packages/overview-and-workflow.md) and the documentation for official pack
> tooling (for example, [using the dotnet CLI](../create-packages/creating-a-package-dotnet-cli.md)).

[!code-csharp[CreatePackage](~/../nuget-samples/NuGetProtocolSamples/Program.cs?name=CreatePackage)]

### Read a package

Read a package from a file stream using [`NuGet.Packaging`](https://www.nuget.org/packages/NuGet.Packaging).

[!code-csharp[ReadPackage](~/../nuget-samples/NuGetProtocolSamples/Program.cs?name=ReadPackage)]

## Third-party documentation

You can find examples and documentation for some of the API in the following blog series by Dave Glick, published 2016:

* [Exploring the NuGet v3 Libraries, Part 1: Introduction and concepts](http://daveaglick.com/posts/exploring-the-nuget-v3-libraries-part-1)
* [Exploring the NuGet v3 Libraries, Part 2: Searching for packages](http://daveaglick.com/posts/exploring-the-nuget-v3-libraries-part-2)
* [Exploring the NuGet v3 Libraries, Part 3: Installing packages](http://daveaglick.com/posts/exploring-the-nuget-v3-libraries-part-3)

> [!Note]
> These blog posts were written shortly after the **3.4.3** version of the NuGet client SDK packages were released.
> Newer versions of the packages may be incompatible with the information in the blog posts.

Martin Björkström did a follow-up blog post to Dave Glick's blog series where he introduces a different approach on using the NuGet Client SDK to install NuGet packages:

* [Revisiting the NuGet v3 Libraries](https://martinbjorkstrom.com/posts/2018-09-19-revisiting-nuget-client-libraries)
