# An Overview of the NuGet Ecosystem

First introduced in 2010, NuGet has been around for a few years now and many people and organizations are starting to realize that NuGet presents a great opportunity to improve and automate different aspects of the development processes. Because the NuGet project is open source under a permissive [Apache v2 license](http://choosealicense.com/licenses/apache/), other projects can leverage NuGet and companies can build support for it in their products. Whether for open source projects or enterprise application development, NuGet plus the ever-growing set of applications built on and around NuGet, provide a broad selection of tools for improving your software development process.

All of these projects are able to innovate for the same reason that has allowed NuGet to innovate - You. As such, please contribute to these projects just as you contribute to NuGet by reporting defects and new feature ideas, providing feedback, writing documentation, and contributing code where possible. 

## .NET Foundation Projects
The NuGet project provides a free, open source package management system for the Microsoft development platform and consists out of a few client toolsas well as the set of services that comprise the [official NuGet Gallery](http://www.nuget.org). Combined, these form the NuGet project which is governed by the [.NET Foundation](http://www.dotnetfoundation.org/).

The NuGet project has migrated from codeplex into our new home in GitHub. The NuGet Organization contains various repositories and the [Home Repo](https://github.com/Nuget/Home) gives an overview to repos and where the various components of the NuGet platform are developed.

## Microsoft Projects
Microsoft has extensively contributed to the development of the NuGet project. All contributions made by Microsoft employees are also open source and are donated (including copyrights) to the .NET Foundation.

(TBD) - Super Brief Overview of our components

## Non-Microsoft Projects
In addition to Microsoft, many other individuals and companies have made significant contributions to the NuGet ecosystem. Each project listed here may have a different license than the core NuGet components so please confirm that the license terms are acceptable prior to use. 

* [AppVeyor CI](https://www.appveyor.com/)
* [Artifactory](https://www.jfrog.com/artifactory/)
* [BoxStarter](http://boxstarter.org/)
* [Chocolatey](https://chocolatey.org/)
* [CoApp](http://coapp.org/)
* [JetBrains ReSharper](https://resharper-plugins.jetbrains.com/)
* [JetBrains TeamCity](https://www.jetbrains.com/teamcity/)
* [Klondike](https://github.com/themotleyfool/Klondike)
* [MyGet](http://www.myget.org/) (or NuGet-as-a-Service)
* [NuGet Package Explorer](https://github.com/NuGetPackageExplorer/NuGetPackageExplorer)
* [NuGet Server](http://nugetserver.net/)
* [OctopusDeploy](https://octopus.com/)
* [Paket](https://fsprojects.github.io/Paket/)
* [ProGet](http://inedo.com/proget) (Inedo)
* [scriptcs](http://scriptcs.net/)
* [SharpDevelop](http://community.sharpdevelop.net/blogs/mattward/archive/2011/01/23/NuGetSupportInSharpDevelop.aspx)
* [Sonatype Nexus](http://www.sonatype.com/nexus-repository-sonatype)
* [SymbolSource](http://www.symbolsource.org/Public)
* [Xamarin and MonoDevelop](https://github.com/mrward/monodevelop-nuget-addin)


## Other NuGet-based utilities
There are quite a few other tools and utilities building further on top of NuGet. Here's a list of what I've found interesting:

* [Glimpse Extensions](http://getglimpse.com/Packages) (plug-ins are packages)
* [NuGetMustHaves.com](http://nugetmusthaves.com/)
* [NuGetFeed](http://nugetfeed.org/) (build a list of favorite packages)
* [Orchard](http://www.orchardproject.net/) (CMS modules are fetched from a v1 NuGet feed hosted in the Orchard Gallery)
* [Java implementation of NuGet Server](http://blog.jonnyzzz.name/2012/03/nuget-server-in-pure-java.html)
* [NuGetLatest](https://twitter.com/NuGetLatest) (Twitter bot tweeting new package publications)
* [HotNuget](http://hotnuget.com) (Compare package usage)
* [DefinitelyTyped](http://definitelytyped.org/) ([Automatic](https://github.com/DefinitelyTyped/NugetAutomation/) TypeScript Type [Definitions published to NuGet](http://www.nuget.org/packages?q=DefinitelyTyped))

## Training Materials and References
Using a new tool or technology usually comes with a learning curve. Luckily for you, NuGet has no steep learning curve it all! In fact, anyone can [get started consuming packages in no time](http://www.youtube.com/watch?feature=player_embedded&v=PboPfoptU2c). Authoring packages however, and especially authoring good packages, as well as embracing NuGet in your automated build and deployment processes requires some research in order to get things right.

The following pointers should help you get the maximum out of NuGet:

* Official NuGet Documentation site: you are here :-)
* NuGet Blog: [http://blog.nuget.org/](http://blog.nuget.org/)
* NuGet team on Twitter: [@nuget](http://twitter.com/nuget)
* Books: [Apress Pro NuGet](http://bit.ly/ProNuGet), [NuGet 2 Essentials](http://www.amazon.com/NuGet-2-Essentials-Damir-Arh-ebook/dp/B00GTQD5M4)

## Documentation for Individual Packages

[NuDoq](http://nudoq.org) provides the missing link between straightforward access and updates to NuGet packages, and their corresponding API documentation.

NuDoq regularly polls the NuGet.org gallery server for the latest package updates, unpacks and processes the library documentation files, and update the site accordingly.

## Adding Your Project
If you have a NuGet ecosystem project that would be a valuable addition to this page, please  submit a pull request with an edit to [this page](https://github.com/NuGet/NuGetDocs/tree/master/NuGet.Docs/Contribute/Ecosystem.md).
