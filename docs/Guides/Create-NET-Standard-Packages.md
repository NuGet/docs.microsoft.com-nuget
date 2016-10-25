#Create .NET Standard Packages

The [.NET Standard Library]((https://docs.microsoft.com/en-us/dotnet/articles/standard/library)) is a formal specification of .NET APIs intended to be available on all .NET runtimes, thus establishing greater uniformity in the .NET ecosystem. The .NET Standard Library defines a uniform set of BCL (Base Class Library) APIs for all .NET platforms to implement, independent of workload. It enables developers to produce PCLs that are usable across all .NET runtimes, and reduces if not eliminates platform-specific conditional compilation directives in shared code. 

This guide will walk you through creating a nuget package targeting .NET Standard Library 1.4. This will work across .NET Framework 4.6.1, Universal Windows Platform 10, .NET Core, and Mono/Xamarin. For details, see the [.NET Standard mapping table](#table) later in this topic.

1. [Pre-requisites](#pre-requisites)
2. [Create the class library project](#create-the-class-library-project)
3. [Create and update the .nuspec file](#create-and-update-the--nuspec-file)
4. [Package the component](#package-the-component)
5. [Additional options](#additional-options)
5. [.NET Standard mapping table](#-net-standard-mapping-table)
6. [Related topics](#related-topics)


##Pre-requisites
1. Visual Studio 2015. Install the Community edition for free from [visualstudio.com](https://www.visualstudio.com/); you can use the Professional and Enterprise editions as well, of course. 
2. .NET Core: Install .NET Core along with templates and other tools for Visual Studio 2015 from [https://go.microsoft.com/fwlink/?LinkId=824849](https://go.microsoft.com/fwlink/?LinkId=824849).
3. NuGet CLI. Download the latest version of nuget.exe from [nuget.org/downloads](https://nuget.org/downloads), saving it to a location of your choice. Then add that location to your PATH environment variable if it isn't already.

<div class="block-callout-info">
	<strong>Note</strong><br>
	nuget.exe is the CLI tool itself, not an installer, so be sure to save the downloaded file from your browser instead of running it.
</div>


##Create the class library project
1. In Visual Studio, **File > New > Project**, expand the **Visual C# > Windows** node, select **Class Library (Portable)**, change the name to AppLogger, and click OK.
 
	![Create new class library project](/images/BuildForNetStandard/01.PNG)

2. In the **Add Portable Class Library** dialog that appears, select the `.NET Framework 4.6` and `ASP.NET Core 1.0` options.
3. Right-click the `AppLogger (Portable)` in Solution Explorer, select **Properties**, select the **Library** tab, then click **Target .NET Platform Standard** in the **Targeting** section. This will prompt for confirmation, after which you can select `.NET Standard 1.4` from the drop down:

	![Setting the target to .NET Standard 1.4](/images/BuildForNetStandard/02-ChangeTarget.png)

5. Click on the **Build** tab, change the **Configuration** to `Release`, and check the box for **XML documentation file**.
6. Add your code to the component, for example:

		namespace AppLogger
		{
			public class Logger
			{
				public void Log(string text)
				{
					throw new NotImplementedException("Called Log");
				}
			}
		}

6. Build the project (with the Release configuration) and check that DLL and XML files are produced within the bin\Release folder.

##Create and update the .nuspec file

1. Open a command prompt, navigate to the folder containing `AppLogg.csproj` folder (one level below where the .sln file is), and run the NuGet `spec` command to create the initial `AppLogger.nuspec` file:

	<code class="bash hljs">
		nuget spec
	</code>

2. Open `AppLogger.nuspec` in an editor and update it to match the following, replacing YOUR_NAME with an appropriate value. The &lt;id&gt; value, specifically, must be unique across nuget.org (see the naming conventions described in [Creating a package](/ndocs/create-packages/creating-a-package(#choosing-a-unique-package-identifier-and-setting-the-version-number)). Also note that you must also update the author and description tags or you'll get an error during the packing step.

	<?xml version="1.0"?>
	<package >
	  <metadata>
		<id>AppLogger.YOUR_NAME</id>
		<version>1.0.0</version>
		<title>AppLogger</title>
		<authors>YOUR_NAME</authors>
		<owners>YOUR_NAME</owners>
		<requireLicenseAcceptance>false</requireLicenseAcceptance>
		<description>Awesome application logging utility</description>
		<releaseNotes>First release</releaseNotes>
		<copyright>Copyright 2016 (c) Contoso Corporation. All rights reserved.</copyright>
		<tags>logger logging logs</tags>
	  </metadata>
	</package>

3. Add reference assemblies to the nuspec file, namely the library's DLL and the IntelliSense XML file:
 
	 	<!-- Insert below <metadata> element -->
		<files>
			<file src="bin\Release\AppLogger.dll" target="lib\netstandard1.4\AppLogger.dll" />
			<file src="bin\Release\AppLogger.xml" target="lib\netstandard1.4\AppLogger.xml" />			
		</files> 

3. Right-click the solution and select **Build Solution** to generate all the files for the package. 


##Package the component
With the completed .nuspec referencing all the files you need to include in the package, you're ready to run the `pack` command:

<code class="bash hljs">
	nuget pack AppLogger.nuspec
</code>

This will generate `AppLogger.YOUR_NAME.1.0.0.nupkg`. Opening this file in a tool like the [NuGet Package Explorer](https://github.com/NuGetPackageExplorer/NuGetPackageExplorer) and expanding all the nodes, you'll see the following contents:

![NuGet Package Explorer showing the AppLogger package](/images/BuildForNetStandard/03-PackageExplorer.PNG)

<div class="block-callout-info">
	<strong>Note</strong><br>
	A .nupkg file is just a ZIP file with a different extension. You can also examine package contents, then, by change .nupkg to .zip, but remember to restore the extension before uploading a package to nuget.org. 
</div>

To make your package available to other developers,  follow the instructions on [Publish a package](/ndocs/create-packages/publish-a-package).


##Additional options

The following sections go into additional options for NuGet package creation:

- [Declaring dependencies](#declaring-dependencies)
- [Supporting multiple target frameworks](#supporting-multiple-target-frameworks)
- [Adding targets and props for MSBuild](#adding-targets-and-props-for-msbuild)
- [Creating localized packages](#creating-localized-packages)
- [Adding a readme](#adding-a-readme)

###Declaring dependencies
If you have any dependencies on other NuGet packages, list those in the &lt;dependencies&gt; element with &lt;group&gt; elements. For example, to declare a dependency on NewtonSoft.Json 8.0.3 or above, add the following:

	<!-- Insert within the <metadata> element -->
	<dependencies>
		<group targetFramework="uap">
			<dependency id="Newtonsoft.Json" version="8.0.3" />
		</group>
	</dependencies>

The syntax of the *version* attribute here indicates that version 8.0.3 or above is acceptable. To specify different version ranges, refer to [Dependency Versions](/ndocs/create-packages/dependency-versions).

###Supporting multiple target frameworks
Suppose you'd like to take advantage of an API in .NET Framework 4.6.2 that is not available in .NET Standard 1.4. To do this, you'll first need to make sure the library compiles for .NET 4.6.2 by using conditional compilation or shared projects. (In Visual Studio, you can create a NetCore project, add the framework of choice to the mutliple framework section, and then build.) Then you create the package using the simple convention-based working directory technique as follows: 

1. In the project's root folder containing your .nuspec file, create a folder named `lib`.
2. Inside `lib`, create folders for each platform you want to support:
		
		\lib
			\netstandard1.4
				\AppLogger.dll
			\net462
				\AppLogger.dll

3. In the .nuspec file, add a `files` node under the `package` node and refer to the files in `lib` using wildcards. <b>Note:</b> Token replacements are not supported with the convention-based working directory approach, so replace them with literal values:

		<?xml version="1.0"?>
		<package >
		  <metadata>
			<id>AppLogger.YOUR_NAME</id>
			<version>1.0.0.0</version>
			<title>AppLogger</title>
			<authors>YOUR_NAME</authors>
			<owners>YOUR_NAME</owners>
			<requireLicenseAcceptance>false</requireLicenseAcceptance>
			<description>Awesome application logging utility</description>
			<releaseNotes>First release.</releaseNotes>
			<copyright>Copyright 2016</copyright>
			<tags>logger logging logs</tags>
		  </metadata>
		  <files>
			  <file src="lib\**" target="lib" />
		  </files>
		</package>

4. Create the package again using `nuget pack AppLogger.spec`. 
		
For more details on using this technique, see [Supporting Multiple .NET Framework Versions](/ndocs/create-packages/supporting-multiple-target-frameworks)

###Adding targets and props for MSBuild

In some cases you might want to add custom build targets or properties in projects that consume your package, such as running a custom tool or process during build. You do this by adding files in a `\build` folder as described in the steps below. When NuGet installs a package with \build files, it adds an MSBuild element in the project file pointing to the .targets and .props files. 

<div class="block-callout-info">
	<strong>Note</strong><br>
	When using project.json, targets are not added to the project but are made available through the project.lock.json.
</div>

1. In the project folder containing the your .nuspec file, create a folder named `build`.
2. Inside `build`, create folders for each supported, and within those place your .targets and .props files:
		
		\build
			\netstandard1.4
				\AppLogger.props
				\AppLogger.targets
			\net462
				\AppLogger.props
				\AppLogger.targets

3. In the .nuspec file, add a `files` node under the `package` node and refer to the files in `build` using wildcards.

		<?xml version="1.0"?>
		<package >
		  <metadata>...
		  </metadata>
		  <files>
			  <file src="build\**" target="build" />
		  </files>
		</package>

4. Create the package again using `nuget pack AppLogger.nuspec`.

For additional details, refer to [Include MSBuild props and targets in a package](/ndocs/create-packages/creating-a-package#including-msbuild-props-and-targets-in-a-package).


###Creating localized packages
To create localized versions of your library, you can either create separate packages for different locales, or include localized resource assemblies within a single package. Here's how to do the latter approach for German and Italian: 

1. Within each target framework folder under `lib`, create folders for each supported language other than the English default. In these folders you can place resource assemblies  and localized IntelliSense XML files. For example: 
	
	    lib
	    ├───netstandard1.4
	    │   │   AppLogger.dll
	    │   │   AppLogger.xml
	    │   │
	    │   ├───de
	    │   │       AppLogger.resources.dll
	    │   │       AppLogger.xml
	    │   │
	    │   └───it
	    │           AppLogger.resources.dll
	    │           AppLogger.xml
	    └───net462
	        │   AppLogger.dll
	        │   AppLogger.xml
	        │
	        ├───de
	        │       AppLogger.resources.dll
	        │       AppLogger.xml
	        │
	        └───it
	                AppLogger.resources.dll
	                AppLogger.xml
	    
2. In the .nuspec file, reference these files in the &lt;files&gt; node:

		<?xml version="1.0"?>
		<package>
		  <metadata>...
		  </metadata>
		  <files>
		    <file src="lib\**" target="lib" />
		  </files>
		</package>

3. Create the package again using `nuget pack AppLogger.nuspec`.
		

###Adding a readme
When you include a `readme.txt` file in the root of the package, Visual Studio will display it when the package is installed directly.

<div class="block-callout-info">
	<strong>Note</strong><br>
	Readme files are not shown for packages that are installed as a dependency, or for .NET Core projects.
</div>

To do this, create your `readme.txt` file, place it in the project root folder, and refer to it in the .nuspec file:

	<?xml version="1.0"?>
	<package >
	  <metadata>...
	  </metadata>
	  <files>
	    <file src="readme.txt" target="" />
	  </files>
	</package>


## .NET Standard mapping table

<table class="reference">
	<tr>
		<th>Platform Name</th>
		<th>Alias</th>
		<th> </th>
		<th> </th>
		<th> </th>
		<th> </th>
		<th> </th>
		<th> </th>
		<th> </th>
    <tr>
        <td>.NET Standard</td>
        <td>netstandard</td>
		<td>1.0</td>
		<td>1.1</td>
		<td>1.2</td>
		<td>1.3</td>
		<td>1.4</td>
		<td>1.5</td>
		<td>1.6</td>
    </tr>
	<tr>
		<td>.NET Core</td>
		<td>netcoreapp</td>
		<td>&#x2192;</td>
		<td>&#x2192;</td>
		<td>&#x2192;</td>
		<td>&#x2192;</td>
		<td>&#x2192;</td>
		<td>&#x2192;</td>
		<td>1.0</td>
    <tr>
	<tr>
		<td>.NET Framework</td>
		<td>net</td>
		<td>&#x2192;</td>
		<td>4.5</td>
		<td>4.5.1</td>
		<td>4.6</td>
		<td>4.6.1</td>
		<td>4.6.2</td>
		<td>4.6.3</td>
    <tr>
	<tr>
		<td>Mono/Xamarin Platforms</td>
		<td></td>
		<td>&#x2192;</td>
		<td>&#x2192;</td>
		<td>&#x2192;</td>
		<td>&#x2192;</td>
		<td>&#x2192;</td>
		<td>&#x2192;</td>
		<td>*</td>
    <tr>
	<tr>
		<td>Universal Windows Platform</td>
		<td>uap</td>
		<td>&#x2192;</td>
		<td>&#x2192;</td>
		<td>&#x2192;</td>
		<td>&#x2192;</td>
		<td>10.0</td>
		<td></td>
		<td></td>
    <tr>
	<tr>
		<td>Windows</td>
		<td>win</td>
		<td>&#x2192;</td>
		<td>8.0</td>
		<td>8.1</td>
		<td></td>
		<td></td>
		<td></td>
		<td></td>
    <tr>
	<tr>
		<td>Windows Phone</td>
		<td>wpa</td>
		<td>&#x2192;</td>
		<td>&#x2192;</td>
		<td>8.1</td>
		<td></td>
		<td></td>
		<td></td>
		<td></td>
    <tr>
	<tr>
		<td>Windows Phone Silverlight</td>
		<td>wp</td>
		<td>8.0</td>
		<td></td>
		<td></td>
		<td></td>
		<td></td>
		<td></td>
		<td></td>
    <tr>	
</table>


##Related topics
 
* [Nuspec Reference](/ndocs/schema/nuspec)
* [Symbol packages](/ndocs/create-packages/symbol-packages)
* [Dependency Versions](/ndocs/create-packages/dependency-versions)
* [Supporting Multiple .NET Framework Versions](/ndocs/create-packages/supporting-multiple-target-frameworks)
* [Include MSBuild props and targets in a package](/ndocs/create-packages/creating-a-package#including-msbuild-props-and-targets-in-a-package)
* [Creating Localized Packages](/ndocs/create-packages/creating-localized-packages)
* [.NET Standard Library documentation](https://docs.microsoft.com/en-us/dotnet/articles/standard/library)
* [Porting to .NET Core from .NET Framework](https://docs.microsoft.com/en-us/dotnet/articles/core/porting/index)

