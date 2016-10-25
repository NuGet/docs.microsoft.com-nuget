#Supporting Multiple .NET Framework Versions

Many libraries target a specific version of the .NET Framework. For example, you might have one version of your library that's specific to UWP, and another version that takes advantage of features in .NET Framework 4.6. 

Fortunately, NuGet supports putting multiple versions of the same library in a single package through the convention-based working directory method described in [Creating a package](/ndocs/create-package/creating-a-package#from-a-convention-based-working-directory).

<div class="block-callout-info">
	<strong>Note</strong><br>
	Assemblies that have no associated framework name or version should be stored directly in the <em>lib</em> folder and not in separate folders.
</div>

In this topic:

- [Framework version folder structure](#framework-version-folder-structure)
- [Content files and PowerShell scripts](#content-files-and-powershell-scripts)
- [Matching assembly versions and the target framework in a project](matching-assembly-versions-and-the-target-framework-in-a-project)
- [Grouping assemblies by framework version](#grouping-assemblies-by-framework-version)
- [Determining which NuGet target to use](#determining-which-nuget-target-to-use)


## Framework version folder structure

When NuGet installs an assembly from a package, it checks the target .NET Framework version of the project, then selects the correct version of the assembly from the appropriate subfolder under `lib` in the package, provided those folders use the following convention: 
    
    lib\{framework name}[{version}]

For a complete list of supported names, see the [Target Frameworks reference](/ndocs/schema/target-frameworks#supported-frameworks). Note that names are case-sensitive 

The following example shows a folder structure that supports four versions of a library:

	\lib
	    \net46
	        \MyAssembly.dll
	    \net461
	        \MyAssembly.dll
	    \uap
	        \MyAssembly.dll
	    \netcore
	        \MyAssembly.dll


## Content files and PowerShell scripts

<div class="block-callout-warning">
	<strong>Note</strong><br>
	Mutable content file support and script execution is available in NuGEt 2.x, but deprecated in NuGet 3.x and later.
</div>

With NuGet 2.x, content files and PowerShell scripts can be grouped by target framework using the same folder convention inside the `content` and `tools` folders. For example:

    \content
	    \net46
	        \MyContent.txt
	    \net461
	        \MyContent461.txt
	    \uap
	        \MyUWPContent.html
	    \netcore
	\tools
	    init.ps1
	    \net46
	        install.ps1
	        uninstall.ps1
	    \uap
	        install.ps1
	        uninstall.ps1
            
If a framework folder is left empty, NuGet will not add assembly references or content files or run the PowerShell scripts for that framework.

<div class="block-callout-info">
    <strong>Note</strong><br>
    Because init.ps1 is executed at the solution level and not dependent on project, it must be placed directly under the <em>tools</em> folder. If placed under a framework folder, it will be ignored.
</div> 


## Matching assembly versions and the target framework in a project

When NuGet installs a package that has multiple assembly versions, it tries to match the framework name of the assembly with the target framework of the project. 

If a match is not found, NuGet copies the assembly for the highest version that is less than or equal to the project's target framework. 

For example, consider the following folder structure in a package:

	\lib
	    \net45
	        \MyAssembly.dll
	    \net461
	        \MyAssembly.dll


Installing this package in a project that targets .NET Framework 4.6, NuGet installs the assembly in the `net45` folder.

## Grouping assemblies by framework version

NuGet copies assemblies from only a single library folder in the package. For example, suppose a package has the following folder structure:

	\lib
	    \net40
	        \MyAssembly.dll (v1.0)
	        \MyAssembly.Core.dll (v1.0)
	    \net45
	        \MyAssembly.dll (v2.0)


To easily include all these files, use a wildcard in the &lt;files&gt; section of your `.nuspec`:

    <files>
	    <file src="lib\**" target="lib" />
    </files>

When the package is installed in a project that targets .NET Framework 4.5, `MyAssembly.dll (v2.0)` is the only assembly installed. `MyAssembly.Core.dll(v1.0)` is not installed because it's not listed in the `net45` folder. (One reason why NuGet behaves this way is that `MyAssembly.Core.dll` might have merged into version 2.0 of `MyAssembly.dll`.) 

If you want `MyAssembly.Core.dll` to be installed for .NET Framework 4.5, place a copy in the `net45` folder.

The rule about copying assemblies from only one folder also applies to the root `lib` folder. Suppose a package has the following folder structure:

	\lib
	    \MyAssembly.dll (v1.0)
	    \MyAssembly.Core.dll (v1.0)
	    \Net45
	        \MyAssembly.dll (v2.0)

In projects that target .NET Framework 4.0 and .NET Framework 3.5, NuGet copies both `MyAssembly.dll` and `MyAssembly.Core.dll` because their location in the package does not restrict them to a specific target. But as was true of the previous example, in projects that target .NET Framework 4.5, only `MyAssembly.dll` from the `net45` folder will be copied. If you want to include `MyAssembly.Core.dll` for .NET Framework 4.5, place a copy of it in the `net45` folder.


## Grouping assemblies by framework profile

NuGet also supports targeting a specific framework profile by appending a dash and the profile name to the end of the folder.

	lib\{framework name}-{profile}

For example, to target the Windows Phone profile, place your assembly in a folder named `sl3-wp`.

Profiles supported by NuGet :

* `client`: Client Profile
* `full`: Full Profile
* `wp`: Windows Phone
* `cf`: Compact Framework

## Determining which NuGet target to use

When packaging libraries targeting the Portable Class Library it can sometimes to be tricky to determine which NuGet target you should use in your folder names and `.nuspec` file, especially if targeting only a subset of the PCL. Here are some links to useful external resources to help you with this:

* [Framework profiles in .NET](http://blog.stephencleary.com/2012/05/framework-profiles-in-net.html) (stephenclearly.com)
* [Portable Class Library profiles](http://embed.plnkr.co/03ck2dCtnJogBKHJ9EjY/preview) (plnkr.co): Table enumerating PCL profiles and their equivalent NuGet targets
* [Portable Class Library profiles tool](https://github.com/StephenCleary/PortableLibraryProfiles) (github.com): command line tool for determining PCL profiles available on your system
