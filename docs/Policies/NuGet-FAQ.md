# NuGet Frequently Asked Questions

## Getting started

**What is required to run NuGet?**

All the info around tools including CLI tools, what you need to install and run NuGet is available [here](/ndocs/guides/install-nuget).

**Does NuGet support Mono?**

The command-line application (*nuget.exe*) builds and runs under Mono (version 3.2 or later) and allows you to create packages in Mono.

This is especially true for Mono on Windows, but there are some known issues for Mono on Linux and OS X.  To review
the known issues, [search for Mono in our issue list](https://github.com/NuGet/Home/issues?utf8=%E2%9C%93&q=is%3Aissue+is%3Aopen+mono).

Also, [a graphical client is available as an add-in for MonoDevelop](https://github.com/mrward/monodevelop-nuget-addin).

## NuGet in Visual Studio

**How do I check the exact version of NuGet installed?**

In Visual Studio go to the _Help_ > _About Microsoft Visual Studio_ menu and look for NuGet Package Manager. The version is displayed next to that entry.

Alternatively, you can launch the Package Manager Console and type in `$host` to output information about NuGet 
Powershell host including the version.

**What languages are supported by NuGet?**

Generally NuGet is a good tool for .NET languages; it is designed primarily around bringing .NET dlls into your project. Since it also supports msbuild and visual studio automation in some project types it supports other projects and languages types with various degrees of completeness.

The most recent version of NuGet supports C#, Visual Basic, F#, [WiX](http://wix.sourceforge.net/), and C++ Projects.

**What project templates are supported by NuGet?**

NuGet has full support for a variety of project templates like Windows, Web, Cloud, SharePoint, Wix and so on.

**How do I update packages that are part of visual studio templates ?**

You can do an "Update All" from the "Manage NuGet Packages" UI or use the "Update-Package" command from NuGet Package Manager Console. This will get the latest version of all the packages that are part of the template.

However, if you want to update the template in one go, instead of doing it for every project, you will have to manually update the template repository. Check out [Xavier Decoster's blog on the same.](http://www.xavierdecoster.com/update-project-template-to-latest-nuget-packages)

<div class="block-callout-warning">
    <strong>Note:</strong><br>
    Please be noted that manually updating the template repository should be done at your own risk. It might corrupt the template files or the latest version of the packages may not be compatible with each other
</div>

**Can I use NuGet outside of Visual Studio?**

**You sure can!**, check out our [Install Guide]().

## NuGet CommandLine

**How do I get the latest version of NuGet commandline?**

Check out our [Install Guide]().

**Is it possible to extend NuGet commandline?**

Yes, it is possible to add custom commands to NuGet.exe. Check out [this post](http://geekswithblogs.net/robz/archive/2011/07/15/extend-nuget-command-line.aspx) by Rob Reynolds for a quick walkthrough.

## NuGet Package Manager Console

**How do I get access to the DTE object in the Package Manager console?**

The console provides a variable named `$DTE` that returns the `DTE` object. See the `Get-Project` command in 
[Package Manager Console Powershell Reference]().

**I try to cast the $DTE variable to the type DTE2, but I get an error: Cannot convert the "EnvDTE.DTEClass" value of type "EnvDTE.DTEClass" to type "EnvDTE80.DTE2". What's wrong?**

This is a known issue with how PowerShell interacts with a COM object. Try the following:
`$dte2 = Get-Interface $dte ([EnvDTE80.DTE2])`
`Get-Interface` is a helper function added by the NuGet PowerShell host.

## Creating and Publishing packages

**How do I get my package in the feed?**

See the [Creating and publishing a package]() page.

**I have multiple versions of my library that target different versions of the .NET Framework. How do I build a single package that supports this?**

See the [Supporting Multiple .NET Framework Versions and Profiles]()

**How do I set up a local repository or feed?**

See [Hosting Your Own NuGet Feeds]().

**How can I bulk upload packages to my NuGet feed ?**

See [Bulk Publishing NuGet Packages](http://jeffhandley.com/archive/2012/12/13/Bulk-Publishing-NuGet-Packages.aspx
	
**How can I create package out of a project or solution ?**

You can use Nuget.exe [Pack command](/consume/command-line-reference#pack-command-examples) to create a NuGet package out a project. For more information, see [Creating package] topic.


## Working with Packages

**What is the difference between a project-level package and a solution-level package?**

A solution-level package has to be installed only once in a solution to be available for all projects in the solution. A project-level package must be installed separately in each project where you want to use it. Typically, a solution-level package installs new commands that can be called from within the **Package Manager Console** window.

**Is it possible to install NuGet package without internet ?**

**Yes you can!** See Scott Hanselman's Blog post entitled [How to access NuGet when NuGet.org is down (or you're on a plane)](http://www.hanselman.com/blog/HowToAccessNuGetWhenNuGetorgIsDownOrYoureOnAPlane.aspx).

**How to install packages in a different location from the default "Packages" folder?**

This can be done by setting the "repositoryPath" settings in nuget.config.
More details [here.]()

**How do I avoid checking in packages folder to source control?**

You can turn off source control integration for the Packages folder by setting the "disableSourceControlIntegration" propety in the nuget.config to "true". This key works at the solution level and hence need to be added to the NuGet.config file present in the "$(SolutionDir)\.nuget directory". Enabling package restore from VS would add the .nuget\nuget.config file automatically.

**How do I turn off package restore?**

You can disable package restore at the machine wide using the package manager settings or by setting an environment variable "EnableNugetPackageRestore" to "false". However, to disable just at sln level, you would have to manually delete the .nuget folder from the Solution and the file system and revert the changes in the csproj file related to package restore.

**Why do I get an "Unable to resolve dependency error" when installing a local package with remote dependencies?**

**Select the *All* node when installing your local package.** The *all** node aggregates all the feeds. 
When you select a specific repository under the Online tab, that restricts installation to just that node. 
The reason for this behavior is that in many cases, users of a local repository don't want to accidentally 
install a remote package due to corporate polices.

**I have multiple projects in the same folder, how can I use separate packages.config or project.json files for each project?**

In most projects where separate projects live in separate folders, this is not a problem as NuGet will identify the packages.config and project.json files in the same folder as the project file.  As of NuGet 3.3, for multiple projects in the same folder you can insert the name of the project into the packages.config or project.json filename and NuGet will use that file.  The naming convention is as follows:

`packages.config` shall match the pattern `packages.{project-name}.config`

`project.json` shall match the pattern `{project-name}.project.json`

**Help! I don't see nuget.org in my list of repositories, how do I get it back?**

There are two ways to get it back:

Add `https://api.nuget.org/v3/index.json` to your list of sources

Delete your NuGet.config file at `%appdata%\nuget` and the client will recreate it

## Managing Packages in NuGet.Org

**Is it possible to reserve names for packages that will be published in future?**

It is not possible to squat package names. If you feel that an existing package has taken the name which suits your package more, try [contacting the owner of the package](https://nuget.org/packages/[package ID]/ContactOwners?). If you didnt get response within a couple of weeks, you can contact support and the NuGet Gallery team will look in to it.

**How do I claim ownership for packages ?**

Check out [Managing Package Owners on nuget.org]() for details.

**How do I deal with a package owner who is violating my software license?**

We encourage the NuGet community to work together to resolve any disputes that may arise between package owners and the owners of other software.  We have crafted a [dispute resolution process](/Consume/Dispute-Resolution) that we ask you to follow before NuGet.org administrators intercede.

**Is it recommended to upload my test packages to NuGet.org ?**

For test purposes, you can use [staging.nuget.org](http://staging.nuget.org), or alternative public nuget servers like https://myget.org (public and private feeds) or [Visual Studio Online](https://blogs.msdn.microsoft.com/visualstudioalm/2015/08/27/announcing-package-management-support-for-vsotfs/). Please note that the packages being uploaded to staging.nuget.org may not be preserved. More details can be found [here.](http://blog.nuget.org/20130419/goodbye-preview.html)

**What is the maximum size of packages I can upload to NuGet.org?**

NuGet.org allows uploading packages up to 250MB. As a best practice, we recommend uploading smaller packages that are < 1 MB and using dependencies to link together packages. As a rule of thumb, packages contain only one assembly to avoid collissions.

NuGet's protocol for downloading packages is HTTP. The bigger the package, the higher the chance for failed installs. It is much easier to retry and download small packages than large ones.

It is possible to share dependencies between multiple packages, making the total download size for consumers of your NuGet packages smaller.

Dependencies are mostly static and never change. When fixing a bug in code, the dependencies may not have to be updated. If your dependencies ship in the same package you end up reshipping the large packages every time. By splitting NuGet packages into smaller chunks (dependencies), upgrades are much more fine-grained for the end user of the NuGet package.

**Why can't I download / upload packages to NuGet.org?**

When downloading and uploading to NuGet does not work, it's best to [contact support](https://www.nuget.org/policies/Contact) and provide additional connection troubleshooting information such as MTR or a Fiddler trace, as well as the operating system you are using, machine configuration (CPU, Network, hard drive), is your machine behind a proxy or firewall, and what version of .NET is installed on the machine. If you are use new cross platform tools (such as .NET CLI, or DNU) please indicate the exact version you are using.

Also please make sure you are using the latest version of NuGet either in visual studio or on the commandline. When you communicate with support, please include the version of your NuGet tools, the package sources you are using, a restore log (with detailed verbosity), and MTR/Fiddler traces if possible.

**To capture MTR:**

•   Download WinMTR from [http://winmtr.net/download/](http://winmtr.net/)

•   Enter `api.nuget.org` as the hostname and click *Start*.

•   Wait until the *Sent* column is >= 100.

![MTR](/images/consume/mtr.png)
 
•   Copy text to clipboard and include the copied details in your support request. Also let us know your geographical area.

**To capture Fiddler:**

•   Install the latest version of [Fiddler](http://www.telerik.com/download/fiddler). 

•   Start Fiddler and disable capturing traffic using the *File | Capture Traffic* menu.

•   Remove all sessions (select all items in the list, press the *Delete* key)

•   Configure Fiddler to capture HTTPS traffic by checking  *Decrypt HTTPS traffic* in the *HTTPS* tab of the * Tools | Fiddler Options...* menu.

•   Close Visual Studio.

•   Enable the *File | Capture Traffic* menu.

•   Start Visual Studio or NuGet.exe and perform the actions that are not working. The traffic generated by these      actions should show up in Fiddler.

•   Once the actions have run, use the *File | Save | All Sessions* menu and store the captured sessions. Include      the file in your support request.

Note: it may be required to set the `HTTP_PROXY` environment variable to `http://127.0.0.1:8888` for routing NuGet traffic through Fiddler. 

If that fails, try the [tips mentioned in this StackOverflow post](http://stackoverflow.com/questions/21049908/using-fiddler-to-sniff-visual-studio-2013-requests-proxy-firewall).
