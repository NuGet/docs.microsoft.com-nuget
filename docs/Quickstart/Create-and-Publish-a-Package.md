#Create and Publish a Package

There are a few ways to create a NuGet package. This tutorial walks you through creating a NuGet package from a project using the nuget CLI and [Visual Studio 2015](https://www.visualstudio.com/en-us/visual-studio-homepage-vs.aspx) and publishing it to nuget.org.

##Pre-requisites
1. Visual Studio 2015 - If you don't have Visual Studio installed, you can download [Visual Studio Community 2015](https://developer.microsoft.com/en-us/windows/downloads) for free.
2. NuGet CLI - Download the latest version of nuget.exe from [nuget.org/downloads](https://nuget.org/downloads), move it to a location of your choice, and add the path to nuget.exe to the PATH Environment Variable. For more details, take a look at [The NuGet Install Guide](/ndocs/guides/install-nuget#nuget-cli).

##Create a New Class Library Project

In Visual Studio, choose **File**, **New**, **Project**. In the **New Project** dialog, expand the **Visual C#** node and choose the **Windows** node, and then choose **Class Library**. Change the name to AppLogger.

![Create new Project](/images/CreatePublishNugetSample/01.PNG)

After you are done making changes to the code in this project, in the **Build** menu, choose **Build Solution**. Make sure the solution builds successfully.

##Create the .nuspec File

Bring up the command prompt and navigate to the folder containing the `.csproj` file for the project you just created. This path will look something like this:
	`C:\Users\username\Documents\Visual Studio 2015\Projects\AppLogger\AppLogger`

Run the <code>spec</code> command:

<code class="bash hljs">
	nuget spec
</code>

This will generate a new file `AppLogger.nuspec` in the same directory as the `.csproj` file for the project.

Open this file with notepad or your favorite text editor. It will look something like this:

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

This file includes tokens that are meant to be replaced at pack time, based on the project metadata stored in AssemblyInfo.cs (this file can be found by expanding the properties node in Visual Studio's Solution Explorer).
To know more about how tokens are handled, read [Creating a nuspec file.](/ndocs/create-packages/create-a-package#create-a--nuspec-file)


<div class="block-callout-warning">
	<strong>Note</strong><br>
	You must select a package ID that is unique across nuget.org. We recommend using the naming conventions described <a href="/ndocs/create-packages/package-best-practices">here</a>. You must also update the author and description tags or you will get an error in the next step.
</div>

Here is how the updated nuspec file looks:

	<?xml version="1.0"?>
	<package >
	  <metadata>
		<id>MyCompanyName.MyProductName.MyPackageName</id>
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


You should consider updating the metadata tags to make it easier for others to find the package, understand what it does, and how to use it.

Having finalized the nuspec file, we are now ready to create the nuget package.

##Pack

Now run the <code>pack</code> command on the project:

	nuget pack AppLogger.csproj

You will get warnings if you haven't updated the release notes and tags from their default values.

When the command has completed successfully, it will generate a new file `AppLogger.1.0.0.0.nupkg`. This is your nuget package.


##Publish
There are two ways you can publish packages: using the [nuget CLI](/ndocs/tools/nuget-cli-reference) (which is what we will be doing here), or using the [nuget.org publishing workflow](/ndocs/create-packages/publish-a-package#publish-through-nuget-org). No matter which approach you use, you will need to have an account with [nuget.org](https://www.nuget.org/).

<div class="block-callout-warning">
	<strong>Note</strong><br>
	The packages you publish to <a href="https://www.nuget.org/">nuget.org</a> will be available for the rest of the world to consume. Other options for hosting packages can be found <a href="/ndocs/host-packages/hosting-packages-overview">here</a>.
</div>

Go to [nuget.org](https://www.nuget.org/) to register for an account, or login if you already have one. Creating an account is easy and free.

Click on your user name to navigate to your account settings. You can see the API Key that was generated for you in the Credentials section.

![api key](/images/CreatePublishNugetSample/03.PNG)

<div class="block-callout-warning">
	<strong>Note</strong><br>
	Always keep your API key a secret! If your key is accidentally revealed, you can always regenerate it at any time. You can also remove the API key if you no longer want to push packages via the command prompt.
</div>

Open your command prompt and run the following command. Replace the key below with the key that was generated for you.

<code class="bash hljs">
	nuget push AppLogger.1.0.0.0.nupkg 47be3377-c434-4c29-8576-af7f6993a54b -Source https://www.nuget.org/api/v2/package
</code>

You should see something like this when the command has successfully executed.

	Pushing AppLogger.1.0.0.0.nupkg to 'https://www.nuget.org/api/v2/package'...
	  PUT https://www.nuget.org/api/v2/package/
	  Created https://www.nuget.org/api/v2/package/ 6829ms
	Your package was pushed.

You can now go to your account on nuget.org and under <b>Manage my packages</b>, you should be able to see the package that you just published. You should also receive an email notifying you that the package was published.

It might take a while for your package to be indexed and appear in search results, so you or other uses could consume this package. While that happens, you will see the following message on your package page.

![api key](/images/CreatePublishNugetSample/04.PNG)

That's it! You have just created and published your first NuGet package to [nuget.org](https://www.nuget.org/) for the rest of the world to consume.

##Related Reading
* [Create a Package](/ndocs/create-packages/create-a-package)
* [Publish a Package](/ndocs/create-packages/publish-a-package)
* [Support multiple target frameworks](/ndocs/create-packages/supporting-multiple-target-frameworks)
* [Dependency versions](/ndocs/create-packages/dependency-versions)
* [Creating localized packages](/ndocs/create-packages/creating-localized-packages)




