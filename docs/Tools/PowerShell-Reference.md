--- 
# required metadata 
 
title: PowerShell Reference | Microsoft Docs
author: kraigb 
ms.author: kraigb 
manager: ghogen 
ms.date: 11/11/2016 
ms.topic: article 
ms.prod: nuget 
#ms.service: 
ms.technology: nuget 
ms.assetid: cd08b869-44c6-480e-90f7-494a6d08e6d0
 
# optional metadata 
 
#description: 
#keywords: 
#ROBOTS: 
#audience: 
#ms.devlang: 
ms.reviewer:  
- karann 
- harikm 
#ms.suite:  
#ms.tgt_pltfrm: 
#ms.custom: 
 
--- 

# PowerShell Reference

The Package Manager Console provides a PowerShell interface within Visual Studio to interact with NuGet. This topic is the reference for those commands; for a guide to using the console, see the [Package Manager Console](../tools/package-manager-console.md) topic.  

Command | Description | NuGet Version
--- | --- | ---
[Add-BindingRedirect](#add-bindingredirect) | Examines all assemblies within the output path for a project and adds binding redirects to the `app.config` or `web.config` where necessary. | All
[Find-Package](#find-package) | Searches a package source using a package ID or keywords. | 3.0+
[Get-Package](#get-package) | Retrieves the list of packages installed in the local repository, or lists packages available from a package source. | All
[Get-Project](#get-project) | Displays information about the default or specified project. | 3.0+
[Install-Package](#install-package) | Installs a package and its dependencies into the project. | All
[Open-PackagePage](#open-packagepage) | Launches the default browser with the project, license, or report abuse URL for the specified package. | Deprecated in 3.0+
[Sync-Package](#sync-package) | Get the version of installed package from specified project and syncs the version to the rest of projects in the solution. | 3.0+
[Uninstall-Package](#uninstall-package) | Removes a package from a project, optionally removing its dependencies. | All
[Update-Package](#update-package) | Updates a package and its dependencies, or all packages in a project. | All

For detailed help on any of these commands within the console, just run the following with the command name in question:

    Get-Help <command>


Note that all Package Manager Console commands support the following common PowerShell parameters: 

- Debug
- ErrorAction
- ErrorVariable
- OutBuffer
- OutVariable
- PiplineVariable
- Verbose
- WarningAction
- WarningVariable

For details, refer to [about_CommonParameters](http://go.microsoft.com/fwlink/?LinkID=113216) in the PowerShell documentation.


## Add-BindingRedirect

Examines all assemblies within the output path for a project and adds binding redirects to the appropriate configuration files. For additional details on what this means, see [Redirecting Assembly Versions](https://msdn.microsoft.com/library/7wd6ex19.aspx) on MSDN.

> [!NOTE]
> NuGet 1.2+ automatically runs this command when installing a package.

### Usage

    Add-BindingRedirect [-ProjectName <string>]


### Parameters

|     |     |
| --- | --- |
ProjectName | Specifies the project to which to add binding redirects, defaulting to the default project. The -ProjectName switch itself is optional.

### Examples

    Add-BindingRedirect MyProjectName


## Find-Package

*Version 3.0+*

Searches a package source using a package ID or keywords. 

### Usage

        Find-Package [<keywords>] [-Source <string>] [-First <int>] [-Skip <int>] [-AllVersions] [-IncludePrerelease] [-ExactMatch]

### Parameters

|     |     |
| --- | --- |
&lt;keywords&gt; | Searches the package source for packages with the keywords. Use the `-ExactMatch` switch to return only those packages whose package ID matches the keywords. If no keywords are given, Find-Package returns a list of the top 20 packages by downloads.
Source | Specifies the URL or path for the package source to search. If omitted, searched the currently selected package source.
First | Specifies the number of packages to return from the beginning of the list. If not specified, Find-Package lists 20 packages.
Skip | Skips the specified number of packages, counting from the beginning of the list
AllVersions | Displays all available versions of each package instead of only the latest version.
IncludePrerelease | Indicates whether to include prerelease packages in the list.
ExactMatch | Specified to use the keywords as a case-sensitive package ID.

### Examples

    # List packages with the keyword Elmah available
    Find-Package Elmah

    # List all versions of the jquery package
    Find-Package jquery -AllVersions -ExactMatch

    # List packages with the keyword EntityFramework and version 6.1.1
    Find-Package EntityFramework -version 6.1.1


## Get-Package 

Retrieves the list of packages installed in the local repository, or lists packages available from a package source when used with the `-ListAvailable` switch.

### Usage

    Get-Package [-Source <string>] [-ListAvailable] [-Updates] [-ProjectName <string>] [-Filter <string>] [-First <int>] [-Skip <int>] [-AllVersions] [-IncludePrerelease] [-PageSize <int>]

With no parameters, `Get-Package` displays the list of packages installed in the default project. 

### Parameters

|     |     |
| --- | --- |
Source | Specifies the URL or path of a package source. When used with -ListAvailable, defaults to nuget.org.
ListAvailable | Lists packages available from a package source, defaulting to nuget.org. If -PageSize and/or -First are not specified, this will show 50 packages.
Updates | Lists packages that have an update available from the package source.
ProjectName | Specifies the project from which to get installed packages.
Filter | Specifies a filter string used to narrow down the list of packages returned. The filter is searched for in the package ID, the description and tags.
IncludePrerelease | Indicates that prerelease packages should be included in the list.
First | Specifies the number of packages to return from the beginning of the list. If not specified, defaults to 50.
Skip | Skips the specified number of packages, counting from the beginning of the list.
AllVersions | Displays all available versions of each package instead of only the latest version.
PageSize | *(3.0+)* When used with -ListAvailable, specifies the number of packages to list before giving a prompt to continue. The command will continue listing packages as long as there are more to return. Without -PageSize, at most 50 packages are listed by default.

### Examples

    # Lists the packages installed in the current solution
    Get-Package

    # Lists packages available in the current package source
    Get-Package -ListAvailable

    # Lists all packages in the current source in pages of 20
    Get-Package -ListAvailable -PageSize 20 

    # Lists packages with the Ninject keyword in the current source, up to 50
    Get-Package -ListAvailable -Filter Ninject

    # Lists packages installed in the solution that have available updates
    Get-Package -Updates


## Get-Project

Displays information about the default or specified project.

### Usage

    Get-Project [-Name <string>] [-All]

### Parameters

|     |     |
| --- | --- |
Name | Specifies the project to display, defaulting to the default project. The -Name switch is itself optional.
All | Displays information for every project in the solution.

### Examples

    # Displays information for the default project
    Get-Project

    # Displays information for MyProjectName
    Get-Project MyProjectName

     # Displays information for all projects in the solution
    Get-Project -All


## Install-Package

Installs a package and its dependencies into the project.

### Usage

    Install-Package [-Id] <string> [-Version <string>] [-IgnoreDependencies] [-ProjectName <string>]  [-Source <string>] [-IncludePrerelease] [-FileConflictAction] [-DependencyVersion <dependencyVersion>] [-WhatIf]

In NuGet 2.8+, `Install-Package` can downgrade an existing package in your project. For example, if you have Microsoft.AspNet.MVC 5.1.0-rc1 installed, the following command would downgrade it to 5.0.0: 

    Install-Package Microsoft.AspNet.MVC -Version 5.0.0.
 
NuGet 2.7 and earlier will give an error saying that a newer version is already installed.
    
### Parameters

|     |     |
| --- | --- |
Id | Specifies the package ID of the package to install. With NuGet 3.0+, the ID can be a path or URL of a packages.config file or a .nupkg file. The -Id switch itself is optional.
Version | Specifies the version of the package to install, defaulting to the latest version.
IgnoreDependencies | Installs only this package and not its dependencies.
ProjectName | Specifies the project into which to install the package, defaulting to the default project.
Source | Specifies the URL or path to a package source. If omitted, defaults to the currently selected package source.
IncludePrerelease | Indicates that prerelease packages can be installed.
FileConflictAction | Specifies the action to take when asked to overwrite or ignore existing files referenced by the project. Possible values are *Overwrite, Ignore, None, OverwriteAll*, and *IgnoreAll* (3.0+).
DependencyVersion | Specifies the version of the dependency packages to use, which can be one of the following:
| | - Lowest (default): the lowest version
| | - HighestPatch: the version with the lowest major, lowest minor, highest patch
| | - HighestMinor: the version with the lowest major, highest minor, highest patch
| | - Highest: the highest version
| | You can set the default value using the [*dependencyVersion*](../schema/nuget.config-file.md#config-section) setting in the nuget.config file.
WhatIf | Shows what would happen when running the command without actually performing the install.

### Examples

    Install-package https://raw.githubusercontent.com/json-ld.net/master/src/JsonLD/packages.config

    Install-package c:\temp\packages.config

    Install-package https://az320820.vo.msecnd.net/packages/microsoft.aspnet.mvc.5.2.3.nupkg

    Install-package c:\temp\packages\jQuery.1.10.2.nupkg

    # Installs the latest version of Elmah from the current source
    Install-Package Elmah

    # Installs Glimpse 1.0.0 into the MvcApplication1 project 
    Install-Package Glimpse -Version 1.0.0 -Project MvcApplication1

    # Installs Ninject.Mvc3 but not its dependencies from c:\temp\packages
    Install-Package Ninject.Mvc3 -IgnoreDependencies -Source c:\temp\packages


## Open-PackagePage

*Deprecated in 3.0+*

Launches the default browser with the project, license, or report abuse URL for the specified package.

### Usage

    Open-PackagePage [-Id] <string> [-Version <string>] [-Source <string>] [-License <string>] [-ReportAbuse] [-PassThru]

### Parameters

|     |     |
| --- | --- |
Id | Specifies the package ID of the desired package. The -Id switch itself is optional.
Version | Specifies the version of the package, defaulting to the latest version.
Source | Specifies the package source, defaulting to the default source.
License | Opens the browser to the package's License URL. If neither -License nor -ReportAbuse is specified, the browser opens the package's Project URL.
ReportAbuse | Opens the browser to the package's Report Abuse URL. If neither -License nor -ReportAbuse is specified, the browser opens the package's Project URL.
PassThru | Displays the selected URL but does not open it in the browser.

### Examples

    # Opens a browser with the Ninject's package's project page
    Open-PackagePage Ninject

    # Opens a browser with the Ninject's package's license page
    Open-PackagePage Ninject -License

    # Opens a browser with the Ninject's package's report abuse page    
    Open-PackagePage Ninject -ReportAbuse

    # Assigns the license URL to the variable, $url, without launching the browser
    $url = Open-PackagePage Ninject -License -WhatIf -PassThru


## Sync-Package

*Version 3.0+*

Get the version of installed package from specified project and syncs the version to the rest of projects in the solution.

### Usage

    Sync-Package [-Id] <string> [-Version <string>] [-IgnoreDependencies] [-ProjectName <string>] [-Source <string>] [-IncludePrerelease] [-FileConflictAction] [-DependencyVersion <dependencyVersion>] [-WhatIf]

### Parameters

|     |     |
| --- | --- |
Id | Specifies the identifier of the package to sync. The -Id switch itself is optional.
Version | Specifies the version of the package to sync, defaulting to the currently installed version.
ProjectName | Specifies the project to sync the package from, defaulting to the default  project.
Source | Specifies the URL or path of a package source, defaulting to the current package source.
IncludePrerelease | Includes prerelease packages in the sync.
FileConflictAction | Specifies the action to take when asked to overwrite or ignore existing files referenced by the project. Possible values are *Overwrite, Ignore, None, OverwriteAll*, and *IgnoreAll* (3.0+).
DependencyVersion | Specifies the version of the dependency packages to use, which can be one of the following:
| | -Lowest (default): the lowest version
| | -HighestPatch: the version with the lowest major, lowest minor, highest patch
| | -HighestMinor: the version with the lowest major, highest minor, highest patch
| | -Highest: the highest version
| | You can set the default value using the [*dependencyVersion*](../schema/nuget.config-file.md#config-section) setting in the nuget.config file.
WhatIf | Shows what would happen when running the command without actually performing the sync.

### Examples

    # Syncs the Ninject package installed in the default project into the other projects in the solution
    Sync-Package Ninject

    # Syncs only Microsoft.Aspnet.package to the rest of the projects, but not its dependencies 
    Sync-Package Microsoft.Aspnet.Mvc -IgnoreDependencies

    # Syncs jQuery.Validation and installs the highest version of jQuery (a dependency) from the package source      
    Sync-Package jQuery.Validation -DependencyVersion highest


## Uninstall-Package

Removes a package from a project, optionally removing its dependencies.

### Usage

    Uninstall-Package [-Id] <string> [-Version <string>] [-RemoveDependencies] [-ProjectName <string>] [-Force] [-WhatIf]

If other packages depend on this package, the command will fail unless the –Force option is specified.


### Parameters

|     |     |
| --- | --- |
Id | Specifies the identifier of the package to uninstall. The -Id switch itself is optional.
Version | Specifies the version of the package to uninstall, defaulting to the currently installed version.
RemoveDependencies | Uninstalls the package and its unused dependencies. That is, if any dependency has another package that depends on it, it is skipped.
ProjectName | Specifies the project from which to uninstall the package, defaulting to the default project.
Force | Forces a package to be uninstalled, even if there are dependencies on it.
WhatIf | Shows what would happen when running the command without actually performing the uninstall.

### Examples

    # Uninstalls the Elmah package from the default project
    Uninstall-Package Elmah

    # Uninstalls the Elmah package and all its unused dependencies
    Uninstall-Package Elmah -RemoveDependencies   

    # Uninstalls the Elmah package even if another package depends on it.
    Uninstall-Package Elmah -Force


## Update-Package

Updates a package and its dependencies, or all packages in a project.

### Usage

    Update-Package [-Id <string>] [-Source <string>] [-IgnoreDependencies] [-ProjectName <string>] [-Version <string>] [-Safe] [-IncludePrerelease] [-Reinstall] [-FileConflictAction] [-DependencyVersion] [-WhatIf]

In NuGet 2.8+, `Update-Package` can be used to downgrade an existing package in your project. For example, if you have Microsoft.AspNet.MVC 5.1.0-rc1 installed, the following command would downgrade it to 5.0.0: 

    Update-Package Microsoft.AspNet.MVC -Version 5.0.0.

NuGet 2.7 and earlier will give an error saying that a newer version is already installed.

### Parameters

|     |     |
| --- | --- |  
Id | Specifies the package ID of the package to update. If omitted, updates all packages. The -Id switch itself is optional.
Source | Specifies the URL or path for a package source.
IgnoreDependencies | Skips updating any of the package's dependencies.
ProjectName | Specifies the project containing the packages to update, defaulting to all projects.
Safe | Constrains upgrades to only versions with the same Major and Minor version as the currently installed package.
ToHighestMinor | Constrains upgrades to only versions with the same Major version as the currently installed package
Version | Specifies the version to use for the upgrade, defaulting to the latest version. In NuGet 3.0+, the version value must be one of *Lowest, Highest, HighestMinor*, or *HighestPatch* (equivalent to -Safe).
IncludePrerelease | Indicates that prerelease packages can be used for the update.
Reinstall | Resintalls packages using their currently installed versions. See [Reinstalling and updating packages](../consume-packages/reinstalling-and-updating-packages.md).
FileConflictAction | Specifies the action to take when asked to overwrite or ignore existing files referenced by the project. Possible values are *Overwrite, Ignore, None, OverwriteAll*, and *IgnoreAll* (3.0+).
DependencyVersion | Specifies the version of the dependency packages to use, which can be one of the following:
| | -Lowest (default): the lowest version
| | -HighestPatch: the version with the lowest major, lowest minor, highest patch
| | -HighestMinor: the version with the lowest major, highest minor, highest patch
| | -Highest (default for Update-Package with no parameters): the highest version
| | You can set the default value using the [*dependencyVersion*](../schema/nuget.config-file.md#config-section) setting in the nuget.config file.
WhatIf | Shows what would happen when running the command without actually performing the update.

### Examples

    # Updates all packages in every project.
    Update-Package

    # Updates every package in the MvcApplication1 project
    Update-Package -Project MvcApplication1

    # Updates the Elmah package in every project to the latest version
    Update-Package Elmah

    # Updates the Elmah package to version 1.1.0 in every project.
    Update-Package Elmah -Version 1.1.0

    # Updates the Elmah package within the MvcApplication1 project only
    Update-Package Elmah -Project MvcApplication1

    # Updates Elmah to the highest "safe" version. For example, if Elmah version 1.0.0 of a package is installed, and versions 1.0.1, 1.0.2, and 1.1 are available in the feed, the -Safe parameter updates the package to 1.0.2 instead of 1.1 as it would otherwise.
    Update-Package Elmah -Safe