---
title: NuGet Client SDK
description: The API is evolving and not yet documented, but examples are available on Dave Glick's blog.
author: karann-msft
ms.author: karann
ms.date: 01/09/2018
ms.topic: conceptual
---

# NuGet Client SDK

The *NuGet Client SDK* refers to a group of NuGet packages centered around [NuGet.Commands](https://www.nuget.org/packages/NuGet.Commands), [NuGet.Packaging](https://www.nuget.org/packages/NuGet.Packaging), and [NuGet.Protocol](https://www.nuget.org/packages/NuGet.Protocol). These packages replace the earlier [NuGet.Core](https://www.nuget.org/packages/NuGet.Core/) library.

> [!Note]
>  For documentation on the NuGet server protocol, please refer to the [NuGet Server API](~/api/overview.md).

## Source code

The source code is published on GitHub in the project [NuGet/NuGet.Client](https://github.com/NuGet/NuGet.Client).

## Third-party documentation

You can find examples and documentation for some of the API in the following blog series by Dave Glick, published 2016:

- [Exploring the NuGet v3 Libraries, Part 1: Introduction and concepts](http://daveaglick.com/posts/exploring-the-nuget-v3-libraries-part-1)
- [Exploring the NuGet v3 Libraries, Part 2: Searching for packages](http://daveaglick.com/posts/exploring-the-nuget-v3-libraries-part-2)
- [Exploring the NuGet v3 Libraries, Part 3: Installing packages](http://daveaglick.com/posts/exploring-the-nuget-v3-libraries-part-3)

> [!Note]
> These blog posts were written shortly after the **3.4.3** version of the NuGet client SDK packages were released.
> Newer versions of the packages may be incompatible with the information in the blog posts.

Martin Björkström did a follow-up blog post to Dave Glick's blog series where he introduces a different approach on using the NuGet Client SDK for installing NuGet packages:

- [Revisiting the NuGet v3 Libraries](https://martinbjorkstrom.com/posts/2018-09-19-revisiting-nuget-client-libraries)
