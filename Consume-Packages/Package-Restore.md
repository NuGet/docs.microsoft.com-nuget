# NuGet Package Restore

> **Important**: Nuget Automatic Package Restore has changed in Nuget 2.7+. Do not mix 'old' and new methods for automatic package restoration. For more information, see Common Issues with Automatic Package Restore, below.

Many developers like to omit binaries from their source control repository. This can be beneficial in multiple ways:

1. Distributed version control systems (DVCS) include every version of every file within the repository, and binary files that are updated frequently can lead to significant repository bloat and more time required to clone the repository.
1. With the packages included in the repository, team members may add references directly to package contents on disk rather than referencing packages through NuGet.
1. It becomes harder to "clean" your solution of any unused package folders, as you need to ensure you don't delete any package folders still in use.

To promote a cleaner developer environment while also reducing repository size, NuGet offers a **Package Restore** feature that will install all referenced packages before a project is built, thereby ensuring that all dependencies are available to a project without requiring them to be stored in source control. NuGet Package Restore is an extremely popular feature of NuGet and therefore it's important to understand how it works.

## Omitting Packages from Source Control

Even though package restore *[consent](#package-restore-consent)* is on by default, users still need to choose to omit their packages from source control before package restore is engaged. By default, source control systems will include the `packages` folder in your repository, and you need to take action to omit the packages from source control.

### Git
Use the [.gitignore file](https://www.kernel.org/pub/software/scm/git/docs/gitignore.html) to ignore the `packages` folder. [Sample `.gitignore` for Visual Studio projects](https://github.com/github/gitignore/blob/master/VisualStudio.gitignore).

The important parts of the file are the following:


	# Ignore NuGet Packages
	*.nupkg
	# Ignore the packages folder
	**/packages/*
	# except build/, which is used as an MSBuild target.
	!**/packages/build/
	# Uncomment if necessary however generally it will be regenerated when needed
	#!**/packages/repositories.config


### TFVC

**Preferably, you do this BEFORE adding your solution to source control to avoid having to delete the packages folder from the source control repository first.**

If you already added your solution to source control, you'll first have to delete the `\packages` folder from the repository, and check-in these pending changes before continuing to modify your already installed packages.

To **disable source control integration**, use the `.nuget\NuGet.config` file, as explained on the [NuGet Config Settings](NuGet-Config-Settings) document under the "Source control integration" section. This `nuget.config` file does not have to be added to your solution file, but it must be put in the `$(SolutionDir)\.nuget` folder to be taken into account as it works at the solution level. Also, you need to add this file to source control.
Tip: on Windows, you can create the `.nuget` file in Windows Explorer by creating a folder with the name `.nuget.` (note the trailing dot!).

At the minimum, the `.nuget\NuGet.config` file should contain the following:


	<?xml version="1.0" encoding="utf-8"?>
	<configuration>
		<solution>
			<add key="disableSourceControlIntegration" value="true" />
		</solution>
	</configuration>


This configuration setting allows NuGet to completely skip the call into Visual Studio to pend changes to the `packages` folder.

If you are on a version of TFS older than TFS 2012, you'll also need to cloak the `packages` folder in your workspace mappings.

If you are on TFS 2012 or newer, or using VSO, you'll need to add a [`.tfignore`](https://msdn.microsoft.com/en-us/library/ms245454.aspx#tfignore) file to **explicitly ignore modifications to the `\packages` folder** on the repository level.
To create a `.tfignore` file using Windows Explorer, create a new file and give it the name `.tfignore.` (no extension). You might need to disable the "Hide known file extensions" option first.

The `.tfignore` file should have the following entry:


	## Ignore the NuGet packages folder in the root of the repository
	packages

	#include package target files which may be required for msbuild
	!packages/*.targets


Depending on the location of the .tfignore file relative to the packages folder, you might need to adjust the entry to reflect this (e.g. if you put sources in a `\src` subfolder and have the `.tfignore` file in the repository root, the entry should be `\src\packages`).

## Package Restore Approaches

NuGet offers three approaches to using package restore. Automatic Package Restore is the NuGet team's recommended approach to Package Restore within Visual Studio, and it was introduced in NuGet 2.7. Command-Line Package Restore is required when building a solution from the command-line; it was introduced in early versions of NuGet, but was improved in NuGet 2.7. The MSBuild-integrated package restore approach is the original Package Restore implementation and though it continues to work in many scenarios, it does not cover the full set of scenarios addressed by the other two approaches.

### Automatic Package Restore in Visual Studio

Beginning with NuGet 2.7, the NuGet Visual Studio extension integrates into Visual Studio's build events and restores missing packages when a build begins. This feature is enabled by default, but developers can [opt out](#opting-out) if desired.

Here's how it works:

1. On project or solution build, Visual Studio raises an event that a build is beginning within the solution.
1. NuGet responds to this event and checks for `packages.config` files included in the solution.
1. For each `packages.config` file found, its packages are enumerated and checked for existence in the solution's `packages` folder.
1. Any missing packages are downloaded from the user's configured (and enabled) package sources, respecting the order of the package sources.
1. As packages are downloaded, they are unzipped into the solution's `packages` folder.

Before any of the above steps are taken however, NuGet verifies that consent is given on two levels:

1. Visual Studio is configured to 'Allow NuGet to download missing packages'
1. Visual Studio is configured to 'Automatically check for missing packages during build in Visual Studio'

These two options are available on the **Package Manager** General settings in Visual Studios options.

Additionally, if NuGet recognizes that the MSBuild-Integrated package restore approach is enabled for the solution, Automatic Package Restore is skipped. MSBuild-Integrated package restore is identified by checking the existence of the `.nuget\NuGet.targets` file at the solution root.

This approach to package restore offers several advantages:

1. No need to enable it for your project or solution. Visual Studio will automatically download missing packages before your projects are built and team members don't need to understand NuGet Package Restore.
1. No additional directories and files are required within your project or solution.
1. Packages are restored before MSBuild is invoked by Visual Studio. This allows packages that extend MSBuild though targets/props file imports to be restored before MSBuild starts, ensuring a successful build.
1. Compatibility with ASP.NET Web Site projects created in Visual Studio.

### Command-Line Package Restore

As a complement to Automatic Package Restore, NuGet offers a simple command-line approach to restoring packages before invoking MSBuild, ensuring that all referenced NuGet packages are available before the build starts. NuGet 2.7 introduced a new [Restore Command](Command-Line-Reference#Restore-command) that provides a single command that restores all packages for an entire solution. Prior to NuGet 2.7, the [Install Command](Command-Line-Reference#Install-command) was used to restore packages, but only for a single `packages.config` file.

Usage of `NuGet.exe`'s [Restore Command](Command-Line-Reference#Restore-command) is very straightforward. Given a folder `D:\projects\contoso` that contains a single solution file contoso.sln, the following variations of the command will each restore all packages used by projects within the solution:

1. `D:\projects\contoso\> nuget.exe restore`
1. `D:\projects\contoso\> nuget.exe restore contoso.sln`
1. `D:\projects\> nuget.exe restore contoso`

These three use cases are the most common, but other scenarios do exist. For more information on the Restore and Install commands, see the [Command-Line Reference](Command-Line-Reference).

### Command-Line Package Restore wrapped in MSBuild

In order to use the command line based approach with existing build servers, such as [Team Foundation Build](http://msdn.microsoft.com/en-us/library/ms181710(v=VS.90).aspx), it's often desirable to wrap the command line in a regular MSBuild project as this has the following advantages:

1. Existing build servers already have support for running MSBuild projects
2. Many projects already have their own MSBuild-based build process
3. It provides built-in support for aggregated logging and error reporting  

This approach differs from the MSBuild-Integrated Package Restore as this doesn't run *while* building the sources but instead it runs this *before* anything else is built. 

You can find a more detailed walkthrough on the [Package Restore with Team Foundation Build](package-restore/team-build).

### MSBuild-Integrated Package Restore

Prior to NuGet 2.7, an MSBuild-Integrated Package Restore approach was used and promoted. While this approach is still available, the NuGet team suggests using the Automatic Package Restore and Command-Line Package Restore instead.

The MSBuild-Integrated approach requires user action to enable it for a solution. This is done from within the Visual Studio Solution Explorer by right-clicking on the solution and choosing 'Enable NuGet Package Restore'. On this gesture, the following actions occur:

1. NuGet downloads a copy of `NuGet.exe` and `NuGet.targets` from https://www.nuget.org.
1. NuGet saves these files, along with a `NuGet.config` file, into a `.nuget` folder at the root of the solution.
1. NuGet updates all projects in the soluton to have them import the `.nuget\NuGet.targets` file, extending MSBuild to invoke `NuGet.exe`'s [Install Command](Command-Line-Reference#Install-Command) during build.

After that initial setup, building the solution through either Visual Studio or MSBuild from the command-line will restore packages during the build. `NuGet.exe` verifies package restore consent before downloading any missing packages, verifying only the 'Allow NuGet to download missing packages' setting.

The MSBuild-integrated approach to package restore has some drawbacks that vary in severity by situation.

1. Requires additional files within the solution folder.
1. Requires a targets file to be imported into all projects in the solution; this can introduce issues when projects are shared among multiple solutions.
1. Projects fail to load if the `NuGet.targets` file cannot be found.
1. Projects don't build successfully if any of the restored NuGet packages extend MSBuild through a targets/props file import.

For more information on the MSBuild-Integrated Package Restore approach, visit the [Using NuGet without committing packages](../Consume/Package-Restore/MSBuild-Integrated) page.

To switch from MSBuild-Integrated Package Restore to Automatic Package Restore, see the walkthrough of [migrating to Automatic Package Restore](../Consume/Package-Restore/Migrating-to-Automatic-Package-Restore).

## Package Restore Consent

As mentioned above, Automatic Package Restore in Visual Studio and the MSBuild-Integrated Package Restore both verify that the user has granted consent before packages are downloaded from the user's configured package sources (which likely includes a package source from the public nuget.org gallery). The concept of package restore consent was introduced in NuGet 2.0 (which was included in Visual Studio 2012). Package restore consent was revised with NuGet 2.7 to address feedback received and improve the usability of package restore.

### NuGet 2.7+

Starting with NuGet 2.7, package restore consent is **ON** by default. This means that all users are implicitly opted into restoring missing packages during build. This eliminates hurdles encountered by users when attempting to build projects that use NuGet, especially if the user is unfamiliar with NuGet and the package restore consent concept. When building a solution in Visual Studio, any missing packages will be automatically downloaded during build, and a cancellable progress window will be shown. Additionally, a message will be written to the Output window to indicate that package restore executed.

### NuGet 2.0-2.6

Starting with NuGet 2.0 and continuing through NuGet 2.6, package restore consent was **OFF** by default. This resulted in users getting build errors in Visual Studio when NuGet packages were missing from their solution. Users had to explicitly opt in to package restore consent before solutions missing packages could be built either from within Visual Studio or from the command-line. This adversely affected many users' workflows and build servers, forcing them to explicitly opt in on all machines where NuGet was used to restore packages.

#### Opting In

There are two ways to opt into package restore consent, as needed by NuGet 2.0-2.6.

1. From Visual Studio's options, select the `Package Manager` node and its "General" settings. Check the box to "Allow NuGet to download missing packages during build" and click OK. *Note that in NuGet 2.7, the phrase "during build" was removed from this setting.* This setting is stored in the user's `%AppData%\NuGet\NuGet.config` file, but it can also be specified in any `NuGet.config` file that applies to the solution being built, as documented on the [NuGet Config File] page.
1. Specify an environment variable called `EnableNuGetPackageRestore` with a value of `true` before launching Visual Studio or MSBuild. 

#### Opting Out

Users uncomfortable automatically downloading missing packages during build have two levels for opting out of this feature, both included in the *Package Manager* General settings in Visual Studio:

1. **Allow NuGet to download missing packages**: Unchecking this will prevent NuGet from downloading packages during build or even checking for missing packages during build. This affects all three approaches for NuGet's package restore feature.
1. **Automatically check for missing packages during build in Visual Studio**: Unchecking this will disable the Automatic Package Restore feature in Visual Studio, but it will still allow other package restore approaches to work including the MSBuild-Integrated Package Restore.

#### Machine-Wide Configuration to Opt Out all Users

If desired, a user or company can employ machine-wide NuGet configuration to opt all users out of package restore consent by default. While users will still be able to apply additional NuGet configuration to re-enable package restore consent, it can provide a good default experience in some environments.

To accomplish this, NuGet's [config extensibility point](NuGet-Config-File#NuGet-config-extensibility-point) can be utilized to specify one or both of the opt out settings. Here's a sample `NuGet.config` file that opts users out of both global package restore consent as well as Automatic Package Restore.

	<?xml version="1.0" encoding="utf-8"?>
	<configuration>
	  <packageRestore>
	  	<!-- Opts out of both Automatic Package Restore and MSBuild-Integrated Package Restore -->
	    <add key="enabled" value="False" />

	    <!-- Opts out of Automatic Package Restore in Visual Studio -->
	    <add key="automatic" value="False" />
	  </packageRestore>
	</configuration>

### Package Restore Consent Errors with NuGet 2.7

If you are using NuGet 2.7+, but you are working in a solution that had enabled package restore through the MSBuild-integrated approach, it's possible that package restore will still fail due to a lack of package restore consent. This happens when the version of `NuGet.exe` in your solution's `.nuget` folder is version 2.6 or earlier, where package restore consent was still OFF by default.

If you have upgraded to NuGet 2.7+ but your solution fails to build stating that you haven't given consent, you have a few options for proceeding:

1. **Force save your NuGet settings with consent given.** To do this, open Visual Studio's options and under Package Manager, choose General. Uncheck and then re-check the boxes for consent and click OK. This forces your `%AppData%\NuGet\NuGet.config` file to be saved with consent explicitly given, allowing NuGet 2.6 and earlier to see that you've given consent.
1. **Update the version of `NuGet.exe` in your `.nuget` folder.** To do this, run `nuget.exe update -self` from your `.nuget` folder, which will download the latest version of `NuGet.exe` and replace the version in the `.nuget` folder. The latest version of `NuGet.exe` will infer consent to be ON even when not explicitly saved in the `NuGet.config` file.
1. **Migrate to Automatic Package Restore.** For this approach, you would migrate from the MSBuild-integrated package restore to the Automatic Package Restore approach, following the [documented walkthrough](../Consume/Package-Restore/Migrating-to-Automatic-Package-Restore).

#### Common issues with Automatic Package Restore

If you have Nuget 2.7+ installed; it's important to pick one method for managing Automatic Package Restore in Visual Studio.

Two methods are available:

 1. (Nuget 2.7+): Visual Studio -> Tools -> Package Manager -> Package Manager Settings -> Enable Automatic Package Restore
 2. (Nuget 2.6 and below) Right clicking on a solution and clicking "Enable Package Restore for this solution".
 
These are different methods; and have drastically different outcomes for developing with Nuget.

Method #1 causes the following to happen:

 - Packages are automatically downloaded by Visual Studio before the actual build
 - A `.nuget` folder is added with one file, `nuget.config`; this file *only* contains the `disableSourceControlIntegration` setting
 - Packages are put into a `packages` folder at the solution level
 - For Team Foundation Server 2013 and later, packages are automatically restored during build, providing that you're using a Team Build Template for Team Foundation Server 2013 or later. If you're using a previous version of build templates (perhaps because you've upgraded to TFS 2013), you'll have to migrate those build templates to TFS 2013 (essentially recreate the custom parts of the Build Templates using the appropriate template for your Source Control (TFVC or Git)).
 
Method #2 causes the following to happen:

 - Packages are automatically downloaded by Visual Studio and added to source control (if using TFVC)
 - A `.nuget` folder is added with three files, `Nuget.exe`, `Nuget.targets`, and `nuget.config`.
 - The `.csproj` or `.vbproj` files are edited to add `<PackageRestore>true</PackageRestore>` and references to the `nuget.targets` file and build actions based on `nuget.targets`.
 - For a custom build `.proj`, a pre build `<Exec>` action to restore nuget packages is required. This is not added automatically.
 
If you mix these two methods; you may encounter problems. If you're using Nuget 2.7+, it's recommended to choose Method #1.
 
## Further Reading

[Package Restore with Team Build](package-restore/team-build)

[Migrating to Automatic Package Restore](package-restore/migrating-to-automatic-package-restore)

[MSBuild-Integrated Package Restore](package-restore/msbuild-integrated)
