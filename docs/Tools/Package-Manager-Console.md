# Package Manager Console

The Package Manager Console in Visual Studio lets you use [NuGet PowerShell commands](/ndocs/tools/powershell-reference) to find, install, uninstall, and update NuGet packages. Using the Console is necessary if you want to work with packages without having a solution open, and is required in cases where the Package Manager UI does not provide a way to perform an operation. Note, however, that all operations can be done with the [NuGet CLI](/ndocs/tools/nuget.exe-cli-reference). 

In all cases, you open the Console in Visual Studio through the **Tools > NuGet Package Manager > Package Manager Console** command.

At the top of the pane you can select the desired package source, manage sources (by clicking the gear icon), and select the default project to which commands will be applied:

![Package Manager Console controls](/images/docs/PackageManagerConsoleControls.png) 

You can override these settings with most commands by using the `-Source` and `-ProjectName` options.

In this topic:

- [Finding a package](#finding-a-package)
- [Installing a package](#installing-a-package)
- [Uninstalling a package](#uninstalling-a-package)
- [Updating a package](#updating-a-package)
- [Extending the Package Manager Console](#extending-the-package-manager-console)  
- [Setting up a NuGet PowerShell profile](#setting-up-a-nuget-powershell-profile)


## Finding a package

In the console, [`Get-Package -ListAvailable`](/ndocs/tools/powershell-reference#get-package) see all the packages available from the selected source. For nuget.org, the list will contain thousands of packages, so it's helpful to use the `-Filter` switch along with `-PageSize`. In NuGet 3.0 and later, you can instead use the [`Find-Package`](/ndocs/tools/powershell-reference#find-package) command that is better suited to this operation.  

Examples:

	# All versions of NuGet	
	Get-Package -ListAvailable -Filter elmah
	Get-Package -ListAvailable -Filter Logging 

	# List all versions of packages matching the filter "jquery"
	Get-Package -ListAvailable -Filter jquery -AllVersions


	# NuGet 3.0+
	Find-Package elmah
	Find-Package Logging

	# List packages with the keyword EntityFramework and version 6.1.1
    Find-Package EntityFramework -version 6.1.1

	# List all versions of the package with the ID of "jquery"
	Find-Package jquery -AllVersions -ExactMatch


## Installing a package

Once you know the identifier of the package you want to install use [`Install-Package`](/ndocs/tools/powershell-reference#install-package), such as `Install-Package elmah`.

NuGet retrieves the package from the specified package source and installs it in the default project of the solution, unless you specify another project using the `-ProjectName` switch. 

Installing a package performs the following actions:

- Display applicable license terms in the Console window with implied agreement. If you do not agree to the terms, you should uninstall the package immediately.
- Creates a `packages` folder (if needed) and copies package files into a subfolder within it.
- Adds references to the project, which subsequently appear in Solution Explorer
- Updates `app.config` and/or `web.config` if the package uses [source and config file transformations](/ndocs/create-packages/source-and-config-file-transformations).
- Installs any dependencies if not already present in the project. This might update package versions in the process, as described in [Dependency Resolution](/ndocs/consume-packages/dependency-resolution).  


## Uninstalling a package 

If you do not already know the name of the package you want to remove, use the [`Get-Package`](/ndocs/tools/powershell-reference#get-package) command with no parameters to see all of the currently-installed packages.

To uninstall a package, use [`Uninstall-Package`](/ndocs/tools/powershell-reference#uninstall-package) with the package ID, such as `Update-Package jQuery`.

Uninstalling a package performs the following actions:

- References to the package no longer appear in the **Reference** or **Bin** folders in Solution Explorer. (You might need to rebuild the project to see it removed from the **Bin** folder.)
- The folder for the package is removed from the `packages` folder; the `packages` folder itself is deleted if no packages remain.
- Any changes made to `app.config` or `web.config` when the package was installed are removed.
- If other packages were installed because they were dependencies of the package that was removed, and if no remaining packages use those dependencies, the dependency packages are also removed.


## Updating a package 

The [`Get-Package -updates`](/ndocs/tools/powershell-reference#get-package) command checks if there are newer versions available for any installed packages.

To update a package, use [`Update-Package`](/ndocs/tools/powershell-reference#update-package) with the package ID, such as `Update-Package jQuery`.


## Extending the Package Manager Console

Some packages install new commands for the Console. For example, `MvcScaffolding`, creates commands, such as `Scaffold` shown below, to generate ASP.NET MVC controllers and views:

![Installing and using MvcScaffold](/images/consume/package-manager-console-install-mvcscaffold.png)


## Setting up a NuGet PowerShell Profile

A PowerShell profile lets you make commonly-used commands available wherever you use PoewrShell. NuGet supports a NuGet specific profile typically located at:

    %UserProfile%\Documents\WindowsPowerShell\NuGet_profile.ps1

To find the profile file, type `$profile` in the Console: 

    $profile
    C:\Users\<user>\Documents\WindowsPowerShell\NuGet_profile.ps1

For more details, refer to [Windows PowerShell Profiles](https://technet.microsoft.com/library/bb613488.aspx).
