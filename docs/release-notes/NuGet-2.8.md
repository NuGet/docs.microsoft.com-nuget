---
title: NuGet 2.8 Release Notes
description: Release notes for NuGet 2.8 including known issues, bug fixes, added features, and DCRs.
author: JonDouglas
ms.author: jodou
ms.date: 11/11/2016
ms.topic: release-notes
---

# NuGet 2.8 Release Notes

[NuGet 2.7.2 Release Notes](../release-notes/nuget-2.7.2.md) | [NuGet 2.8.1 Release Notes](../release-notes/nuget-2.8.1.md)

NuGet 2.8 was released on January 29, 2014.

## Acknowledgements

1. ```[Llewellyn Pritchard](https://www.codeplex.com/site/users/view/leppie)``` ([@leppie](https://twitter.com/leppie))
    - ```[#3466](https://nuget.codeplex.com/workitem/3466)``` - When packing packages, verifying Id of dependency packages.
2. ```[Maarten Balliauw](https://www.codeplex.com/site/users/view/maartenba)``` ([@maartenballiauw](https://twitter.com/maartenballiauw))
    - ```[#2379](https://nuget.codeplex.com/workitem/2379)``` - Remove the $metadata suffix when persistening feed credentials.
3. ```[Filip De Vos](https://www.codeplex.com/site/users/view/FilipDeVos)``` ([@foxtricks](https://twitter.com/foxtricks))
    - ```[#3538](http://nuget.codeplex.com/workitem/3538)``` - Support specifying project file for the nuget.exe update command.
4. ```[Juan Gonzalez](https://www.codeplex.com/site/users/view/jjgonzalez)```
    - ```[#3536](http://nuget.codeplex.com/workitem/3536)``` - Replacement tokens not passed with -IncludeReferencedProjects.
5. ```[David Poole](https://www.codeplex.com/site/users/view/Sarkie)``` ([@Sarkie_Dave](https://twitter.com/Sarkie_Dave))
    - ```[#3677](http://nuget.codeplex.com/workitem/3677)``` - Fix nuget.push throwing OutOfMemoryException when pushing large package.
6. ```[Wouter Ouwens](https://www.codeplex.com/site/users/view/Despotes)```
    - ```[#3666](http://nuget.codeplex.com/workitem/3666)``` - Fix incorrect target path when project references another CLI/C++ project.
7. ```[Adam Ralph](http://www.codeplex.com/site/users/view/adamralph)``` ([@adamralph](https://twitter.com/adamralph))
    - ```[#3639](https://nuget.codeplex.com/workitem/3639)``` - Allow packages to be installed as development dependencies by default
8. ```[David Fowler](https://www.codeplex.com/site/users/view/dfowler)``` ([@davidfowl](https://twitter.com/davidfowl))
    - ```[#3717](https://nuget.codeplex.com/workitem/3717)``` - Remove implicit upgrades to the latest patch version
9. ```[Gregory Vandenbrouck](https://www.codeplex.com/site/users/view/vdbg)```
    - Several bug fixes and improvements for NuGet.Server, the nuget.exe mirror command, and others.
    - This work was done over several months, with Gregory working with us on the right timing to integrate into master for 2.8.

## Patch Resolution for Dependencies

When resolving package dependencies, NuGet has historically implemented a strategy of selecting the lowest major and minor package version which satisfies the dependencies on the package. Unlike the major and minor version, however, the patch version was always resolved to the highest version. Though the behavior was well-intentioned, it created a lack of determinism for installing packages with dependencies. Consider the following example:

```
PackageA@1.0.0 -[ >=1.0.0 ]-> PackageB@1.0.0

Developer1 installs PackageA@1.0.0: installed PackageA@1.0.0 and PackageB@1.0.0

PackageB@1.0.1 is published

Developer2 installs PackageA@1.0.0: installed PackageA@1.0.0 and PackageB@1.0.1
```

In this example, even though Developer1 and Developer2 installed PackageA@1.0.0, each ended up with a different version of PackageB. NuGet 2.8 changes this default behavior such that the dependency resolution behavior for patch versions is consistent with the behavior for major and minor versions. In the above example, then, PackageB@1.0.0 would be installed as a result of installing PackageA@1.0.0, regardless of the newer patch version.

## -DependencyVersion Switch

Though NuGet 2.8 changes the _default_ behavior for resolving dependencies, it also adds more precise control over dependency resolution process via the -DependencyVersion switch in the package manager console. The switch enables resolving dependencies to the lowest possible version (default behavior), the highest possible version, or the highest minor or patch version.  This switch only works for install-package in the powershell command.

![DependencyVersion Switch](./media/NuGet-2.8/dependencyversion.png)

## DependencyVersion Attribute

In addition to the -DependencyVersion switch detailed above, NuGet has also allowed for the ability to set a new attribute in the Nuget.Config file defining what the default value is, if the -DependencyVersion switch is not specified in an invocation of install-package. This value will also be respected by the NuGet Package Manager Dialog for any install package operations. To set this value, add the attribute below to your Nuget.Config file:

```xml
<config>
    <add key="dependencyversion" value="Highest" />
</config>
```

## Preview NuGet Operations With -whatif

Some NuGet packages can have deep dependency graphs, and as such, it can be helpful during an install, uninstall, or update operation to first see what will happen. NuGet 2.8 adds the standard PowerShell -whatif switch to the install-package, uninstall-package, and update-package commands to enable visualizing the entire closure of packages to which the command will be applied. For example, running `install-package Microsoft.AspNet.WebApi -whatif` in an empty ASP.NET Web application yields the following.

```
PM> install-package Microsoft.AspNet.WebApi -whatif
Attempting to resolve dependency 'Microsoft.AspNet.WebApi.WebHost (≥ 5.0.0)'.
Attempting to resolve dependency 'Microsoft.AspNet.WebApi.Core (≥ 5.0.0)'.
Attempting to resolve dependency 'Microsoft.AspNet.WebApi.Client (≥ 5.0.0)'.
Attempting to resolve dependency 'Newtonsoft.Json (≥ 4.5.11)'.
Install Newtonsoft.Json 4.5.11
Install Microsoft.AspNet.WebApi.Client 5.0.0
Install Microsoft.AspNet.WebApi.Core 5.0.0
Install Microsoft.AspNet.WebApi.WebHost 5.0.0
Install Microsoft.AspNet.WebApi 5.0.0
```

## Downgrade Package

It is not uncommon to install a prerelease version of a package in order to investigate new features and then decide to roll back to the last stable version. Prior to NuGet 2.8, this was a multi-step process of uninstalling the prerelease package and its dependencies, and then installing the earlier version. With NuGet 2.8, however, the update-package will now roll back the entire package closure (e.g. the package's dependency tree) to the previous version.

## Development Dependencies

Many different types of capabilities can be delivered as NuGet packages - including tools that are used for optimizing the development process. These components, while they can be instrumental in developing a new package, should not be considered a dependency of the new package when it's later published. NuGet 2.8 enables a package to identify itself in the `.nuspec` file as a developmentDependency. When installed, this metadata will also be added to the `packages.config` file of the project into which the package was installed. When that `packages.config` file is later analyzed for NuGet dependencies during `nuget.exe pack`, it will exclude those dependencies marked as development dependencies.

## Individual packages.config Files for Different Platforms

When developing applications for multiple target platforms, it's common to have different project files for each of the respective build environments. It is also common to consume different NuGet packages in different project files, as packages have varying levels of support for different platforms. NuGet 2.8 provides improved support for this scenario by creating different `packages.config` files for different platform-specific project files.

![Multiple package.config files](./media/NuGet-2.8/multiple-packageconfigs.png)

## Fallback to Local Cache

Though NuGet packages are typically consumed from a remote gallery such as [the NuGet gallery](https://www.nuget.org/) using a network connection, there are many scenarios where the client is not connected. Without a network connection, the NuGet client was not able to successfully install packages - even when those packages were already on the client's machine in the local NuGet cache. NuGet 2.8 adds automatic cache fallback to the package manager console. For example, when disconnecting the network adapter and installing jQuery, the console shows the following:

```
PM> Install-Package jquery
The source at nuget.org [https://www.nuget.org/api/v2/] is unreachable. Falling back to NuGet Local Cache at C:\Users\me\AppData\Local\NuGet\Cache
Installing 'jQuery 2.0.3'.
Successfully installed 'jQuery 2.0.3'.
Adding 'jQuery 2.0.3' to WebApplication18.
Successfully added 'jQuery 2.0.3' to WebApplication18.
```

The cache fallback feature does not require any specific command arguments. Additionally, cache fallback currently works only in the package manager console - the behavior does not currently work in the package manager dialog.

## WebMatrix NuGet Client Updates

Along with NuGet 2.8, the NuGet extension for WebMatrix was also updated to include many of the major features delivered with [NuGet 2.5](../release-notes/nuget-2.5.md). New capabilities include those such as 'Update All', 'Minimum NuGet Version', and allowing for overwriting of content files.

To update your NuGet Package Manager extension in WebMatrix 3:

1. Open WebMatrix 3
1. Click the Extensions icon in the ribbon
1. Select the Updates tab
1. Click to update NuGet Package Manager to 2.5.0
1. Close and restart WebMatrix 3

This is the NuGet team's first release of the NuGet Package Manager extension for WebMatrix.  The code was recently contributed by Microsoft into the open-source NuGet project. Previously, the NuGet integration was built into WebMatrix, and it could not be updated out of band from WebMatrix.  We now have the capability to further update it alongside the rest of NuGet's client tools.

## Bug Fixes

One of the major bug fixes made was performance improvement in the  update-package -reinstall    command.

In addition to these features and the aforementioned performance fix, this release of NuGet also includes many other bug fixes. There were 181 total issues addressed in the release. For a full list of the work items fixed in NuGet 2.8, please view the ```[NuGet Issue Tracker for this release](https://nuget.codeplex.com/workitem/list/advanced?release=NuGet%202.8&status=all)```.
