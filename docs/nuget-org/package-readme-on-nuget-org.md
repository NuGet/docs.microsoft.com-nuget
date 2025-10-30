---
title: Package readme on NuGet.org
description: Detailed explanation of how readme files on NuGet.org are rendered and what to do when you run into issues.
author: nkolev92
ms.author: nikolev
ms.date: 08/31/2022
ms.topic: article
ms.reviewer: anangaur
---

# Package readme on NuGet.org

[Include a readme file in your NuGet package](/nuget/reference/msbuild-targets#packagereadmefile) to make your package details richer and more informative for your users!

This is likely one of the first elements users will see when they view your package details page on NuGet.org and is essential to making a good impression!

> [!IMPORTANT]
> NuGet.org only supports readme files in [Markdown](https://daringfireball.net/projects/markdown/) and images from a limited set of domains. See our [allowed domains for images](#allowed-domains-for-images-and-badges) and [supported Markdown features](#supported-markdown-features) to ensure your readme renders correctly on NuGet.org.

## What should my readme include?

Consider including the following items in your readme:
* An introduction to what your package is and does - what problems does it solve?
* How to get started with your package - are there any specific requirements?
* Links to more comprehensive documentation if not included in the readme itself.
* At least a few code snippets/samples or example images.
* Where and how to leave feedback such as link to the project issues, Twitter, bug tracker, or other platform.
* How to contribute, if applicable.

For example, you can start with this package README template:

```text
# Package readme title, e.g., display name or title of the package (optional)

Start with a clear and concise description: A brief overview of what your package is and does, also what problem it solves.

## Getting started

Explain how to use your package, provide clear and concise getting started instructions, including any necessary steps.

### Prerequisites

What are specific minimum requirements to use your packages? Consider excluding this section if your package works without any additional setup beyond simple package installation.

## Usage

Examples about how to use your package by providing code snippets/example images, or samples links on GitHub if applicable. 

- Provide sample code using code snippets
- Include screenshots, diagrams, or other visual help users better understand how to use your package

## Additional documentation

Provide links to more resources: List links such as detailed documentation, tutorial videos, blog posts, or any other relevant documentation to help users get the most out of your package.

## Feedback

Where and how users can leave feedback?

- Links to a GitHub repository where could open issues, Twitter, a Discord channel, bug tracker, or other platforms where a package consumer can connect with the package author.
```

Keep in mind, high quality readmes can come in a wide variety of formats, shapes, and sizes! If you already have a package available on NuGet.org, chances are that you already have a `readme.md` or other documentation file in your repository that would be a great addition to your NuGet.org details page.

> [!Note]
> Read through our [blog on writing a high-quality README](https://devblogs.microsoft.com/nuget/write-a-high-quality-readme-for-nuget-packages/) for some best practices.

## Preview your readme

To preview your readme file before it's live on NuGet.org, upload your package using the [Upload Package web portal on NuGet.org](/nuget/nuget-org/publish-a-package#web-portal-use-the-upload-package-tab-on-nugetorg) and scroll down to the "Readme File" section of the metadata preview. It should look something like this:

![Readme File preview](media\readme-upload-preview.PNG)

Consider taking time to review and preview your readme file for [image compliance](#allowed-domains-for-images-and-badges) and [supported formatting](#supported-markdown-features) to make sure it gives a great first impression to potential users! To correct mistakes on your package readme once it's published to NuGet.org, you will need to push an updated package version with the fix. Making sure everything looks good in advance may save you headache down the road.
## Allowed domains for images and badges

Due to security and privacy concerns, NuGet.org restricts the domains from which images and badges can be rendered to trusted hosts. 

NuGet.org allows all images, including badges, from the following trusted domains to be rendered:
* api.codacy.com
* api.codeclimate.com
* api.dependabot.com
* api.reuse.software
* api.travis-ci.com
* app.codacy.com
* app.deepsource.com
* avatars.githubusercontent.com
* badgen.net
* badges.gitter.im
* camo.githubusercontent.com
* caniuse.bitsofco.de
* cdn.jsdelivr.net
* cdn.syncfusion.com
* ci.appveyor.com
* circleci.com
* cloudback.it
* codecov.io
* codefactor.io
* coveralls.io
* dev.azure.com
* flat.badgen.net
* github.com/.../workflows/.../badge.svg
* gitlab.com
* i.imgur.com
* img.shields.io
* infragistics.com
* isitmaintained.com
* media.githubusercontent.com
* opencollective.com
* raw.github.com
* raw.githubusercontent.com
* snyk.io
* sonarcloud.io
* travis-ci.com
* travis-ci.org
* user-images.githubusercontent.com

If you feel that another domain should be added to the allow-list, please feel free to [file an issue](https://github.com/NuGet/NuGetGallery/issues) and it will be reviewed by our engineering team for privacy and security compliance. Images with relative local paths and images hosted from unsupported domains will not be rendered and will produce a warning on the readme file preview and package details page that is only visible to the package owners.

## Supported Markdown features
[Markdown](https://daringfireball.net/projects/markdown/) is a lightweight markup language with plain text formatting syntax. NuGet.org readmes support [CommonMark](https://commonmark.org/) compliant Markdown through the [Markdig](https://github.com/lunet-io/markdig) parsing engine.

NuGet.org currently supports the following Markdown features:
* [Headers](https://spec.commonmark.org/0.29/#atx-headings)
* [Images](https://spec.commonmark.org/0.29/#images)
* [Extra emphasis](https://github.com/xoofx/markdig/blob/master/src/Markdig.Tests/Specs/EmphasisExtraSpecs.md)
* [Lists](https://spec.commonmark.org/0.29/#lists)
* [Links](https://spec.commonmark.org/0.29/#links)
* [Block quotes](https://spec.commonmark.org/0.29/#block-quotes)
* [Backslash escapes](https://spec.commonmark.org/0.29/#backslash-escapes)
* [Code spans](https://spec.commonmark.org/0.29/#code-spans)
* [Task lists](https://github.com/xoofx/markdig/blob/master/src/Markdig.Tests/Specs/TaskListSpecs.md)
* [Tables](https://github.com/xoofx/markdig/blob/master/src/Markdig.Tests/Specs/PipeTableSpecs.md)
* [Emojis](https://github.com/xoofx/markdig/blob/master/src/Markdig.Tests/Specs/EmojiSpecs.md)
* [Auto-links](https://github.com/xoofx/markdig/blob/master/src/Markdig.Tests/Specs/AutoLinks.md)

We also support syntax highlighting, You can add an language identifier to enable syntax highlighting in your code spans.
