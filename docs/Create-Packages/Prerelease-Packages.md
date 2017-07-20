---
# required metadata

title: Pre-release versions in NuGet packages | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 7/20/2017
ms.topic: article
ms.prod: nuget
#ms.service:
ms.technology: null
ms.assetid: df6a366a-22c1-47bb-8017-18231311ce88

# optional metadata

description: Guidance for building pre-release packages
keywords: versioning, NuGet package versioning, NuGet prerelease versions, NuGet prerelease packages, preview package versions, RC package versions, Beta package versions, NuGet semantic versioning
#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer:
- karann
- unnir
#ms.suite:
#ms.tgt_pltfrm:
#ms.custom:

---

# Building pre-release packages

Whenever you release an updated package with a new version number, NuGet considers that one as the "latest stable release" as shown, for example in the Package Manager UI within Visual Studio:

![Package Manager UI showing the latest stable release](media/Prerelease_01-LatestStable.png)

A stable release is one that's considered reliable enough to be used in production. The latest stable release is also the one that will be installed as a package update or during package restore (subject to constraints as described in [Reinstalling and updating packages](../consume-packages/reinstalling-and-updating-packages.md)).

To support the software release lifecycle, NuGet 1.6 and later allows for the distribution of pre-release packages, where the version number includes a semantic versioning suffix such as `-alpha`, `-beta`, or `-rc`. For more information, see [Package versioning](../Schema/Package-Versioning.md#pre-release-versions).

You can specify such versions in two ways:

- `.nuspec` file: include the semantic version suffix in the `version` element:

    ```xml
    <version>1.0.1-alpha</version>
    ```

- Assembly attributes: when building a package from a Visual Studio project (`.csproj` or `.vbproj`), use the `AssemblyInformationalVersionAttribute` to specify the version:

    ```cs
    [assembly: AssemblyInformationalVersion("1.0.1-beta")]
    ```

    NuGet picks up this value instead of the one specified in the `AssemblyVersion` attribute, which does not support semantic versioning.

> [!Note]
> A stable package release cannot have a pre-release dependency. This avoids accidentally installing potentially unstable releases.

When you’re ready to release a stable version, just remove the suffix and the package takes precedence over any pre-release versions. Again, see [Package versioning](../Schema/Package-Versioning.md#pre-release-versions).