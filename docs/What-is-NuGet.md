---
# required metadata

title: What is NuGet and what does it do? | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 6/14/2017
ms.topic: article
ms.prod: nuget
#ms.service:
ms.technology: null
ms.assetid: c3faf278-4cbf-4733-96f6-9ee9f7203af9

# optional metadata

description: A comprehensive introduction to what NuGet is and does
keywords: NuGet package manager, consumption, package creation, package hosting
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

# A simple introduction to NuGet

A mechanism through which developers can create, distribute, and consume useful libraries and packages with each other is essential for any modern development platform. For .NET, that mechanism is **NuGet**, first released in early 2011, which defines how packages for .NET are created, hosted, and consumed and provides the tools for each of those roles.

## The flow of packages between creators, hosts, and consumers

With hosting, NuGet itself maintains the central repository for over 80,000 unique, publicly-available packages at [nuget.org](https://www.nuget.org), which are employed by millions of .NET developers. NuGet also enables you to host packages privately in the cloud, on a private network, or even on just your local file system, making packages available to only those developers within a particular organization or customer group. The options are explained on [Hosting your own NuGet feeds](../hosting/overview.md).

Whatever its nature, the host serves as a point of connection between package *creators* and package *consumers*. Creators build useful packages and publish them to a host. Consumers then search for useful and compatible packages on accessible NuGet hosts, downloading and including those packages in their projects which makes the packages' APIs available to the rest of the project code.

![Relationship between package creators, package hosts, and package consumers](media/nuget-roles.png)

A "compatible" package in this case means that it contains assemblies that are built for at least one target .NET framework that's compatible with the consuming project's target framework. To make a package widely compatible, it's creator compiles separate assemblies for each target framework and includes all of them in the same package. When a consumer installs that package, NuGet extracts only those assemblies that are needed by the project, thereby minimizing the package's footprint in the final application and/or assemblies produced by that project.

## NuGet tools

In addition to providing the public nuget.org host, NuGet also provides a variety of tools used by both creators and consumers:

| Tool | Platforms | Applicable Scenarios | Description |
| --- | --- | --- | --- |
| [nuget.exe CLI](../tools/nuget-exe-cli-reference.md) | All | Creation, Consumption | Provides all NuGet capabilities, with some commands applying specifically to package creators, some applying only to consumers, and others applying to both. For example, the `nuget pack` command creates a package from various assemblies and related files and is used exclusively by package creators. The `nuget install` command installs a package into a project and is used by consumers. The `nuget config` command is used by everyone to set NuGet configuration variables.  |
| [Package Manager UI](../tools/package-manager-ui.md) | Visual Studio on Windows | Consumption | Provides an easy-to-use UI through which developers can install and manage packages in .NET projects. | 
| [Package Manager Console](../tools/package-manager-console.md) | Visual Studio on Windows | Consumption | Provides [PowerShell commands](../tools/Powershell-Reference.md) for installing and managing packages in .NET projects. | 
| [dotnet CLI](../tools/dotnet-commands.md) | All | Creation, Consumption | Provides certain NuGet CLI capabilities directly within the .NET Core toolchain. |
| [MSBuild](../schema/msbuild-targets.md) | Windows | Creation, Consumption | Provides the ability to create packages and restore packages used in a project directly through the MSBuild toolchain. |

As you can see, the tools you work with depend greatly on whether you're creating (and publishing) packages or consuming them, and the platform you're working on. More specific details can be found in the [Package creation workflow](./Create-Packages/Overview-and-Workflow.md) and [Package consumption workflow](./Consume-Packages/Overview-and-Workflow.md) topics, along with other topics in those sections. 

Package creators are typically also consumers, as they build on top of functionality that exists in other NuGet packages. And those packages, of course, may in turn depend on still others.

## Managing dependencies and package restore

The ability to easily build on the work of others is one of the things that makes a package management system so powerful, and thus much of what NuGet does is managing that dependency tree or "graph" on behalf of your project. Simply said, all that any given project should concern itself with are only those packages that it's using directly. NuGet can worry about all other down-level dependencies.

It's also important to manage those dependencies in such a way that projects can easily move between developer computers, source control repositories, build servers, and so forth. For this reason it's highly impractical to keep binary assemblies from NuGet packages directly bound to a project. Not only would this make each copy of the project unnecessarily bloated (and thereby waste space in source control repositories), it would also make it very difficult to update package binaries to newer versions as this would have to be done across all copies of the project. 

Instead, NuGet simply maintains a reference list of the packages upon which a project depends, and provides the means to restore all referenced packages upon request. That is, whenever you install a package from some host into a project, NuGet records the package identifier and version number in this reference list. (Uninstalling a package, of course, removes it from the list.) When it comes time to commit the project to source control (or to share is some other way), you need include only the reference list and not any of the package binaries, because those packages can simply be reinstalled&mdash;restores&mdash;from a public and/or private hosts at any later time using only the reference list. (For this reason, nuget.org does not allow permanent deletion of published packages, although they can be hidden; see [Deleting packages](policies/deleting-packages.md).)

Thus if the project is copied onto a build server as part of an automated deployment system, for example, the build system can simply ask NuGet to restore dependencies at the beginning of the process. Build systems like Visual Studio Team Services provide NuGet package restore steps for this exact purpose. Similarly, when developers obtain a copy of a project (as when cloning a repository) then open the project in Visual Studio and run a build, Visual Studio automatically restores the necessary NuGet packages. Developers can also tell NuGet to restore packages at any time using the the `nuget restore` CLI command or the `Install-Package` cmdlet in the Package Manager Console.

Clearly, then, NuGet's primarily role where developers are concerned is maintaining that reference list on behalf of your project and providing the means to efficiently restore (and update) those referenced packages.

How this exactly happens has evolved over the different versions of NuGet:

- [`packages.config`](../schema/packages.config.md): *(NuGet 1.0+)* An XML file that maintains a flat list of all dependencies in the project, including the dependencies of other installed packages. 
- [`project.json`](../schema/project-json.md): *(NuGet 3.0+)* A JSON file that maintains a list of the project's dependencies with an overall package graph in an associated file, `project.lock.json`. This structure provides improved performance over `packages.config` as described on [Dependency Resolution](../consume-packages/dependency-resolution.md).
- [Package references in project files](../consume-packages/package-references-in-project-files.md) (also known as "PackageReference") | *(NuGet 4.0+)* Maintains a list of a project's top-level dependencies directly within the project file, so no separate file is needed. An associated file, `project.assets.json`, is dynamically generated like `project.lock.json` to manage the overall dependency graph.

To check what method is in use for project, simply look for `packages.config` or `project.json` in the project root after installing your first package. If you don't see either file, look in the project file directly for a &lt;PackageReference&gt;element. This is the case for most new projects created in Visual Studio 2017.

You can also switch from one method to another. NuGet literally doesn't care which method you use, so long as you have the appropriate version of [NuGet installed](../guides/install-nuget.md). Older projects that use `packages.config` can be converted to use either `project.json` or PackageReference, and projects using `project.json` can also be converted to use PackageReference. For each package, simply create the appropriate reference element in the new file using the same package identifier and version number shown in the older file. ((TODO: make a topic for this migration showing examples.)) When it comes time to restore or update packages, NuGet will find whatever method you're using.

## What else does NuGet do?

So far we've seen NuGet's central hosting role with nuget.org, its support for private hosting, the tools it provides for creating, publishing, and consuming packages, and its most important function of maintaining a reference list of packages used in a project along with the ability to restore and update those packages.

To make these processes work efficiently, NuGet does some behind-the-scenes optimizations. Most notably, NuGet manages both machine-wide and project-specific package caches to shortcut installation and reinstallation. Where the machine-wide cache is concerned, any package that you download and install in a project is stored in the cache, such that installing the same package in another project doesn't have to download it again. This is clearly very helpful when you're frequently restoring a larger number of packages, as on a build server. For more details on the mechanism and how to work with it, see [Managing the NuGet cache](../consume-packages/managing-the-nuget-cache.md).

Within an individual project, NuGet does a lot of work to manage the overall dependency graph, which includes resolving multiple references to different versions of the same package. (When using `project.json` or &lt;PackageReference&gt;, NuGet keeps that information in a secondary file called `project.lock.json` and `project.assets.json`, respectively.) That is, it's quite common that a project takes a dependency on one or more packages that themselves had the same dependencies. For example, the most popular package on nuget.org, Newtonsoft.Json, is quite frequently used by application and packages alike to manage JSON data, so in the entire dependency graph you could easily have ten different references to Newtonsoft.Json. At the same time, you don't want to bring multiple versions of that package into the application itself, so NuGet sorts out which single version that everyone can use. (See Dependency Resolution](consume-packages/dependency-resolution.md) for more on this topic.)

Beyond that, NuGet maintains all the specifications related to how packages are structured (including [localization](Create-Packages/Creating-Localized-Packages.md) and [debug symbols](Create-Packages/Symbol-Packages.md) , how they are referenced (including [version ranges](consume-packages/dependency-versions.md) and [pre-release versions](create-packages/Prerelease-Packages.md), and provides APIs for credential providers (for accessing private hosts) and developer who write Visual Studio extensions and project templates...

Take a moment to browse the table of contents for this documentation, and you'll see all of these capabilities represented there, along with release notes dating back to NuGet's beginnings.

## Comments, contributions, and issues

Finally, we very much welcome comments and contributions to this documentation&mdash;just select the **Comments** and **Edit** commands on any page, or visit the [docs repository](https://github.com/NuGet/docs.microsoft.com-nuget/) and [docs issue list](https://github.com/NuGet/docs.microsoft.com-nuget/issues) on GitHub.

We also welcome contributions to NuGet itself through its [various GitHub repositories](https://github.com/NuGet); NuGet issues can be found on [https://github.com/NuGet/home/issues](https://github.com/NuGet/home/issues).

Enjoy your NuGet experience!