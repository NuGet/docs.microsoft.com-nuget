---
title: Overview of Hosting Your Own NuGet Feeds
description: An overview of opens for hosting your own NuGet package feeds or galleries either locally or remotely.
author: karann-msft
ms.author: karann
ms.date: 08/25/2017
ms.topic: conceptual
ms.reviewer: anangaur
---

# Hosting your own NuGet feeds

Instead of making packages publicly available, you might want to release packages to only a limited audience, such as your organization or workgroup. In addition, some companies may want to restrict which third-party libraries their developers may use, and thus direct those developers to draw from a limited package source rather than nuget.org.

For all such purposes, NuGet supports setting up private package sources in the following ways:

- Local feed: Packages are simply placed on a suitable network file share, ideally using `nuget init` and `nuget add` to create a hierarchical folder structure (NuGet 3.3+). For details, see [Local Feeds](../hosting-packages/local-feeds.md).
- NuGet.Server: Packages are made available through a local HTTP server. For details, see [NuGet.Server](../hosting-packages/nuget-server.md).
- NuGet Gallery: Packages are hosted on an Internet server using the [NuGet Gallery Project](https://github.com/NuGet/NuGetGallery#build-and-run-the-gallery-in-arbitrary-number-easy-steps) (github.com). NuGet Gallery provides user management and features such as an extensive web UI that allows searching and exploring packages from within the browser, similar to nuget.org.

There are also several other NuGet hosting products that support remote private feeds, including the following:

- [Visual Studio Team Services Package Management](https://www.visualstudio.com/docs/package/nuget/publish), which is also available on Team Foundation Server 2017 and later.
- [MyGet](http://myget.org)
- [ProGet](http://inedo.com/proget) from Inedo
- [NuGet Server](http://nugetserver.net/), a community project from Inedo
- [NuGet Server (Open Source)](http://nuget-server.net), an open-source implementation similar to Inedo's NuGet Server
- [LiGet](https://github.com/ai-traders/liget), an open-source implementation of NuGet V2 server that runs on kestrel in docker
- [BaGet](https://github.com/loic-sharma/BaGet), an open-source implementation of NuGet V3 server built on ASP.NET Core
- [Artifactory](https://www.jfrog.com/artifactory/) from JFrog.
- [Nexus](http://www.sonatype.org/nexus/) from Sonatype.
- [TeamCity](https://www.jetbrains.com/teamcity/) from JetBrains.

Regardless of how packages are hosted, you access them by adding them to the list of available sources in `NuGet.Config`. This can be done in Visual Studio as described in [Package Sources](../tools/package-manager-ui.md#package-sources), or from the command line using [`nuget sources`](../tools/cli-ref-sources.md). The path to a source can be a local folder pathname, a network name, or a URL.
