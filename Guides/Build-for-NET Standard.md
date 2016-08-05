#Build for .NET Standard
This guide will walk you through creating a nuget package that takes advantage of the .NET Standard.

##What is .NET Standard
The .NET Standard Library is a formal specification of .NET APIs that are intended to be available on all .NET runtimes. The motivation behind the Standard Library is establishing greater uniformity in the .NET ecosystem. 

* [Read more about the .NET Standard Library](https://docs.microsoft.com/en-us/dotnet/articles/standard/library)
* [Porting to .NET Core from .NET Framework](https://docs.microsoft.com/en-us/dotnet/articles/core/porting/index)

##Why should you use it
The .NET Standard Library enables the following key scenarios:

* Defines uniform set of BCL (Base Class Library) APIs for all .NET platforms to implement, independent of workload.
* Enables developers to produce portable libraries that are usable across .NET runtimes, using this same set of APIs.
* Reduces and hopefully eliminates conditional compilation of shared source due to .NET APIs, only for OS APIs.

##Pre-requisites
1. [Visual Studio 2015 Update 3](https://www.visualstudio.com/news/releasenotes/vs2015-update3-vs). If you don't have Visual Studio already, you can download [Visual Studio Community 2015](https://www.visualstudio.com/downloads/download-visual-studio-vs) for free.
2. [.NET Core Tooling Preview 2 for Visual Studio 2015](https://go.microsoft.com/fwlink/?LinkId=817245). This installs templates and other tools for Visual Studio 2015, as well as .NET Core 1.0 itself.
3. NuGet CLI - Download the latest version of nuget.exe from [nuget.org/downloads](https://nuget.org/downloads), move it to a common location and add this path to the PATH Environment Variable. For more details, take a look at [The NuGet Install guide](/ndocs/guides/install-nuget#nuget-cli)

##What are we building
We are going to create a NuGet package that works across .NET framework 4.6.1, Universal Windows Platform 10, .NET Core and Mono/Xamarin. 
The table below maps .NET Standard versions to various implementations:

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

Referenceing this table and based on our requirements, we will target .NET Standard 1.4. For more information on .NET Standard, read the [.NET Standard Library documentation](https://docs.microsoft.com/en-us/dotnet/articles/standard/library).

##Create new Project
In Visual Studio, choose File, New, Project. In the New Project dialog, expand the Visual C# node and choose the Windows node, and then choose Class Library (Portable). Change the name to AppLogger.

![Create new Project](/images/BuildForNetStandard/01.PNG)

In the Add Portable Class Library dialog, select the .NET Framework 4.6 and ASP.NET Core 1.0 options.

##Modify project.json
From the solution explorer, open project.json. It will look like:

	{
	  "supports": {
		"net46.app": {},
		"dnxcore50.app": {}
	  },
	  "dependencies": {
		"Microsoft.NETCore": "5.0.0",
		"Microsoft.NETCore.Portable.Compatibility": "1.0.0"
	  },
	  "frameworks": {
		"dotnet": {
		  "imports": "portable-net452"
		}
	  }
	}

From the context menu for the project, select Properties. Under the Targeting section, click on Target .NET Platform Standard. Click Yes on the confirmation dialog.

![Project Properties](/images/BuildForNetStandard/02.PNG)

Based on the table, we have determined that we will target .NET Standard 1.4. Select .NETStandard1.4 from the drop down.

![Create new Project](/images/BuildForNetStandard/05.PNG)


The project.json should now look like:

	{
	  "supports": {},
	  "dependencies": {
		"Microsoft.NETCore.Portable.Compatibility": "1.0.1",
		"NETStandard.Library": "1.6.0"
	  },
	  "frameworks": {
		"netstandard1.4": {}
	  }
	}

"Microsoft.NETCore.Portable.Compatibility"	enables compatiblity with portable libraries targeting previous .NET releases like .NET Framework 4.0 and Silverlight.

**Recommended Reading:** [Introduction to project.json](/ndocs/consume-packages/projectjson-intro)

##Add your code
We are leaving the code of the library as an exercise for the reader of the guide.

	namespace AppLogger
	{
		public class Class1
		{
			//Add your code
		}
	}

##Create the .nuspec file

Bring up a Windows command prompt (e.g. by using Windows + X and choosing Command Prompt) and navigate to the folder containing the `.csproj` file for the project that you just created. This path will look something like this
	`C:\Users\username\Documents\Visual Studio 2015\Projects\AppLogger\AppLogger`

Then run the `spec` command (ensure you've added nuget.exe to the PATH as discussed in Prerequisites above)

<code class="bash hljs">
	nuget spec
</code>

This will generate a new file `AppLogger.nuspec`. Open this file. It will look something like:

	<?xml version="1.0"?>
	<package >
	  <metadata>
		<id>$id$</id>
		<version>$version$</version>
		<title>$title$</title>
		<authors>$author$</authors>
		<owners>$author$</owners>
		<licenseUrl>http://LICENSE_URL_HERE_OR_DELETE_THIS_LINE</licenseUrl>
		<projectUrl>http://PROJECT_URL_HERE_OR_DELETE_THIS_LINE</projectUrl>
		<iconUrl>http://ICON_URL_HERE_OR_DELETE_THIS_LINE</iconUrl>
		<requireLicenseAcceptance>false</requireLicenseAcceptance>
		<description>$description$</description>
		<releaseNotes>Summary of changes made in this release of the package.</releaseNotes>
		<copyright>Copyright 2016</copyright>
		<tags>Tag1 Tag2</tags>
	  </metadata>
	</package>

This file includes tokens that are meant to be replaced at pack time, based on the project metadata stored in AssemblyInfo.cs (This can be found by expanding the properties node in the solution explorer.)
To know more about how tokens are handled, read [Creating a nuspec file](/ndocs/create-packages/create-a-package#create-a--nuspec-file)

<div class="block-callout-warning">
	You must update the author and description or you will get an error in the next step.
</div>

Here is how the updated nuspec file looks:

	<?xml version="1.0"?>
	<package >
	  <metadata>
		<id>$id$</id>
		<version>$version$</version>
		<title>$title$</title>
		<authors>karann</authors>
		<owners>karann</owners>
		<requireLicenseAcceptance>false</requireLicenseAcceptance>
		<description>Awesome application logging utility</description>
		<releaseNotes>First release</releaseNotes>
		<copyright>Copyright 2016</copyright>
		<tags>application app logger logging logs</tags>
	  </metadata>
	</package>


It is a good practice to update the metadata tags making it easier for others to find the package and understand what it does and how to use it. Having finalized the nuspec file, we are now ready to create the nuget package.

**Recommended Reading:** [nuspec reference](/ndocs/schema/nuspec)

##Pack
On the **Build** menu, choose **Build Solution**.

Now run the `pack` command

<code class="bash hljs">
	nuget pack AppLogger.csproj
</code>
	

You will get warnings if you haven't updated the release notes and tags from the default value. When the command has run successfully, it will generate a new file `AppLogger.1.0.0.nupkg`. This is your nuget package.

The following sections will go into more advanced scenarios around NuGet package creation.

##Adding Dependencies
Let's say your library has a dependency on another nuget package, say Newtonsoft.Json. Here's how you would change the nuspec file to add a dependency on Newtonsoft.Json 8.0.3

	<?xml version="1.0"?>
	<package >
	  <metadata>
		<id>$id$</id>
		<version>$version$</version>
		<title>$title$</title>
		<authors>karann</authors>
		<owners>karann</owners>
		<licenseUrl>http://LICENSE_URL_HERE_OR_DELETE_THIS_LINE</licenseUrl>
		<projectUrl>http://PROJECT_URL_HERE_OR_DELETE_THIS_LINE</projectUrl>
		<iconUrl>http://ICON_URL_HERE_OR_DELETE_THIS_LINE</iconUrl>
		<requireLicenseAcceptance>false</requireLicenseAcceptance>
		<description>Awesome application logging utility</description>
		<releaseNotes>First release</releaseNotes>
		<copyright>Copyright 2016</copyright>
		<tags>application app logger logging logs</tags>
		<dependencies>
		  <group>
			<dependency id="Newtonsoft.Json" version="8.0.3" />
		  </group>
		</dependencies>
	  </metadata>
	</package>

Specifying a dependency this way implies your library requires Newtonsoft.Json at a minimum version 8.0.3. This is the recommended way of specifying dependencies i.e. to only specify a lower bound, and leave the upper bound open. NuGet also supports using interval notation for specifying version ranges. Take a look at the [Dependency Versions](/ndocs/create-packages/dependency-versions) doc for more details.

##Multiple Target Frameworks
Let's say you would also like to target .NET Framework 4.6.2 because you would like to take advantage of a new API that is not supported by the .NET Framework 4.6.1 API surface. But, .NET Framework 4.6.2 is not available in .NET Standard 1.4. To do this, we will use the approach of creating a nupkg from a convention based working directory. The package author will have to do the necessary steps to make sure that their library compiles for .NET 4.6.2 by using techniques like conditional compilation and/or using shared projects.

1. In the root directory of the project (folder containing the `.nuspec` file), create a new folder - `lib`
2. Inside `lib`, create two new folders - one for each platform that we want to support.
		<pre>
		\lib
			\netstandard1.4
				\AppLogger.dll
			\net462
				\AppLogger.dll
		</pre>

3. Edit the nuspec file - add a child node `files` to the `package` node. <b>Note:</b> Token replacements are not supported when using the convention based working directory approach. So you should replace these tokens with hardcoded values.

		<?xml version="1.0"?>
		<package >
		  <metadata>
			<id>AppLogger</id>
			<version>1.0.0.0</version>
			<title>AppLogger</title>
			<authors>karann</authors>
			<owners>karann</owners>
			<licenseUrl>http://LICENSE_URL_HERE_OR_DELETE_THIS_LINE</licenseUrl>
			<projectUrl>http://PROJECT_URL_HERE_OR_DELETE_THIS_LINE</projectUrl>
			<iconUrl>http://ICON_URL_HERE_OR_DELETE_THIS_LINE</iconUrl>
			<requireLicenseAcceptance>false</requireLicenseAcceptance>
			<description>Awesome application logging utility</description>
			<releaseNotes>First release.</releaseNotes>
			<copyright>Copyright 2016</copyright>
			<tags>application app logger logging logs</tags>
		  </metadata>
		  <files>
			  <file src="lib\**" target="lib" />
		  </files>
		</package>

4. Now run the `pack` command

	<code class="bash hljs">
		nuget pack AppLogger.nuspec
	</code>
		
**Recommended Reading:** [Specifying Files to Include in the Package](/ndocs/schema/nuspec#specifying-files-to-include-in-the-package)

##Targets and Props for MSBuild
In some cases you might want to add custom [build targets or properties](add link here) to the consumers of your package, for example if you need a custom tool or process to run during build. When NuGet installs a package with \build files, it will add an MSBuild element in the project file pointing to the .targets and .props files. The .props file is added at the top, whereas the .targets file is added to the bottom.

<div class="block-callout-info">
	In the project.json world, targets are not added to the project but are made available through the project.lock.json.
</div>

1. In the root directory of project (folder containing the `.nuspec` file), create a new folder - `build`
2. Inside `build`, create two new folders - one for each platform that we want to support. This is where you would place `.targets` and `.props` files
	<pre>
		\build
			\netstandard1.4
				\AppLogger.props
				\AppLogger.targets
			\net462
				\AppLogger.props
				\AppLogger.targets
	</pre>

3. Edit the nuspec file - add a child node `files` to the `package` node

		<?xml version="1.0"?>
		<package >
		  <metadata>...
		  </metadata>
		  <files>
			  <file src="build\**" target="build" />
		  </files>
		</package>

4. Now run the `pack` command

	<code class="bash hljs">
		nuget pack AppLogger.nuspec
	</code>

**Recommended Reading:** [Import MSBuild targets and props files into project](/ndocs/create-packages/create-a-package#import-msbuild-targets-and-props-files-into-project)

##Creating localized packages
There are two options for providing a localized experience for your library package:

1. Include your localized satellite assemblies in the same NuGet package as your runtime assemblies.
2. Create separate localized satellite packages.

We are going to take the first approach. Let's say, you would like to support German and Italian.

1. Create a new folder for the languages (other than english) that we are trying to support under the `lib` folder.

	<pre>
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
    </pre>

	This package contains a single class library (AppLogger.dll) that contains the English strings as part of the runtime assembly. The package also contains localized satellite assemblies and XML IntelliSense files for German and Italian.

2. Edit the nuspec file - add a child node `files` to the `package` node

		<?xml version="1.0"?>
		<package>
		  <metadata>...
		  </metadata>
		  <files>
		    <file src="lib\**" target="lib" />
		  </files>
		</package>

3. Now run the `pack` command
		<code class="bash hljs">
			nuget pack AppLogger.nuspec
		</code>

**Recommended Reading:** [Creating Localized Packages](/ndocs/create-packages/creating-localized-packages)

##Adding a readme
A package can include a *readme.txt* file in the root of the package. This file will be displayed in Visual Studio immediately after the package is installed.

<div class="block-callout-info">
	<strong>Note:</strong> When your package is being consumed by a .NET Core project, it does not display the readme.txt.
</div>

To do this create a text file and edit its content to whatever you would like to be displayed once the package is installed.
Rename it to readme.txt. Edit the nuspec file - add a child node `files` to the `package` node like below

	<?xml version="1.0"?>
	<package >
	  <metadata>...
	  </metadata>
	  <files>
	    <file src="readme.txt" target="" />
	  </files>
	</package>

If the package is installed because it is a dependency of another package, the *readme.txt* file will not be opened. Only the *readme.txt* file of the package that the user is explicitly installing will be shown.

**Recommended Reading:** [Adding files to nuget packages](add link here)

##Publish
Go to [nuget.org](https://www.nuget.org/) and register for an account or login if you already have one.

Click on <b>My Account</b> to see the API Key that was generated for you.

![api key](/images/CreatePublishNugetSample/03.PNG)

<div class="block-callout-warning">
	Always keep your API key a secret! If your key is accidentally revealed, you can always regenerate it at any time. You can also remove the API key if necessary. 
</div>

Open your console and run the following command. Replace the key below with the key that was generated for you.

<code class="bash hljs">
	nuget push AppLogger.1.0.0.0.nupkg 47be3377-c434-4c29-8576-af7f6993a54b -Source https://www.nuget.org/api/v2/package
</code>
	
You should see something like this when the command has successfully executed.

<code class="bash hljs">
	Pushing AppLogger.1.0.0.0.nupkg to 'https://www.nuget.org/api/v2/package'...<br>
	PUT https://www.nuget.org/api/v2/package/ <br>
	Created https://www.nuget.org/api/v2/package/ 6829ms <br>
	Your package was pushed.
</code>

You can go to your account on nuget.org and <b>Manage my packages</b> to see the package that you just published. You should also receive an email notifying you that the package was just published.

It might take a while for your package to be indexed and appear in search results. While that happens, you will see the following message on your package page.

![api key](/images/CreatePublishNugetSample/04.PNG)
