#Build for UWP

Windows 10 introduces the Universal Windows Platform (UWP), which further evolves the Windows Runtime model and brings it into the Windows 10 unified core. As part of the core, the UWP now provides a common app platform available on every device that runs Windows 10. With this evolution, apps that target the UWP can call not only the WinRT APIs that are common to all devices, but also APIs (including Win32 and .NET APIs) that are specific to the device family the app is running on.

[Learn more about the Universal Windows Platform](https://developer.microsoft.com/en-us/windows)

##Pre-requisites
1. Visual Studio 2015 Update 3 with Windows developer tools. If you don't have Visual Studio already, you can download [Visual Studio Community 2015](https://developer.microsoft.com/en-us/windows/downloads) for free. 
2. NuGet CLI - Download the latest version of nuget.exe from [nuget.org/downloads](https://nuget.org/downloads), move it to a common location and add this path to the PATH Environment Variable. For more details, take a look at [The NuGet Install guide](/ndocs/guides/install-nuget#nuget-cli)

##What are we building
We will create a native UWP component that can be used in Managed and Native projects. In addition, we will also show you how to author and package XAML controls in NuGet packages.

##Create new Project

1. In Visual Studio, choose File, New, Project. In the New Project dialog, from Visual C++ -> Windows, choose Windows Runtime Component (Universal Windows). Change the name to ImageEnhancer and click ok.

	![Create new Project](/images/BuildForUWP/01.PNG)

2. You may accept the default values for and click ok.

3. From the context menu of the project, select Add->New Item. In the Add New Item dialog, select XAML under the Visual C++ node and then select Templated Control. Change the name to AwesomeImageControl.cpp and click Add.
	
	![Add New Item](/images/BuildForUWP/02.PNG)

4. From the context menu of the project, select properties. In the Property Pages dialog, expand Configuration Properties, expand C/C++ and click on Output Files. In the pane on the right, change the value for Generate XML Documentation Files to Yes.

	![Generate XML Documentation Files](/images/BuildForUWP/03.PNG)

5. From the context menu of the solution, select Batch Build. In the Batch Build dialog, check the three debug boxes, then click Build.

	![Batch Build](/images/BuildForUWP/04.PNG)

##Create the .nuspec file

Bring up the console and navigate to the project folder. This path will look something like this
	`C:\Users\username\Documents\Visual Studio 2015\Projects\ImageEnhancer\ImageEnhancer`

Then run the `spec` command

<code class="bash hljs">
	nuget spec
</code>

This will generate a new file `ImageEnhancer.nuspec`. Open this file. Here is how the nuspec files looks.

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

###Adding nuspec metadata
Update the metadata for the package. The updated nuspec should look like below.

	<?xml version="1.0"?>
	<package >
	  <metadata>
		<id>ImageEnhancer</id>
		<version>1.0.0</version>
		<title>ImageEnhancer</title>
		<authors>karann</authors>
		<owners>karann</owners>
		<licenseUrl>http://LICENSE_URL_HERE_OR_DELETE_THIS_LINE</licenseUrl>
		<projectUrl>http://PROJECT_URL_HERE_OR_DELETE_THIS_LINE</projectUrl>
		<iconUrl>http://ICON_URL_HERE_OR_DELETE_THIS_LINE</iconUrl>
		<requireLicenseAcceptance>false</requireLicenseAcceptance>
		<description>Awesome Image Enhancer</description>
		<releaseNotes>First Release</releaseNotes>
		<copyright>Copyright 2016</copyright>
		<tags>image enhancer</tags>
	  </metadata>
	</package>

**Recommended Reading:** [Nuspec Reference](/ndocs/schema/nuspec)

###Adding a winmd to the package.
[WinMd]() is a metadata file that describes the shape of all publicly available types. This is required in order to consume these types in other UWP libs or apps. In addition, you can also add xml files to enable IntelliSense.

	<?xml version="1.0"?>
	<package >
		<metadata>...
		</metadata>
		<files>

		<!--Adding the WinMd and XML -->
		<file src="..\Debug\ImageEnhancer\ImageEnhancer.winmd" target="lib\uap10.0"/>
		<file src="..\Debug\ImageEnhancer\ImageEnhancer.xml" target="lib\uap10.0"/>

		</files>
	</package>


###Adding XAML content
For the consuming project to use XAML controls in this library, you need to add the XAML file that has the deafult template for the control.

	<?xml version="1.0"?>
	<package >
		<metadata>...
		</metadata>
		<files>
		<file src="..\Debug\ImageEnhancer\ImageEnhancer.winmd" target="lib\uap10.0"/>
		<file src="..\Debug\ImageEnhancer\ImageEnhancer.xml" target="lib\uap10.0"/>

		<!--Adding the XAML-->
		<file src="Themes\Generic.xaml" target="lib\uap10.0\Themes"/>

		</files>
	</package>

###Adding the native implementation libraries
The core logic of the type is in native code and this is contained in the implementation (ImageEnhancer.dll) assembly. Since implementation assembly is per target Runtime, we have to make sure the package includes implementation assemblies for all available runtimes.

In addition, pri files are the generated artifacts that contain the resources in your project. You need to add these to the package as well.

	<?xml version="1.0"?>
	<package >
		<metadata>...
		</metadata>
		<files>
		<file src="..\Debug\ImageEnhancer\ImageEnhancer.winmd" target="lib\uap10.0"/>
		<file src="..\Debug\ImageEnhancer\ImageEnhancer.xml" target="lib\uap10.0"/>
		<file src="Themes\Generic.xaml" target="lib\uap10.0\Themes"/>

		<!--Adding the dll and pri-->
		<file src="..\ARM\Debug\ImageEnhancer\ImageEnhancer.dll" target="runtimes\win10-arm\native"/>
		<file src="..\ARM\Debug\ImageEnhancer\ImageEnhancer.pri" target="runtimes\win10-arm\native"/>
		<file src="..\x64\Debug\ImageEnhancer\ImageEnhancer.dll" target="runtimes\win10-x64\native"/>
		<file src="..\x64\Debug\ImageEnhancer\ImageEnhancer.pri" target="runtimes\win10-x64\native"/>
		<file src="..\Debug\ImageEnhancer\ImageEnhancer.dll" target="runtimes\win10-x86\native"/>
		<file src="..\Debug\ImageEnhancer\ImageEnhancer.pri" target="runtimes\win10-x86\native"/>

		</files>
	</package>

###Adding .targets
For C++ projects, we need to provide information about the reference and implementation assemblies. This information goes in the .targets file that we will create and pack with the nupkg.
Here is the .targets file that you need to use with this sample. Copy the following into a text file and rename it to ImageEnhancer.targets. Note that you must change the extension of the file from .txt to .targets. Place this file in the same folder as the nuspec.

	<?xml version="1.0" encoding="utf-8"?>
	<Project xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
		<PropertyGroup>
			<ImageEnhancer-Platform Condition="'$(Platform)' == 'Win32'">x86</ImageEnhancer-Platform>
			<ImageEnhancer-Platform Condition="'$(Platform)' != 'Win32'">$(Platform)</ImageEnhancer-Platform>
		</PropertyGroup>
		<ItemGroup Condition="'$(TargetPlatformIdentifier)' == 'UAP'">
			<Reference Include="$(MSBuildThisFileDirectory)..\..\lib\uap10.0\ImageEnhancer.winmd">
				<Implementation>ImageEnhancer.dll</Implementation>
			</Reference>
		<ReferenceCopyLocalPaths Include="$(MSBuildThisFileDirectory)..\..\runtimes\win10-$(ImageEnhancer-Platform)\native\ImageEnhancer.dll" />
		</ItemGroup>
	</Project>

Modify the nuspec file

	<?xml version="1.0"?>
	<package >
		<metadata>...
		</metadata>
		<files>
		<file src="..\Debug\ImageEnhancer\ImageEnhancer.winmd" target="lib\uap10.0"/>
		<file src="..\Debug\ImageEnhancer\ImageEnhancer.xml" target="lib\uap10.0"/>
		<file src="Themes\Generic.xaml" target="lib\uap10.0\Themes"/>
		<file src="..\ARM\Debug\ImageEnhancer\ImageEnhancer.dll" target="runtimes\win10-arm\native"/>
		<file src="..\ARM\Debug\ImageEnhancer\ImageEnhancer.pri" target="runtimes\win10-arm\native"/>
		<file src="..\x64\Debug\ImageEnhancer\ImageEnhancer.dll" target="runtimes\win10-x64\native"/>
		<file src="..\x64\Debug\ImageEnhancer\ImageEnhancer.pri" target="runtimes\win10-x64\native"/>
		<file src="..\Debug\ImageEnhancer\ImageEnhancer.dll" target="runtimes\win10-x86\native"/>
		<file src="..\Debug\ImageEnhancer\ImageEnhancer.pri" target="runtimes\win10-x86\native"/>

		<!--Adding .targets-->
		<file src="ImageEnhancer.targets" target="build\native"/>

		</files>
	</package>

###Final nuspec
The final nuspec looks something like below with the WinMd, XAML controls and the native implementation libraries. You can now pack it.

	<?xml version="1.0"?>
	<package >
	  <metadata>
		<id>ImageEnhancer</id>
		<version>1.0.0</version>
		<title>ImageEnhancer</title>
		<authors>karann</authors>
		<owners>karann</owners>
		<licenseUrl>http://LICENSE_URL_HERE_OR_DELETE_THIS_LINE</licenseUrl>
		<projectUrl>http://PROJECT_URL_HERE_OR_DELETE_THIS_LINE</projectUrl>
		<iconUrl>http://ICON_URL_HERE_OR_DELETE_THIS_LINE</iconUrl>
		<requireLicenseAcceptance>false</requireLicenseAcceptance>
		<description>Awesome Image Enhancer</description>
		<releaseNotes>First Release</releaseNotes>
		<copyright>Copyright 2016</copyright>
		<tags>image enhancer</tags>
	  </metadata>
	  <files>
		<file src="..\Debug\ImageEnhancer\ImageEnhancer.winmd" target="lib\uap10.0"/>
		<file src="..\Debug\ImageEnhancer\ImageEnhancer.xml" target="lib\uap10.0"/>
		<file src="Themes\Generic.xaml" target="lib\uap10.0\Themes"/>
		<file src="..\ARM\Debug\ImageEnhancer\ImageEnhancer.dll" target="runtimes\win10-arm\native"/>
		<file src="..\ARM\Debug\ImageEnhancer\ImageEnhancer.pri" target="runtimes\win10-arm\native"/>
		<file src="..\x64\Debug\ImageEnhancer\ImageEnhancer.dll" target="runtimes\win10-x64\native"/>
		<file src="..\x64\Debug\ImageEnhancer\ImageEnhancer.pri" target="runtimes\win10-x64\native"/>
		<file src="..\Debug\ImageEnhancer\ImageEnhancer.dll" target="runtimes\win10-x86\native"/>
		<file src="..\Debug\ImageEnhancer\ImageEnhancer.pri" target="runtimes\win10-x86\native"/>
		<file src="ImageEnhancer.targets" target="build\native"/>
	  </files>
	</package>


##Pack
Now run the `pack` command

<code class="bash hljs">
	nuget pack ImageEnhancer.nuspec
</code>

This will generate a new file `ImageEnhancer.1.0.0.nupkg`. Open this file. The contents should look something like

![nupkg](/images/BuildForUWP/05.PNG)

##Advanced scenarios
* [MSBuild targets and props](/ndocs/create-packages/create-a-package#import-msbuild-targets-and-props-files-into-project)
* [Symbol packages](/ndocs/create-packages/symbol-packages)
* [Creating Localized Packages](/ndocs/create-packages/creating-localized-packages)