---
# required metadata

title: Overview of Hosting Your Own NuGet Feeds | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 1/9/2017
ms.topic: article
ms.prod: nuget
#ms.service:
ms.technology: nuget
ms.assetid: 97577ddd-c294-432d-81a7-b4aebe88bd1c

# optional metadata

description: An overview of opens for hosting your own NuGet package feeds or galleries either locally or remotely.
keywords: NuGet feed, NuGet gallery, custom package feed, NuGet.Server
#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer:
- karann
- unnir
- anangaur
#ms.suite:
#ms.tgt_pltfrm:
#ms.custom:

---
# Hosting your own NuGet feeds

Instead of making packages publicly available, you might want to release packages for only a limited audience, such as your organization or workgroup. In addition, some companies may want to restrict which third-party libraries their developers may use, and thus direct those developers to draw from a limited package source rather than nuget.org.

For all such purposes, NuGet supports setting up a private package source in the following ways:

- Local feed: Packages are simply placed on a suitable network file share, ideally using `nuget init` and `nuget add` to create a hierarchical folder structure (NuGet 3.3+). For details, see [Local Feeds](../hosting-packages/local-feeds.md).
- NuGet.Server: Packages are made available through a local server through HTTP. For details, see [NuGet.Server](../hosting-packages/NuGet-Server.md).
- NuGet Gallery: Packages are hosted on an Internet server using the [NuGet Gallery Project](https://github.com/NuGet/NuGetGallery#build-and-run-the-gallery-in-arbitrary-number-easy-steps) on GitHub. NuGet Gallery provides user management and additional features such as an extensive web UI that allows searching and exploring packages from within the browser, similar to nuget.org.

There are also several other NuGet hosting products that support remote private feeds, including the following:

- [Package Management](https://www.visualstudio.com/docs/package/overview) from [VS Team Services](https://www.visualstudio.com/team-services/) and [TFS](https://www.visualstudio.com/tfs/)
- [MyGet](http://myget.org)
- [ProGet](http://inedo.com/proget) from Inedo
- [NuGet Server](http://nugetserver.net/), a community project from Inedo
- [Artifactory](https://www.jfrog.com/artifactory/) from JFrog.
- [Nexus](http://www.sonatype.org/nexus/) from Sonatype.

Regardless of how packages are hosted, you access them by adding them to the list of available sources in `NuGet.Config`. This can be done in Visual Studio as described in [Package Sources](../tools/package-manager-ui.md#package-sources), or from the command line using [`nuget sources`](../tools/nuget-exe-cli-reference.md#sources). The path to a source can be anything from a local folder to a network name to an Internet URL.
