#Supporting Multiple .NET Framework Versions

Many libraries target a specific version of the .NET Framework. For example, you might have one version of your library that's 
specific to UWP, and another version of the same library that takes advantage of .NET Framework 4 features. 
You do not need to create separate packages for each of these versions. NuGet supports putting multiple versions of the 
same library in a single package keeping them in separate folders within the package.

## Framework Version Folder Structure

When NuGet installs an assembly from a package, it checks the target .NET Framework version of the project 
you are adding the package to. NuGet then selects the correct version of the assembly in the package by selecting 
the correct subfolder within the `lib` folder. 

To enable NuGet to do this, you use the following naming convention to indicate which assemblies go 
with which framework versions:

    lib\{framework name}{version}


The following example shows a folder structure that supports four versions of a library:

	\lib
	    \net46
	        \MyAssembly.dll
	    \net461
	        \MyAssembly.dll
	    \uap
	        \MyAssembly.dll
	    \netcore
	        MyAssembly.dll

### Content Files and PowerShell Scripts

<div class="block-callout-warning">
    <strong>Deprecated in NuGet 3.0+:</strong><br>
    Mutable content file support and script execution is deprecated in NuGet 3.0+
</div>

In NuGet 2.x, in addition to assembly references, content files as well as PowerShell scripts can be grouped by target frameworks too. The framework folder structure inside `lib` folder described above  applies exactly the same to `content` and `tools` folders.

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
            
A framework folder can be *empty*, in which case, NuGet will not add assembly references or content files or run the PowerShell scripts for the particular framework version.

<div class="block-callout-info">
    <strong>Note</strong><br>
   Because init.ps1is executed at the solution level and not dependent on project, it must be placed directly under the `tools` folder. If placed under a framework folder, it will be ignored.
</div>

## Framework Names

NuGet attempts to parse the folder name into a [FrameworkName](http://msdn.microsoft.com/en-us/library/dd414023.aspx) 
object. Names are case insensitive, and you can use abbreviations for both framework name and version number.
 
If you omit the framework name, the .NET Framework is assumed. For example, the following folder structure is equivalent to the previous one:

	\lib
	    \net46
	        \MyAssembly.dll
	    \uap
	        \MyAssembly.dll

**Recommended Reading**: [List of Target Frameworks](/ndocs/schema/Target-Frameworks.md)

## Assemblies that are not Specific to a Framework Version

Assemblies that have no associated framework name or version are stored directly in the `lib` folder.

## Matching Assembly Version to the Target Framework of a Project

When NuGet installs a package that has multiple assembly versions, it tries to match the framework name of the 
assembly with the target framework of the project. 

If a match is not found, NuGet copies the assembly that's for the highest version that is less than or 
equal to the project's target framework. 

For example, given the follow folder structure: 

	\lib
	    \net45
	        \MyAssembly.dll
	    \net461
	        \MyAssembly.dll

If you install a package that has the `lib` folder structure shown in the previous example 
in a project that targets the .NET Framework 4.6, the assembly in the `net461` folder (for .NET Framework 4.6.1) is selected.

## Grouping Assemblies by Framework Version

NuGet copies assemblies from only a single library folder. For example, suppose a package has the following folder structure:

	\lib
	    \Net40
	        \MyAssembly.dll (v1.0)
	        \MyAssembly.Core.dll (v1.0)
	    \Net45
	        \MyAssembly.dll (v2.0)

When the package is installed in a project that targets the .NET Framework 4.5, *MyAssembly.dll (v2.0)* is the only assembly installed. *MyAssembly.Core.dll (v1.0)* is not installed. (One reason why NuGet behaves this way is that *MyAssembly.Core* might have been merged into version 2.0 of *MyAssembly*.) 

In this example, if you want *MyAssembly.Core.dll* to be installed in a project that targets the .NET Framework 4.5, 
you must include it in the `Net45` folder as well as in the `Net40` folder.

The rule about copying assemblies from only one folder also applies to the root `lib` folder. Suppose a package has the following folder structure:

	\lib
	    \MyAssembly.dll (v1.0)
	    \MyAssembly.Core.dll (v1.0)
	    \Net45
	        \MyAssembly.dll (v2.0)

In projects that target the .NET Framework 4.0 and the .NET Framework 3.5, NuGet copies both *MyAssembly.dll* and *MyAssembly.Core.dll*. But as was true of the previous example, in projects that target the .NET Framework 4.5, only *MyAssembly.dll* from the `Net45` folder will be copied. 

As in the previous example, if you want *MyAssembly.Core.dll* to be installed in a project that targets the .NET Framework 4.5, you must include it in the *Net45* folder.

## Grouping Assemblies by Framework Profile

NuGet also supports targeting a specific framework profile by appending a dash and the profile name to the end of the folder.

	lib\{framework name}-{profile}

For example, to target the Windows Phone profile, place your assembly in a folder named `sl3-wp`.

Profiles supported by NuGet include:

* Client - Client Profile
* Full - Full Profile
* WP - Windows Phone

<table class="reference">
    <tr><th>Profile Name</th><th>Abbreviations</th></tr>
    <tr><td>Client</td><td>client</td></tr>
    <tr><td>WindowsPhone</td><td>wp</td></tr>
    <tr><td>CompactFramework</td><td>cf</td></tr>
    <tr><td>Full</td><td>full</td></tr>
</table>


## Determining which NuGet Target to use

When packaging libraries targeting the Portable Class Library it can sometimes to be tricky to determine which NuGet Target you should use in your folder names and manifest files especially if targeting only a subset of the PCL.  Here are some links to useful external resources to help you with this:

* [Framework Profiles in .Net](http://blog.stephencleary.com/2012/05/framework-profiles-in-net.html)
* [Portable Class Library Profiles](http://embed.plnkr.co/03ck2dCtnJogBKHJ9EjY/preview) - Table enumerating PCL profiles and their equivalent NuGet targets
* [Portable Library Profiles Tool](https://github.com/StephenCleary/PortableLibraryProfiles) - Command line tool for determining PCL profiles available on your system
