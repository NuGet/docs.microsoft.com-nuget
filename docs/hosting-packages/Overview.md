---
title: Overview of Hosting Your Own NuGet Feeds
description: An overview of opens for hosting your own NuGet package feeds or galleries either locally or remotely.
author: JonDouglas
ms.author: jodou
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

There are also several other NuGet hosting products such as [Azure Artifacts](https://www.visualstudio.com/docs/package/nuget/publish) and [GitHub package registry](https://help.github.com/articles/configuring-nuget-for-use-with-github-package-registry) that support remote private feeds. Below is a list of such products:

- [Artifactory](https://www.jfrog.com/artifactory/) from JFrog.
- [Azure Artifacts](https://www.visualstudio.com/docs/package/nuget/publish), which is also available on Team Foundation Server 2017 and later.
- [BaGet](https://github.com/loic-sharma/BaGet), an open-source implementation of NuGet V3 server built on ASP.NET Core
- [Bytesafe](https://docs.bytesafe.dev/package-managers/nuget/) A fully managed package and supply chain security platform
- [Cloudsmith](https://cloudsmith.io/l/nuget-feed/), a fully managed package management SaaS
- [GitHub package registry](https://help.github.com/articles/configuring-nuget-for-use-with-github-package-registry)
- [GitLab Package Registry](https://docs.gitlab.com/ee/user/packages/nuget_repository/)
- [LiGet](https://github.com/ai-traders/liget), an open-source implementation of NuGet V2 server that runs on kestrel in docker
- [MyGet](https://myget.org)
- [Nexus Repository OSS](https://www.sonatype.com/nexus-repository-oss) from Sonatype.
- [NuGet Server (Open Source)](https://github.com/svenkle/nuget-server), an open-source implementation similar to Inedo's NuGet Server
- [NuGet Server](http://nugetserver.net/), a community project from Inedo
- [ProGet](https://inedo.com/proget) from Inedo
- [Sleet](https://github.com/emgarten/sleet), an open-source NuGet V3 static feed generator
- [TeamCity](https://www.jetbrains.com/teamcity/) from JetBrains.

Regardless of how packages are hosted, you access them by adding them to the list of available sources in `NuGet.Config`. This can be done in Visual Studio as described in [Package Sources](../consume-packages/install-use-packages-visual-studio.md#package-sources), or from the command line using [`nuget sources`](../reference/cli-reference/cli-ref-sources.md). The path to a source can be a local folder pathname, a network name, or a URL.
