---
title: How to Publish a NuGet Package
description: Detailed explanation of how Readme files on NuGet.org are rendered and what to do when you run into issues.
author: chgill-MSFT
ms.author: chgill
ms.date: 02/23/2021
ms.topic: conceptual
ms.reviewer: anangaur
---

# Readme files on NuGet.org

[Include a Readme file in your NuGet package](https://docs.microsoft.com/nuget/reference/msbuild-targets#packagereadmefile) to make your package details richer and more informative for your users!

This is likely one of the first elements users will see when they view your package details page on NuGet.org and is essential to making a good impression!

## What should be in my Readme

The information you include in your Readme will vary depending on the type, purpose, and scope of your package. It doesn't need be a single comprehensive document for how to use your package, but it should contain the following items at minimum:
* An introduction to what your package is and what it does. What problems does it solve?
* How to get started with your package. Are there any specific requirements (framework, project type, OS)?
* A link to comprehensive documentation if applicable.
* Code snippets/samples or example images/screenshots.
* Where and how to leave feedback such as link to the project issues, Twitter, bug tracker, or other platform.
* How to contribute, if applicable.

## Preview your Readme

To preview your Readme file before it's live on NuGet.org, upload your package using the [Upload Package web portal on NuGet.org](https://docs.microsoft.com/nuget/nuget-org/publish-a-package#web-portal-use-the-upload-package-tab-on-nugetorg) and scroll down to the "Readme File" section of the metadata preview. It should look something like this:

![Readme File preview](../media)



## Domain allow-list for images and badges

Due to security and privacy concerns, NuGet.org restricts the domains from which images and badges (SVGs) can be rendered to trusted hosts. 

NuGet.org allows badges from the following trusted domains to be rendered:
