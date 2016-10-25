#Create and publish a package

It's a simple process to create a NuGet package from a .NET Class Library and publish it to nuget.org. The following steps walk you through the process using the NuGet command-line interface (CLI) and Visual Studio:

- [Install pre-requisites](#install-pre-requisites)
- [Create a class library project](#create-a-class-library-project)
- [Create the .nuspec package manifest file](#create-the--nuspec-package-manifest-file)
- [Create the package](#create-the-package)
- [Publish the package](#publish-the-package)

##Install pre-requisites
1. Visual Studio 2015. Install the Community edition for free from [visualstudio.com](https://www.visualstudio.com/); you can use the Professional and Enterprise editions as well, of course.
2. NuGet CLI. Download the latest version of nuget.exe from [nuget.org/downloads](https://nuget.org/downloads), saving it to a location of your choice. Then add that location to your PATH environment variable if it isn't already.

<div class="block-callout-info">
	<strong>Note</strong><br>
	nuget.exe is the CLI tool itself, not an installer, so be sure to save the downloaded file from your browser instead of running it.
</div>

##Create a class library project

In Visual Studio, choose **File > New > Project**, expand the **Visual C# > Windows** node, select the "Class Library" template, name the project AppLogger, and click OK.

![Create new class library project](/images/CreatePublishNugetSample/QS_Create-01-NewProject.png)

Right click on the resulting project file and select **Build** to make sure the project was created properly.

Within a real NuGet package, of course, you'll implement many useful features upon which others can build applications. For this walkthrough, however, you won't add any additional code because a class library from the template is sufficient to create a package. 

##Create the .nuspec package manifest file

Every NuGet package needs a manifest–a .nuspec file–to describe its contents and its dependencies. The NuGet CLI will create this file for you, which you then customize.

1. Open a command prompt and navigate to the folder containing the AppLogger project file (.csproj).
2. Run the NuGet CLI <code>spec</code> command to generate `AppLogg.nuspec`:

	<code class="bash hljs">
		nuget spec
	</code>

3. Open the file in your favorite text editor. It will look something like the code below, where tokens in the form *$&lt;token&gt;$* will be replaced during the packaging process with values from the project's Properties/AssemblyInfo.cs file. For more details on tokens, see [Creating a .nuspec file](/ndocs/create-packages/creating-a-package#creating-the--nuspec-file).
			
		<?xml version="1.0"?>
		<package>
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

4. Select a package ID that is unique across nuget.org. We recommend using the naming conventions described in [Creating a package](/ndocs/create-packages/creating-a-package#choosing-a-unique-package-identifier-and-setting-the-version-number). You must also update the author and description tags or you will get an error in the next step. Here's an updated .nuspec file as an example:
 		
		<?xml version="1.0"?>
		<package>
		  <metadata>
			<id>MyCompanyName.MyProductName.MyPackageName</id>
			<version>$version$</version>
			<title>$title$</title>
			<authors>kraigb</authors>
			<owners>kraigb</owners>
			<requireLicenseAcceptance>false</requireLicenseAcceptance>
			<description>Awesome application logging utility</description>
			<releaseNotes>First release</releaseNotes>
			<copyright>Copyright 2016</copyright>
			<tags>application app logger logging logs</tags>
		  </metadata>
		</package>

<div class="block-callout-info">
	<strong>Note</strong><br>
	For packages built for public consumption, pay special attention to the <em>&lt;tags&gt;</em> element, as these tags help others find your package and understand what it does.
</div>


##Create the package

Creating a NuGet package from a project is simple: just run the <code>pack</code> command:

<code class="bash hljs">
	nuget pack AppLogger.csproj
</code>

This will create a NuGet package file like `AppLogger.1.0.0.0.nupkg` using, of course, the package name and version number from the .nuspec file. 
	
Note that you'll get warnings if you haven't updated various fields in the .nuspec file from from their default values.


##Publish the package

You're now ready to publish the package to nuget.org using the NuGet CLI. (Alternately, you can use the [nuget.org publishing workflow](/ndocs/create-packages/publish-a-package#publish-through-nuget-org).

<div class="block-callout-warning">
	<strong>Note</strong><br>
	The packages you publish to <a href="https://www.nuget.org/">nuget.org</a> will be publicly visible to other developers. To host packages privately, see <a href="/ndocs/hosting-packages/overview">Hosting packages</a>.
</div>

1. Create a free account on [nuget.org](https://www.nuget.org/users/account/LogOn?returnUrl=%2F), or log in if you already have one. When creating a new account, you'll receive a confirmation email. You must confirm the account before you can upload a package.
2. Once logged in, click your user name (on the upper right) to navigate to your account settings.
3. Under **API Key**, click **copy to clipboard** to retrieve the access key you'll need in the CLI:   

	![Copying the API key to the clipboard](/images/CreatePublishNugetSample/QS_Create-02-APIKey.png)
	
	<div class="block-callout-warning">
		<strong>Note</strong><br>
		Always keep your API key a secret! If your key is accidentally revealed, you can always regenerate it at any time. You can also remove the API key if you no longer want to push packages via the CLI.
	</div>

4. At a command prompt, run the following command, replacing the key with the value copied in step 3:

	<code class="bash hljs">
		nuget push AppLogger.1.0.0.0.nupkg 47be3377-c434-4c29-8576-af7f6993a54b -Source https://www.nuget.org/api/v2/package
	</code>

5. You should then see something like the following:

	<code class="bash hljs">
		Pushing AppLogger.1.0.0.0.nupkg to 'https://www.nuget.org/api/v2/package'...
		  PUT https://www.nuget.org/api/v2/package/
		  Created https://www.nuget.org/api/v2/package/ 6829ms
		Your package was pushed.
	</code>
	
6. In your account on nuget.org, click **Manage my packages** to see the one that you just published; you'll also receive a confirmation email. Note that it might take a while for your package to be indexed and appear in search results where others can find it, during which time you'll see the following message on your package page: 

	![This package has not been indexed yet. It will appear in search results and will be available for install/restore after indexing is complete.](/images/CreatePublishNugetSample/04.PNG)


<div class="block-callout-info">
	<strong>Virus Scanning</strong><br>
	Before being made public, all packages uploaded to nuget.org are scanned for viruses and rejected if any viruses are found. All packages listed on nuget.org are also scanned periodically.
</div> 

And that's it! You've just created and published your first NuGet package to [nuget.org](https://www.nuget.org/), that other developers can use in their own projects.

##Related topics
* [Create a Package](/ndocs/create-packages/creating-a-package)
* [Publish a Package](/ndocs/create-packages/publish-a-package)
* [Support multiple target frameworks](/ndocs/create-packages/supporting-multiple-target-frameworks)
* [Dependency versions](/ndocs/create-packages/dependency-versions)
* [Creating localized packages](/ndocs/create-packages/creating-localized-packages)




