---
title: NuGet 2.7 Release Notes
description: Release notes for NuGet 2.7 including known issues, bug fixes, added features, and DCRs.
author: JonDouglas
ms.author: jodou
ms.date: 11/11/2016
ms.topic: conceptual
---

# NuGet 2.7 Release Notes

[NuGet 2.6.1 for WebMatrix Release Notes](../release-notes/nuget-2.6.1-for-webmatrix.md) | [NuGet 2.7.1 Release Notes](../release-notes/nuget-2.7.1.md)

NuGet 2.7 was released on August 22, 2013.

## Acknowledgements

We would like to thank the following external contributors for their significant contributions to NuGet 2.7:

1. ```[Mike Roth](http://www.codeplex.com/site/users/view/mxrss)``` ([@mxrss](https://twitter.com/mxrss))
    - Show License url when listing packages and verbosity is detailed.
2. ```[Adam Ralph](http://www.codeplex.com/site/users/view/adamralph)``` ([@adamralph](https://twitter.com/adamralph))
    - ```[#1956](http://nuget.codeplex.com/workitem/1956)``` - Add developmentDependency attribute to `packages.config` and use it in pack command to only include runtime packages
3. ```[Rafael Nicoletti](http://www.codeplex.com/site/users/view/tkrafael)``` ([@tkrafael](https://twitter.com/tkrafael))
    - Avoid duplicate Properties key in nuget.exe pack command.
4. ```[Ben Phegan](http://www.codeplex.com/site/users/view/benphegan)``` ([@BenPhegan](https://twitter.com/benphegan))
    - ```[#2610](http://nuget.codeplex.com/workitem/2610)``` - Increase machine cache size to 200.
5. ```[Slava Trenogin](http://www.codeplex.com/site/users/view/derigel)``` ([@derigel](https://twitter.com/derigel))
    - ```[#3217](http://nuget.codeplex.com/workitem/3217)``` - Fix NuGet dialog showing updates in the wrong tab
    - Fix Project.TargetFramework can be null in ProjectManager
    - ```[#3248](http://nuget.codeplex.com/workitem/3248)``` - Fix SharedPackageRepository FindPackage/FindPackagesById will fail on non-existent packageId
6. ```[Kevin Boyle](http://www.codeplex.com/site/users/view/KevinBoyleRG)``` ([@kevfromireland](https://twitter.com/kevfromireland))
    - ```[#3234](http://nuget.codeplex.com/workitem/3234)``` - Enable support for Nomad project
7. ```[Corin Blaikie](http://www.codeplex.com/site/users/view/corinblaikie)``` ([@corinblaikie](https://twitter.com/corinblaikie))
    - ```[#3252](http://nuget.codeplex.com/workitem/3252)``` - Fix push command fails with exit code 0 when file doesn't exist.
8. ```[Martin Veselý](http://www.codeplex.com/site/users/view/veselkamartin)```
    - ```[#3226](http://nuget.codeplex.com/workitem/3226)``` - Fix bug with Add-BindingRedirect command when a project references a database project.
9. ```[Miroslav Bajtos](http://www.codeplex.com/site/users/view/miroslavbajtos)``` ([@bajtos](https://twitter.com/bajtos))
    - ```[#2891](http://nuget.codeplex.com/workitem/2891)``` - Fix bug of nuget.pack parsing wildcard in the 'exclude' attribute incorrectly.
10. ```[Justin Dearing](http://www.codeplex.com/site/users/view/zippy1981)``` ([@zippy1981](https://twitter.com/zippy1981))
     - ```[#3307](http://nuget.codeplex.com/workitem/3307)``` - Fix bug `NuGet.targets` does not pass $(Platform) to nuget.exe when restoring packages.
11. ```[Brian Federici](http://www.codeplex.com/site/users/view/benerdin)```
     - ```[#3294](http://nuget.codeplex.com/workitem/3294)``` - Fix bug in nuget.exe package command which would allow adding files with the same name but different casing, eventually causing "Item already exists" exception.
12. ```[Daniel Cazzulino](http://www.codeplex.com/site/users/view/dcazzulino)``` ([@kzu](https://twitter.com/kzu))
     - ```[#2990](http://nuget.codeplex.com/workitem/2990)``` - Add Version property to NetPortableProfile class.
13. ```[David Simner](https://www.codeplex.com/site/users/view/DavidSimner)```
     - ```[#3460](https://nuget.codeplex.com/workitem/3460)``` - Fix bug NullReferenceException if requireApiKey = true, but the header X-NUGET-APIKEY isn't present
14. ```[Michael Friis](https://www.codeplex.com/site/users/view/friism)``` ([@friism](https://twitter.com/friism))
     - ```[#3278](https://nuget.codeplex.com/workitem/3278)``` - Fixes NuGet.Build targets file to so that it works correctly on MonoDevelop
15. ```[Pranav Krishnamoorthy](https://www.codeplex.com/site/users/view/pranavkm)``` ([@pranav_km](https://twitter.com/pranav_km))
     - Improve Restore command performance by increasing parallelization

## Notable features in the release

### Package Restore by Default (with implicit consent)

NuGet 2.7 introduces a new approach to package restore, and also overcomes a major hurdle: Package restore consent is now on by default! The combination of the new approach and the implicit consent will drastically simplify package restore scenarios.

#### Implicit Consent

With NuGet versions 2.0, 2.1, 2.2, 2.5, and 2.6, users needed to explicitly allow NuGet to download missing packages during build. If this consent had not been explicitly given, then solutions that had enabled package restore would fail to build until the user had granted consent.

Starting with NuGet 2.7, package restore consent is ON by default while allowing users to explicitly *opt out* of package restore if desired, using the checkbox in NuGet's settings in Visual Studio. This change for implicit consent affects NuGet in the following environments:

* Visual Studio 2013 Preview
* Visual Studio 2012
* Visual Studio 2010
* nuget.exe Command-Line Utility

#### Automatic Package Restore in Visual Studio

Starting with NuGet 2.7, NuGet will automatically download missing packages during build in Visual Studio, even if package restore hasn't been explicitly enabled for the solution. This Automatic Package Restore happens in Visual Studio when you build a project or the solution, but before MSBuild is invoked. This yields a few significant benefits:

1. No further need to use the "Enable NuGet Package Restore" gesture on your solution
1. Projects don't need to be modified, and NuGet won't make changes to your project to ensure package restore is enabled
1. All NuGet packages, including those that included MSBuild imports for props/targets files, will be restored *before* MSBuild is invoked, ensuring those props/targets are properly recognized during the build

In order to use Automatic Package Restore in Visual Studio, you only need to take one (in)action:

1. Don't check in your `packages` folder

There are several ways to omit your `packages` folder from source control. For more information, see the [Packages and Source Control](../consume-packages/packages-and-source-control.md) topic.

While all users are implicitly opted into Automatic Package Restore consent, you can easily opt out through the Package Manager settings in Visual Studio.

![Package Manager Settings](./media/NuGet-2.7/package-manager-settings.png)

#### Simplified Package Restore from the Command-Line

NuGet 2.7 introduces a new feature for nuget.exe: `nuget.exe restore`

This new Restore command allows you to easily restore all packages for a solution with a single command, by accepting a solution file or folder as an argument. Furthermore, that argument is implied when there's only a single solution in the current folder. That means the following all work from a folder that contains a single solution file (MySolution.sln):

1. nuget.exe restore MySolution.sln
1. nuget.exe restore .
1. nuget.exe restore

The Restore command will open the solution file and find all projects within the solution. From there, it will find the `packages.config` files for each of the projects and restore all of the packages found. It also restores solution-level packages found in the `.nuget\packages.config` file. More information about the new Restore command can be found in the [Command-Line Reference](../reference/cli-reference/cli-ref-restore.md).

#### The New Package Restore Workflow

We are excited about these changes to Package Restore, as it introduces a new workflow. If you want to omit your packages from source control you simply don't commit the `packages` folder. Visual Studio users who open and build the solution will see the packages automatically restored. For command-line builds, simply invoke `nuget.exe restore` before invoking `msbuild`. You no longer need to remember to use the "Enable NuGet Package Restore" gesture on your solution, and we'll no longer need to modify your projects to alter the build. And this also yields a much improved experience for packages that include MSBuild imports, especially for imports added through NuGet's recent feature for [automatically importing props/targets files](../release-notes/nuget-2.5.md#automatic-import-of-msbuild-targets-and-props-files) from the \build folder.

In addition to the work we've done ourselves, we're also working with some important partners to round this new approach out. We don't have concrete timelines for any of these yet, but each partner is as excited as we are about the new approach.

* Team Foundation Service - They are working to integrate the call to `nuget.exe restore` into the default build scenarios.
* Windows Azure Web Sites - They are working to allow you to push your project to Azure and have `nuget.exe restore` called before your web site is built.
* TeamCity - They are updating their NuGet Installer plugin for TeamCity 8.x
* AppHarbor - They are working to allow you to push your repo to AppHarbor and have `nuget.exe restore` called before your solution is build.

With each of the partners above, they would use their own copy of nuget.exe and you would not need to carry nuget.exe in your solution.

#### Known Issues

There were two known issues with nuget.exe restore with the initial 2.7 release, but they were fixed on 9/6/2013 with an update to the ```[NuGet.CommandLine package](http://www.nuget.org/packages/NuGet.CommandLine/)```.  This update is also available on the [NuGet 2.7 download page](https://nuget.codeplex.com/releases/view/107605) on CodePlex.  Running `nuget.exe update -self` will update to the latest release.

The fixed were:

1. ```[New package restore doesn't work on Mono when using SLN file](https://nuget.codeplex.com/workitem/3596)```
1. ```[New package restore doesn't work with Wix projects](https://nuget.codeplex.com/workitem/3598)```

There is also a known issue with the new package restore workflow whereby ```[Automatic Package Restore does not work for projects under a solution folder](https://nuget.codeplex.com/workitem/3625)```. This issue was fixed in NuGet 2.7.1.

### Project Retargeting and Upgrade Build Errors/Warnings

Many times after retargeting or upgrading your project, you find that some NuGet packages aren't functioning properly. Unfortunately, there is no indication of this and then there's no guidance on how to address it. With NuGet 2.7, we now use some Visual Studio events to recognize when you've retargeted or upgraded your project in a way that affects your installed NuGet packages.

If we detect that any of your packages were affected by the retargeting or upgrade, we'll produce immediate build errors to let you know. In addition to the immediate build error, we also persist a `requireReinstallation="true"` flag in your `packages.config` file for all packages that were affected by the retargeting, and each subsequent build in Visual Studio will raise build warnings for those packages.

Although NuGet cannot take automatic action to reinstall affected packages, we hope this indication and warning will guide help you discover when you need to reinstall packages. We are also working on [package reinstallation guidance documentation](../consume-packages/reinstalling-and-updating-packages.md) that these error messages direct you to.

### NuGet Configuration Defaults

Many companies are using NuGet internally, but have had a hard time guiding their developers to use internal package sources instead of nuget.org. NuGet 2.7 introduces a Configuration Defaults feature that allows machine-wide defaults to be specified for:

1. Enabled package sources
1. Registered, but disabled package sources
1. The default nuget.exe push source

Each of these can now be configured within a file located at `%ProgramData%\NuGet\NuGetDefaults.Config`. If this config file specifies package sources, then the default nuget.org package source will not be registered automatically, and the ones in `NuGetDefaults.Config` will be registered instead.

While not required to use this feature, we expect companies to deploy `NuGetDefaults.Config` files using Group Policy.

*Note that this feature will never cause a package source to be removed from a developer's NuGet settings. That means if the developer has already used NuGet and therefore has the nuget.org package source registered, it won't be removed after the creation of a `NuGetDefaults.Config` file.*

See [NuGet Configuration Defaults](../consume-packages/configuring-nuget-behavior.md#nuget-defaults-file) for more information about this feature.

### Renaming the Default Package Source

NuGet has always registered a default package source called "NuGet official package source" that points to nuget.org. That name was verbose and it also didn't specify where it was actually pointing. To address those two issues, we've renamed this package source to simply "nuget.org" in the UI. The URL for the package source was also changed to include the "www." prefix. After using NuGet 2.7, your existing "NuGet official package source" will automatically be updated to "nuget.org" as its name and "<https://www.nuget.org/api/v2/>" as its URL.

### Performance Improvements

We made some performance improvement in 2.7 which will yield smaller memory footprint, less disk usage and faster package installation. We also made smarter queries to OData-based feeds which will reduce the overall payload.

### New Extensibility APIs

We added some new APIs to our extensibility services to fill the gap of missing functionalities in previous releases.

#### IVsPackageInstallerServices

```cs
// Checks if a NuGet package with the specified Id and version is installed in the specified project.
bool IsPackageInstalledEx(Project project, string id, string versionString);

// Get the list of NuGet packages installed in the specified project.
IEnumerable<IVsPackageMetadata> GetInstalledPackages(Project project);
```

#### IVsPackageInstaller

```cs
// Installs one or more packages that exist on disk in a folder defined in the registry.
void InstallPackagesFromRegistryRepository(string keyName, bool isPreUnzipped, bool skipAssemblyReferences, Project project, IDictionary<string, string> packageVersions);

// Installs one or more packages that are embedded in a Visual Studio Extension Package.
void InstallPackagesFromVSExtensionRepository(string extensionId, bool isPreUnzipped, bool skipAssemblyReferences, Project project, IDictionary<string, string> packageVersions);
```

### Development-Only Dependencies

This feature was contributed by [Adam Ralph](https://twitter.com/adamralph) and it allows package authors to declare dependencies that were only used at development time and don't require package dependencies. By adding a `developmentDependency="true"` attribute to a package in `packages.config`, `nuget.exe pack` will no longer include that package as a dependency.

### Removed Support for Visual Studio 2010 Express for Windows Phone

The new package restore model in 2.7 is implemented by a new VSPackage which is different from the main NuGet VSPackage. Due to a technical issue, this new VSPackage doesn't work correctly in the *Visual Studio 2010 Express for Windows Phone* SKU as we share the same code base with other supported Visual Studio SKUs. Therefore, starting with NuGet 2.7, we are dropping support for *Visual Studio 2010 Express for Windows Phone* from the published extension. Support for *Visual Studio 2010 Express for Web* is still included in the primary extension published to the Visual Studio Extension Gallery.

Since we are unsure how many developers are still using NuGet in that version/edition of Visual Studio, we are publishing a separate Visual Studio extension specifically for those users and publishing it on CodePlex (rather than the Visual Studio Extension Gallery). We don't plan to continue to maintain that extension, but if this affects you please let us know by filing an issue on CodePlex.

To download the NuGet Package Manager (for Visual Studio 2010 Express for Windows Phone), visit the ```[NuGet 2.7 Downloads](https://nuget.codeplex.com/releases/view/107605)``` page.

### Bug Fixes

In addition to these features, this release of NuGet also includes many other bug fixes. There were 97 total issues addressed in the release. For a full list of work items fixed in NuGet 2.7, please view the ```[NuGet Issue Tracker for this release](https://nuget.codeplex.com/workitem/list/advanced?release=NuGet%202.7&status=all)```.
