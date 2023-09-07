---
title: NuGet 2.6 Release Notes
description: Release notes for NuGet 2.6 including known issues, bug fixes, added features, and DCRs.
author: JonDouglas
ms.author: jodou
ms.date: 11/11/2016
ms.topic: conceptual
---

# NuGet 2.6 Release Notes

[NuGet 2.5 Release Notes](../release-notes/nuget-2.5.md) | [NuGet 2.6.1 for WebMatrix Release Notes](../release-notes/nuget-2.6.1-for-webmatrix.md)

NuGet 2.6 was released on June 26, 2013.

## Notable features in the release

### Support for Visual Studio 2013

NuGet 2.6 is the first release that provides support for Visual Studio 2013. And
like Visual Studio 2012, the NuGet Package Manager extension is included in every
edition of Visual Studio.

In order to provide the best possible support for Visual Studio 2013 while still
supporting both Visual Studio 2010 and Visual Studio 2012, and keeping the extension
sizes as small as possible, we are producing a separate extension for Visual Studio
2013 while the original extension continues to target both Visual Studio 2010 and 2012.

Starting with NuGet 2.6, we will publish two extensions as below:

1. [NuGet Package Manager](https://marketplace.visualstudio.com/items?itemName=NuGetTeam.NuGetPackageManager) (applies to Visual Studio 2010 and 2012)
1. [NuGet Package Manager for Visual Studio 2013](https://marketplace.visualstudio.com/items?itemName=NuGetTeam.NuGetPackageManagerforVisualStudio2013)

With this split, the [nuget.org](https://nuget.org) home page's "Install NuGet" button takes you to the [installing NuGet](../install-nuget-client-tools.md) page, where you can find more information about installing the different NuGet clients.

<a name="xdt"></a>

### XDT Web.config transformation support

One of the most highly-requested features for the NuGet client has been to support more
powerful XML transformations using the XDT transformation engine which is used in Visual
Studio build configuration transformations.

In April 2013, we made two big announcements regarding NuGet support for XDT. The first
was that the XDT library itself was being itself [released as a NuGet package](https://nuget.org/packages/Microsoft.Web.Xdt)
and [open sourced on CodePlex](http://xdt.codeplex.com/). This step enabled the XDT engine
to be used freely by other open-source software, including the NuGet client. The second
announcement was the plan to support use of the XDT engine for transformations in the
NuGet client. NuGet 2.6 includes this integration.

#### How it works

To take advantage of NuGet’s XDT support, the mechanics look similar to those of the
[current config transformation feature](../create-packages/source-and-config-file-transformations.md).
Transformation files are added to the package’s content folder. However, while config
transformations use a single file for both installation and uninstallation, XDT
transformations enable fine-grained control over both of these processes using the
following files:

- Web.config.install.xdt
- Web.config.uninstall.xdt

Additionally, NuGet uses the file suffix to determine which engine to run for transformations,
so packages using the existing web.config.transforms will continue to work. XDT transformations
can also be applied to any XML file (not just web.config), so you can leverage this for other
applications in your project.

#### What you can do with XDT

One of XDT’s greatest strengths is its [simple but powerful syntax](/previous-versions/aspnet/dd465326(v=vs.110))
for manipulating the structure of an XML DOM. Rather than simply overlaying one fixed document
structure onto another structure, XDT provides controls for matching elements in a variety of
ways, from simple attribute name matching to full XPath support. Once a matching element or
set of elements is found, XDT provides a rich set of functions for manipulating the elements,
whether that means adding, updating, or removing attributes, placing a new element at a specific
location, or replacing or removing the entire element and its children.

### Machine-Wide Configuration

One of the great strengths of NuGet is that it breaks down an otherwise large executable
or library into a set of modular components which can be integrated, and most importantly
maintained and versioned independently. One side effect of this, however, is that the
conventional idea of a product or product family becomes potentially more fragmented.
NuGet’s custom package source feature provides one way of organizing packages; however,
custom package sources are not discoverable on their own.

NuGet 2.6 extends the logic for configuring NuGet by searching the folder hierarchy
under the path %ProgramData%/NuGet/Config. Product installers can add custom NuGet
configuration files under this folder to register a custom package source for their
products. Additionally, the folder structure supports semantics for product, version,
and even SKU of the IDE. Settings from these directories are applied in the following
order with a "last in wins" precedence strategy.

1. %ProgramData%\NuGet\Config\*.config
2. %ProgramData%\NuGet\Config\{IDE}\*.config
3. %ProgramData%\NuGet\Config\{IDE}\{Version}\*.config
4. %ProgramData%\NuGet\Config\{IDE}\{Version}\{SKU}\*.config

In this list, the {IDE} placeholder is specific to the IDE in which NuGet is running,
so in the case of Visual Studio, it will be "VisualStudio". The {Version} and {SKU}
placeholders are provided by the IDE (e.g. "11.0" and "WDExpress", "VWDExpress" and
"Pro", respectively). The folder can then contain many different *.config files.
Therefore, the ACME component company can, as a part of their product installer, add
a custom package source which will be visible only in the Professional and Ultimate
versions of Visual Studio 2012 by creating the following file path:

%ProgramData%\NuGet\Config\VisualStudio\11.0\Pro\acme.config

While the folder structure makes it straightforward for programs like software
installers to add machine-wide package sources to NuGet's configuration, the NuGet
configuration dialog has also been updated to allow for the registration of package
sources as either user-specific (e.g. registered in %AppData%/NuGet/NuGet.Config) or machine-wide.

This feature is utilized by Visual Studio 2013, where a file is installed at:

%ProgramData%\NuGet\Config\VisualStudio\12.0\Microsoft.VisualStudio.config

Within this file, a new package source called ".NET Framework Packages" is configured.

![NuGet Config File machine wide settings](./media/NuGet-Config-File-Machine-Wide.png)

### Contextualizing Search

As the number of packages served by the NuGet gallery continues to grow at an exponential
pace, improving search remains ever at the top of the NuGet priority list. One of the
planned features for NuGet is contextual search, meaning that NuGet will use information
about the version and SKU of Visual Studio that you are using and the type of project that
you are building as criteria for determining the relevance of potential search results.

Starting with NuGet 2.6, each time a package is installed, the context for the installation
is recorded as part of the installation operation data.  Searches also send the same context
information, which will allow the NuGet Gallery to boost search results by contextual
installation trends.  A future update to the NuGet Gallery will enable this context-sensitive
relevance boosting.

### Tracking Direct Installs vs. Dependency Installs

Package authors are relying more and more on the [Package Statistics](http://blog.nuget.org/20130226/Introducing-Package-Statistics.html)
provided on the NuGet Gallery.  One significant missing data point that authors have asked
for is a differentiation between direct package installs and dependency installs.  Until now,
the NuGet client did not send any context around the installation operation for whether the
developer directly installed the package or if it was installed to satisfy a dependency.
Starting with NuGet 2.6, that data will now be sent for the installation operation.  Package
Statistics on the NuGet Gallery will expose that data as separate install operations, with
a "-Dependency" suffix.

* Install
* Install-Dependency
* Update
* Update-Dependency
* Reinstall
* Reinstall-Dependency

In addition to the different operation name, the dependent package id is also recorded for the
installation.  A future update to the NuGet Gallery will expose that data within reports, allowing
package authors to fully understand how developers are installing their packages.

## Bug Fixes

NuGet 2.6 also includes several bug fixes. For a full list of work items fixed in NuGet 2.6, please view
the [NuGet Issue Tracker for this release](https://blog.nuget.org/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=NuGet%202.6&assignedTo=All&component=All&sortField=LastUpdatedDate&sortDirection=Descending&page=0&reasonClosed=All).