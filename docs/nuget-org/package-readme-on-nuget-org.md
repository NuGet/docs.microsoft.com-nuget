---
title: Package Readme on NuGet.org
description: Detailed explanation of how Readme files on NuGet.org are rendered and what to do when you run into issues.
author: chgill-MSFT
ms.author: chgill
ms.date: 02/23/2021
ms.topic: conceptual
ms.reviewer: anangaur
---

# Package Readme on NuGet.org

[Include a Readme file in your NuGet package](https://docs.microsoft.com/nuget/reference/msbuild-targets#packagereadmefile) to make your package details richer and more informative for your users!

This is likely one of the first elements users will see when they view your package details page on NuGet.org and is essential to making a good impression!

## What should my Readme include?

Consider including the following items in your readmes:
* An introduction to what your package is and does - what problems does it solve?
* How to get started with your package - are there any specific requirements?
* Links to more comprehensive documentation if not included in the readme itself.
* At least a few code snippets/samples or example images.
* Where and how to leave feedback such as link to the project issues, Twitter, bug tracker, or other platform.
* How to contribute, if applicable.

Keep in mind, high quality readmes can come in a wide variety of formats, shapes, and sizes! If you already have a package available on NuGet.org, chances are that you already have a `readme.md` file at in your repository that would be a great addition to your NuGet.org details page.

## Preview your Readme

To preview your Readme file before it's live on NuGet.org, upload your package using the [Upload Package web portal on NuGet.org](https://docs.microsoft.com/nuget/nuget-org/publish-a-package#web-portal-use-the-upload-package-tab-on-nugetorg) and scroll down to the "Readme File" section of the metadata preview. It should look something like this:

![Readme File preview](media\readme-upload-preview.PNG)

Consider taking time to review and preview your Readme file for [image compliance](#allowed-domains-for-images-and-badges) and formatting to make sure gives a great first impression to potential users! To correct mistakes on a Readme file on NuGet.org, you will need to push an updated package version, so making sure everything looks good in advance may save you headache down the road.
## Allowed domains for images and badges

Due to security and privacy concerns, NuGet.org restricts the domains from which images and badges can be rendered to trusted hosts. 

NuGet.org allows all images, including badges, from the following trusted domains to be rendered:
* api.bintray.com
* api.codacy.com
* api.codeclimate.com
* api.dependabot.com
* api.travis-ci.com
* api.travis-ci.org
* app.fossa.io
* badge.fury.io
* badgen.net
* badges.gitter.im
* bettercodehub.com
* buildstats.info
* ci.appveyor.com
* circleci.com
* codecov.io
* codefactor.io
* coveralls.io
* gitlab.com
* img.shields.io
* isitmaintained.com
* opencollective.com
* snyk.io
* sonarcloud.io
* raw.github.com
* raw.githubusercontent.com
* user-images.githubusercontent.com
* camo.githubusercontent.com

If you feel that a another domain should be added to the allow-list, please feel free to [file an issue](https://github.com/NuGet/NuGetGallery/issues) and it will be reviewed by our engineering team for privacy and security compliance. Images with relative local paths and images hosted from unsupported domains will not be rendered and will produce a warning on the Readme file preview and package details page that is only visible to the package owners.
