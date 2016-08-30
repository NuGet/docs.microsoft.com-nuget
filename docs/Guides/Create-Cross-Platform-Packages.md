#Create Cross Platform Packages

This guide will walk you through creating a cross platform library that targets iOS, Android and Windows. The scenario we are trying to complete here is to build a logging library that uses native APIs on each platform. This is pretty easy to do but in order to share more code, it is pertinent that we enable users to consume this API surface from PCL or NET Standard libraries. To do this, we will exercise a technique which is popularly known as bait and switch. A primer for bait and switch is available [here](http://log.paulbetts.org/the-bait-and-switch-pcl-trick/) 

##Pre-requisites
1. [Visual Studio 2015 Update 3](https://www.visualstudio.com/news/releasenotes/vs2015-update3-vs). If you don't have Visual Studio already, you can download [Visual Studio Community 2015](https://www.visualstudio.com/downloads/download-visual-studio-vs) for free.
2. UWP and Xamarin tools need to be installed when you install Visual Studio.
3. NuGet CLI - Download the latest version of nuget.exe from [nuget.org/downloads](https://nuget.org/downloads), move it to a common location and add this path to the PATH Environment Variable. For more details, take a look at [The NuGet Install guide](/ndocs/guides/install-nuget#nuget-cli)

##Creating the right project structure
First, you need to create the following projects in your solution:

1. LoggingLibrary: this project should be created using the class library template for iOS, Android and UWP in Visual Studio new project dialog
2. LoggingLibrary.Android: created using Xamarin Android Library project
3. LoggingLibrary.iOS: created using Xamarin iOS Library project
4. LoggingLibrary.UWP: created using UWP class Library project

This should look something like this:
![Project Explorer](/images/BuildForXplat/01.PNG)

###Update Project Properties
Repeat these steps for each of the four projects.

1. From the context menu, click on properties.
2. Update the assembly name and Default namespace:
	
	a. For portable and UWP project, click on the Library tab on the left. In the right pane, update the Assembly name and Default namespace to LoggingLibrary
	
	![assembly name and Default namespace uwp](/images/BuildForXplat/02.PNG)

	b. For Android and iOS project, click on the Application tab on the left. In the right pane, update the Assembly name and Default namespace to LoggingLibrary

	![assembly name and Default namespace ios](/images/BuildForXplat/02_1.PNG)

3. Click on Build tab on the left. In the right pane, change the Configuration to Release and check the box for XML documentation file
	
	![XML documentation file](/images/BuildForXplat/03.PNG)

4. Save (ctrl + s)


Alternately, you can use the [Xamarin plugin extension by James Montemagno](https://visualstudiogallery.msdn.microsoft.com/afead421-3fbf-489a-a4e8-4a244ecdbb1e). This will create the necessary project template structure and the nuspec template for you.

##Creating the stubbed out PCL
Since we want to reference this library in a PCL or NET Standard project, we have to make sure that we provide a stubbed out reference assembly for compilation. At runtime, the platform specific version is loaded and executed.

###Example
This is how your stubbed out PCL surface area should look like

	using System;

    namespace LoggingLibrary
    {
        /// <summary>
        /// Stubbed out class and methods
        /// </summary>
        public class LoggingLibrary
        {
                public void Log (string text)
                {
                    throw new NotImplementedException();
                }    
        }
    }

##Writing platform specific code
Writing the platform specific implementation of the `LoggingLibrary` class and its methods is left as an exercise for the reader. The native implementations should have the same signatures as the stubbed out PCL. Once you have written all the code, build the entire solution in for the release configuration.

##Create the .nuspec file
Bring up a Windows command prompt (e.g. by using Windows + X and choosing Command Prompt). Then run the `spec` command (ensure you've added nuget.exe to the PATH as discussed in Prerequisites above)

<code class="bash hljs">
	nuget spec
</code>

This will generate a new file `Package.nuspec`.  Rename it to `LoggingLibrary.nuspec`. Open this file. It will look something like:

	<?xml version="1.0"?>
	<package >
		<metadata>
			<id>Package</id>
			<version>1.0.0</version>
			<authors>karann</authors>
			<owners>karann</owners>
			<licenseUrl>http://LICENSE_URL_HERE_OR_DELETE_THIS_LINE</licenseUrl>
			<projectUrl>http://PROJECT_URL_HERE_OR_DELETE_THIS_LINE</projectUrl>
			<iconUrl>http://ICON_URL_HERE_OR_DELETE_THIS_LINE</iconUrl>
			<requireLicenseAcceptance>false</requireLicenseAcceptance>
			<description>Package description</description>
			<releaseNotes>Summary of changes made in this release of the package.</releaseNotes>
			<copyright>Copyright 2016</copyright>
			<tags>Tag1 Tag2</tags>
			<dependencies>
				<dependency id="SampleDependency" version="1.0" />
			</dependencies>
		</metadata>
	</package>

Update the nuspec to have all the relevant metadata. For more details read the [nuspec reference](/ndocs/schema/nuspec)

Here is how the updated nuspec file looks:

	<?xml version="1.0"?>
	<package >
		<metadata>
			<id>LoggingLibrary</id>
			<version>1.0.0</version>
			<authors>karann</authors>
			<owners>karann</owners>
			<licenseUrl>http://LICENSE_URL_HERE_OR_DELETE_THIS_LINE</licenseUrl>
			<projectUrl>http://PROJECT_URL_HERE_OR_DELETE_THIS_LINE</projectUrl>
			<iconUrl>http://ICON_URL_HERE_OR_DELETE_THIS_LINE</iconUrl>
			<requireLicenseAcceptance>false</requireLicenseAcceptance>
			<description>Awesome application logging utility</description>
			<releaseNotes>First release</releaseNotes>
			<copyright>Copyright 2016 (c) Contoso Corporation. All rights reserved.</copyright>
			<tags>application app logger logging logs</tags>
		</metadata>
	</package>

Especially for packages that are built for public consumption, it is a good practice to update the metadata tags making it easier for others to find the package and understand what it does and how to use it.

<div class="block-callout-warning">
	<strong>Note</strong><br>
	You must select a package ID that is unique across nuget.org. We recommend using the naming conventions described <a href="/ndocs/create-packages/package-best-practices">here</a>. You must also update the author and description tags or you will get an error in the next step.
</div>

##Adding reference assemblies
In order to pack reference assemblies, you need add the following to the files element in your nuspec.

	<!--Reference Assemblies-->
	<file src="LoggingLibrary\bin\Release\LoggingLibrary.dll" target="lib\netstandard1.4\LoggingLibrary.dll" />
	<file src="LoggingLibrary\bin\Release\LoggingLibrary.xml" target="lib\netstandard1.4\LoggingLibrary.xml" />
			
##Adding assemblies for iOS
To pack iOS assemblies, you need add the following to the files element in your nuspec.

	<!--Xamarin.iOS-->
	<file src="LoggingLibrary.iOS\bin\Release\LoggingLibrary.dll" target="lib\Xamarin.iOS10\LoggingLibrary.dll" />
	<file src="LoggingLibrary.iOS\bin\Release\LoggingLibrary.xml" target="lib\Xamarin.iOS10\LoggingLibrary.xml" />
			

##Adding assemblies for Android
To pack Android assemblies, you need add the following to the files element in your nuspec.

	<!--Xamarin.Android-->
	<file src="LoggingLibrary.Android\bin\Release\LoggingLibrary.dll" target="lib\MonoAndroid10\LoggingLibrary.dll" />
	<file src="LoggingLibrary.Android\bin\Release\LoggingLibrary.xml" target="lib\MonoAndroid10\LoggingLibrary.xml" />
				

##Adding assemblies for UWP
To pack UWP assemblies, you need add the following to the files element in your nuspec.

	<!--uap-->
	<file src="LoggingLibrary.UWP\bin\Release\LoggingLibrary.dll" target="lib\UAP10\LoggingLibrary.dll" />
	<file src="LoggingLibrary.UWP\bin\Release\LoggingLibrary.xml" target="lib\UAP10\LoggingLibrary.xml" />
				

##Adding dependencies
If you want to have specific dependencies for native implementations, you can use the following dependencies element with group elements to specify these dependencies.

	<dependencies>
		<group targetFramework="MonoAndroid">
		<!--MonoAndroid dependencies go here-->
		</group>
		<group targetFramework="Xamarin.iOS10">
		<!--Xamarin.iOS10 dependencies go here-->
		</group>
		<group targetFramework="uap">
		<!--uap dependencies go here-->
		</group>
	</dependencies>

###Example
In the following example we have added iTextSharp to UAP

	<dependencies>
		<group targetFramework="uap">
			<dependency id="iTextSharp" version="5.5.9" />
		</group>
	</dependencies>

##Final nuspec
The final nuspec will look something like:

	<?xml version="1.0"?>
	<package >
		<metadata>
			<id>App</id>
			<version>1.0.0</version>
			<authors>karann</authors>
			<owners>karann</owners>
			<licenseUrl>http://LICENSE_URL_HERE_OR_DELETE_THIS_LINE</licenseUrl>
			<projectUrl>http://PROJECT_URL_HERE_OR_DELETE_THIS_LINE</projectUrl>
			<iconUrl>http://ICON_URL_HERE_OR_DELETE_THIS_LINE</iconUrl>
			<requireLicenseAcceptance>false</requireLicenseAcceptance>
			<description>Awesome application logging utility</description>
			<releaseNotes>First release</releaseNotes>
			<copyright>Copyright 2016 (c) Contoso Corporation. All rights reserved.</copyright>
			<tags>application app logger logging logs</tags>
			<dependencies>
				<group targetFramework="MonoAndroid">
				</group>
				<group targetFramework="Xamarin.iOS10">
				</group>
				<group targetFramework="uap">
					<dependency id="iTextSharp" version="5.5.9" />
				</group>
			</dependencies>
		</metadata>
		<files>
			<!--Reference Assemblies-->
			<file src="LoggingLibrary\bin\Release\LoggingLibrary.dll" target="lib\netstandard1.4\LoggingLibrary.dll" />
			<file src="LoggingLibrary\bin\Release\LoggingLibrary.xml" target="lib\netstandard1.4\LoggingLibrary.xml" />
			<!--Xamarin.iOS-->
			<file src="LoggingLibrary.iOS\bin\Release\LoggingLibrary.dll" target="lib\Xamarin.iOS10\LoggingLibrary.dll" />
			<file src="LoggingLibrary.iOS\bin\Release\LoggingLibrary.xml" target="lib\Xamarin.iOS10\LoggingLibrary.xml" />
			<!--Xamarin.Android-->
			<file src="LoggingLibrary.Android\bin\Release\LoggingLibrary.dll" target="lib\MonoAndroid10\LoggingLibrary.dll" />
			<file src="LoggingLibrary.Android\bin\Release\LoggingLibrary.xml" target="lib\MonoAndroid10\LoggingLibrary.xml" />
			<!--uap-->
			<file src="LoggingLibrary.UWP\bin\Release\LoggingLibrary.dll" target="lib\UAP10\LoggingLibrary.dll" />
			<file src="LoggingLibrary.UWP\bin\Release\LoggingLibrary.xml" target="lib\UAP10\LoggingLibrary.xml" />
		</files>
	</package>



##Pack
Now run the `pack` command

<code class="bash hljs">
	nuget pack LoggingLibrary.nuspec
</code>

This will generate a new file `LoggingLibrary.1.0.0.nupkg`. Open this file. The contents should look something like

![nupkg](/images/BuildForXplat/04.PNG)
