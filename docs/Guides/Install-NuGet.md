#The NuGet Install Guide

The following tools will help you build, publish and consume nuget packages.

1. [NuGet CLI](#nuget-cli)
2. [NuGet Package Manager Extension in Visual Studio](#nuget-package-manager-extension-in-visual-studio)
3. [NuGet in .NET CLI](#nuget-in--net-cli)
4. [Package Explorer](#package-explorer)


##NuGet CLI
This utility can be used to create, publish, and download packages. It also works on macOS and linux on top of mono. [Get an overview of nuget compatibility on mono](#nuget-cli-os-compatibility).

The NuGet CLI can be installed in a few possible ways.

###Download from nuget.org
1. Download the latest version of nuget.exe from [nuget.org/downloads](https://nuget.org/downloads) OR Install the [NuGet.CommandLine](http://www.nuget.org/packages/NuGet.CommandLine/) package from the NuGet Visual Studio client.
2. Move the nuget.exe to a common location and add this path to the PATH Environment Variable OR execute it in the context of your project.

<div class="block-callout-info">
	<strong>NuGet 2.x users</strong><br>
    Because there are a few breaking changes introduced in NuGet 3.2 <a href="https://nuget.org/nuget.exe">https://nuget.org/nuget.exe</a> points to the latest stable NuGet 2.x release to prevent CI systems from potentially breaking at this time.
</div>

###Install from Chocolatey
Alternatively, Install the [NuGet.CommandLine](http://chocolatey.org/packages/NuGet.CommandLine) Chocolatey package using the [Chocolatey](http://chocolatey.org) client.

<code class="bash hljs">
	choco install nuget.commandline
</code>
	

**Recommended Reading:**  [NuGet CLI commands](/ndocs/tools/nuget.exe-cli-reference), [Creating a package](/ndocs/create-packages/create-a-package), [Publishing a Package](/ndocs/create-packages/publish-a-package)


## NuGet Package Manager Extension in Visual Studio
Starting with Visual Studio 2012, NuGet is included in every edition (except Team Foundation Server). This extension includes the package manager UI and the package manager console. If Visual Studio does not already have a specific version of the NuGet Package Manager extension, you can install it using the Extension Manager.<br>

1. In Visual Studio, click Tools and then Extension and Updates.
2. Navigate to Online, search for "NuGet Package Manager for Visual Studio" and click Download.
3. In the Installer dialog box, click Install.
4. When installation is complete, close and re-open Visual Studio.


### Package Manager UI
The package manager UI allows you to find, install, remove, and update NuGet packages.

**Recommended Reading**: [Managing NuGet Packages Using the UI](/ndocs/tools/package-manager-ui)


###Package Manager Console
The package manager console allows you to find, install, remove, and update NuGet packages using PowerShell commands. Some packages set up tools during install which can be accessed through the package manager console.

<div class="block-callout-warning">
	<strong>Note</strong><br>
    Package Manager Console commands do not work outside of visual studio since it is dependent on VS objects like DTE to work correctly.
</div>

The NuGet Package Manager Console requires that [PowerShell 2.0](http://support.microsoft.com/kb/968929) be installed. Powershell 2.0 is already installed if you have Windows 7 (or higher) or Windows Server 2008 R2 (or higher).

**Recommended Reading**: [Managing Packages Using the Package Manager Console](/ndocs/tools/package-manager-console), [Package Manager Console Powershell Commands](/ndocs/tools/powershell-reference)

### Updating the NuGet Extension in Visual Studio
You can update NuGet using the Visual Studio Extension Manager. Navigate to the Extension Manager and click on the Updates tab to check for updates. If there is a new version of NuGet you will see it in the list of available updates. If you are using Visual Studio 2015 Update 2 or higher, the NuGet extension is auto updated every time a new stable release is made available.

## NuGet Beta Channel
NuGet Beta Channel for the Visual Studio 2015 NuGet Package Manager gives you access to Beta releases. Though called a Beta release, we will only make available high quality builds that are close to being complete on this channel. We want to use this feedback mechanism to make sure that our near-final builds do not introduce any inadvertent issues.

<div class="block-callout-info">
	If you run into any issues while dogfooding the Beta build or have an idea you would like to share with us, <a href="https://github.com/Nuget/Home">open an issue on GitHub</a>.
</div>

The Beta channel is recommended for the following users:

1. You like to stay on the latest and greatest releases.
2. You want to try out new features.
3. You are experiencing a blocking issue and want to get access to the build with the fix before it hits RTM.

### Getting Access
You can get access to the Beta builds by following the steps outlined below.

1. Add the Beta Feed: `https://dotnet.myget.org/F/nuget-beta/vsix/` to the Additional Extension Galleries list in Tools->Options->Environment->Extensions and Updates.
2. Navigate to Tools->Extensions and Updates< and select Online. You should now be able to see the NuGet-Beta Feed. Install the NuGet Package Manager Extension.

##NuGet in .NET CLI

The [.NET Core CLI](https://docs.microsoft.com/en-us/dotnet/articles/core/tools/index#installation) is a new cross-platform toolchain for developing .NET Core applications. The only nuget command available at this time is restore which is executed though [dotnet restore](https://docs.microsoft.com/en-us/dotnet/articles/core/tools/dotnet-restore)


##Package Explorer
<div class="block-callout-info">
	<strong>Note</strong><br>
	This tool is OSS, and not supported by the NuGet team at Microsoft.
</div>

Package Explorer is a tool driven by our community which lets you visually explore and create nuget packages. Installing Package Explorer is easy - just click [here](https://npe.codeplex.com/releases/view/624769).

**Recommended Reading**: [Package Explorer GUI to create packages](/ndocs/tools/package-explorer)

##Choosing the Right NuGet Tool
Here is a quick reference to understand your options based on your scenario:
<table class="reference">
	<tr>
		<th>Scenario</th>
		<th align="center">nuget CLI</th>
		<th align="center">Package Manager UI</th>
		<th align="center">Package Manager Console</th>
		<th align="center">.NET CLI</th>
    <tr>
        <td>Search package</td>
        <td align="center">&#10004;</td>
		<td align="center">&#10004;</td>
		<td align="center">&#10004;</td>
		<td align="center"></td>
    </tr>
    <tr>
        <td>Install package</td>
        <td align="center">&#10004;</td>
		<td align="center">&#10004;</td>
		<td align="center">&#10004;</td>
		<td align="center"></td>
    </tr>
	<tr>
        <td>Update package</td>
        <td align="center">&#10004;</td>
		<td align="center">&#10004;</td>
		<td align="center">&#10004;</td>
		<td align="center"></td>
    </tr>
	<tr>
        <td>Uninstall package</td>
        <td align="center">&#10004;</td>
		<td align="center">&#10004;</td>
		<td align="center">&#10004;</td>
		<td align="center"></td>
    </tr>
	<tr>
        <td>Restore package</td>
        <td align="center">&#10004;</td>
		<td align="center">&#10004;</td>
		<td align="center"></td>
		<td align="center">&#10004;</td>
    </tr>
	<tr>
        <td>Package Authoring</td>
        <td align="center">&#10004;</td>
		<td align="center"></td>
		<td align="center"></td>
		<td align="center"></td>
    </tr>
	<tr>
        <td>Manage packages in your feed or nuget.org</td>
        <td align="center">&#10004;</td>
		<td></td>
		<td></td>
		<td></td>
    </tr>
	<tr>
        <td>Manage nuget sources</td>
        <td align="center">&#10004;</td>
		<td align="center">&#10004;</td>
		<td align="center">&#10004;</td>
		<td></td>
    </tr>
	<tr>
        <td>Publish package</td>
        <td align="center">&#10004;</td>
		<td></td>
		<td></td>
		<td></td>
    </tr>
	<tr>
        <td>Manage nuget config</td>
        <td align="center">&#10004;</td>
		<td></td>
		<td></td>
		<td></td>
    </tr>
	<tr>
        <td>Manage nuget cache</td>
        <td align="center">&#10004;</td>
		<td></td>
		<td></td>
		<td></td>
    </tr>
	<tr>
        <td>Replicate nuget package</td>
        <td align="center">&#10004;</td>
		<td></td>
		<td></td>
		<td></td>
    </tr>
</table>


## NuGet CLI OS Compatibility
NuGet was originally designed to be run on Windows-based operating systems to support .NET development. With the advancements of .NET applications running on non-Windows operating systems, the demand for NuGet compatibility on those environments has risen.<br>

The NuGet command-line executable will run on Linux and Mac OSX when the Mono runtime is installed. NuGet does not yet provide support for non-Windows operating systems. However, we have identified a set of features as working on Linux and Mac OS X.

<div class="block-callout-warning">
	<strong>Note</strong><br>
	The scenarios above have only been validated on Nuget 3.2 and above.
</div>

The following commands have been tested to work:

* Config
* Delete
* Help
* Install
* List
* Push
* SetApiKey
* Sources
* Spec

The following commands are partially working:

* Pack - Pack will work with *.nuspec files but will not work with project files
* Restore - Restore works with packages.config and project.json files but will not yet work with *.sln solution files

The following commands do not work properly:

* Update
