---
title: Overview of the NuGet Ecosystem
description: Comprehensive resources in the NuGet ecosystem including NuGet sources, non-Microsoft NuGet projects, utilities, and training materials.
author: JonDouglas
ms.author: jodou
ms.date: 01/18/2018
ms.topic: conceptual
---

# An overview of the NuGet ecosystem

Since it's introduction in 2010, NuGet has presented a great opportunity to improve and automate different aspects of the development processes.

Because NuGet is open source under a permissive [Apache v2 license](http://choosealicense.com/licenses/apache/), other projects can leverage NuGet and companies can build support for it in their products. Whether for open-source projects or enterprise application development, NuGet and other applications built on and around NuGet provide a broad ecosystem of tools for improving your software development process.

All of these projects are able to innovate because of developer contributions. Just as you contribute to NuGet itself, also make contribution to these projects by reporting defects and new feature ideas, providing feedback, writing documentation, and contributing code where possible.

## .NET Foundation projects

NuGet provides a free, open source package management system for the Microsoft development platform. It consists of a few client tools as well as the set of services that comprise the [official NuGet Gallery](https://www.nuget.org). Combined, these form the NuGet project which is governed by the [.NET Foundation](https://www.dotnetfoundation.org/).

The NuGet Organization contains various repositories on GitHub. [https://github.com/Nuget/Home](https://github.com/Nuget/Home) gives an overview of all the repositories and where to find the various NuGet components.

## Microsoft projects

Microsoft has contributed extensively to the development of NuGet. All contributions made by Microsoft employees are also open source and are donated (including copyrights) to the .NET Foundation.

## Non-Microsoft projects

Many other individuals and companies have made significant contributions to the NuGet ecosystem. Each project listed here may have a different license than the core NuGet components, so confirm that the license terms are acceptable prior to use:

- [AppVeyor CI](https://www.appveyor.com/)
- [Artifactory](https://www.jfrog.com/artifactory/)
- [BoxStarter](http://boxstarter.org/)
- [Chocolatey](https://chocolatey.org/)
- [CoApp](https://github.com/coapp/coapp.org)
- [JetBrains ReSharper](https://resharper-plugins.jetbrains.com/)
- [JetBrains Space](https://www.jetbrains.com/space/)
- [JetBrains TeamCity](https://www.jetbrains.com/teamcity/)
- [Klondike](https://github.com/themotleyfool/Klondike)
- [MinimalNugetServer](https://github.com/TanukiSharp/MinimalNugetServer)
- [MyGet (or NuGet-as-a-service)](http://www.myget.org/)
- [NuGet Package Explorer](https://github.com/NuGetPackageExplorer/NuGetPackageExplorer)
- [NuGet Server](http://nugetserver.net/)
- [NuGetizer](https://github.com/devlooped/nugetizer)
- [OctopusDeploy](https://octopus.com/)
- [Paket](https://fsprojects.github.io/Paket/)
- [ProGet (Inedo)](http://inedo.com/proget)
- [scriptcs](http://scriptcs.net/)
- [SharpDevelop](http://community.sharpdevelop.net/blogs/mattward/archive/2011/01/23/NuGetSupportInSharpDevelop.aspx)
- [Sonatype Nexus](http://www.sonatype.com/nexus-repository-sonatype)
- [SymbolSource](http://www.symbolsource.org/Public)
- [Xamarin and MonoDevelop](https://github.com/mrward/monodevelop-nuget-addin)

## Other NuGet-based utilities

These are tools and utilities built on NuGet:

- [Glimpse Extensions](https://meetglimpse.com/)
- [NuGetMustHaves.com](http://nugetmusthaves.com/)
- [Orchard](http://www.orchardproject.net/) (CMS modules are fetched from a v1 NuGet feed hosted in the Orchard Gallery)
- [Java implementation of NuGet Server](http://jonnyzzz.com/blog/2012/03/07/nuget-server-in-pure-java/)
- [NuGetLatest](https://twitter.com/NuGetLatest) (Twitter bot tweeting new package publications)
- [DefinitelyTyped](http://definitelytyped.org/) ([Automatic](https://github.com/DefinitelyTyped/NugetAutomation/) TypeScript Type [Definitions published to NuGet](https://www.nuget.org/packages?q=DefinitelyTyped))

## Training materials and references

Using a new tool or technology usually comes with a learning curve. Luckily for you, NuGet has no steep learning curve it all! In fact, anyone can [get started consuming packages](../quickstart/install-and-use-a-package-in-visual-studio.md) quickly.

That said, authoring packages–and especially good packages–along with  embracing NuGet in automated build and deployment processes, requires spending a little more time with the following resources:

- [NuGet Blog](https://blog.nuget.org/)
- [NuGet team on Twitter, @nuget](http://twitter.com/nuget)
- Books:
  - [Apress Pro NuGet](http://bit.ly/ProNuGet)
  - [NuGet 2 Essentials](http://www.amazon.com/NuGet-2-Essentials-Damir-Arh-ebook/dp/B00GTQD5M4)

## Documentation for individual packages

[NuDoq](http://nudoq.org) provides straightforward access and updates and documentation for NuGet packages.

NuDoq regularly polls the nuget.org gallery server for the latest package updates, unpacks and processes the library documentation files, and updates the site accordingly.

## Adding your project

If you have a NuGet ecosystem project that would be a valuable addition to this page, please  submit a pull request with an edit to this page.
