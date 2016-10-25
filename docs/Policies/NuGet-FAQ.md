# NuGet frequently-asked questions

In this topic:

- [Getting started](#getting-started)
- [NuGet in Visual Studio](#nuget-in-visual-studio)
- [NuGet command line](#nuget-command-line)
- [NuGet Package Manager Console](#nuget-package-manager-console)
- [Creating and publishing packages](#creating-and-publishing-packages)
- [Working with packages](#working-with-packages)
- [Managing packages on nuget.org](#managing-packages-on-nuget-org)

## Getting started

**What is required to run NuGet?**

All the information around both UI and command-line tools is available in the [Install guide](/ndocs/guides/install-nuget).

**Does NuGet support Mono?**

The command-line tool, `nuget.exe`, builds and runs under Mono 3.2+ and can create packages in Mono.

Although `nuget.exe` work fully on Windows, there are known issues on Linux and OS X. Refer [Mono issues](https://github.com/NuGet/Home/issues?utf8=%E2%9C%93&q=is%3Aissue+is%3Aopen+mono) on GitHub.

A [graphical client](https://github.com/mrward/monodevelop-nuget-addin) is available as an add-in for MonoDevelop.

## NuGet in Visual Studio

**How do I check the exact version of NuGet installed?**

In Visual Studio, use the **Help > About Microsoft Visual Studio** command and look at the version displayed next to **NuGet Package Manager**. 

Alternatively, launch the Package Manager Console (**Tools > NuGet Package Manager > Package Manager Console**) and tnere `$host` to see information about NuGet including the version.

**What languages are supported by NuGet?**

NuGet generally works for .NET languages and is designed to bring .NET libraries into a project. Because it also supports MSBuild and Visual Studio automation in some project types, it also supports other projects and languages to various degrees.

The most recent version of NuGet supports C#, Visual Basic, F#, WiX, and C++.

**What project templates are supported by NuGet?**

NuGet has full support for a variety of project templates like Windows, Web, Cloud, SharePoint, Wix and so on.

**How do I update packages that are part of Visual Studio templates?**

Go to the **Updates** tab in the Package Manager UI and select **Update All**, or use the `Update-Package` command from Package Manager Console. 

To update the template itself, you'll need to manually update the template repository. See [Xavier Decoster's blog](http://www.xavierdecoster.com/update-project-template-to-latest-nuget-packages) on this subject. Note that this is done at your own risk, because manual updates might corrupt the template if the latest version of all dependencies are not compatible with each other.

**Can I use NuGet outside of Visual Studio?**

Yes, NuGet works directly from the command line. See the [Install guide](/ndocs/guides/install-nuget).


## NuGet CommandLine

**How do I get the latest version of NuGet commandline?**

See the [Install guide](/ndocs/guides/install-nuget).

**Is it possible to extend the NuGet commandline?**

Yes, it's possible to add custom commands to `nuget.exe`, as described in [Rob Reynold's post](http://geekswithblogs.net/robz/archive/2011/07/15/extend-nuget-command-line.aspx).

## NuGet Package Manager Console

**How do I get access to the DTE object in the Package Manager console?**

The console provides a variable named `$DTE` that returns the `DTE` object. See the [`Get-Project` command](/ndocs/tools/powershell-reference#get-project).

**I try to cast the $DTE variable to the type DTE2, but I get an error: Cannot convert the "EnvDTE.DTEClass" value of type "EnvDTE.DTEClass" to type "EnvDTE80.DTE2". What's wrong?**

This is a known issue with how PowerShell interacts with a COM object. Try the following:

	`$dte2 = Get-Interface $dte ([EnvDTE80.DTE2])`

`Get-Interface` is a helper function added by the NuGet PowerShell host.

## Creating and publishing packages

**How do I list my package in a feed?**

See [Creating and publishing a package](/ndocs/quickstart/create-and-publish-a-package).

**I have multiple versions of my library that target different versions of the .NET Framework. How do I build a single package that supports this?**

See [Supporting Multiple .NET Framework Versions and Profiles](/ndocs/create-packages/supporting-multiple-target-frameworks).

**How do I set up my own repository or feed?**

See the [Hosting packages overview](/ndocs/hosting-packages/overview).

**How can I upload packages to my NuGet feed in bulk?**

See [Bulk publishing NuGet packages](http://jeffhandley.com/archive/2012/12/13/Bulk-Publishing-NuGet-Packages.aspx) (jeffhandly.com).
	
## Working with packages

**What is the difference between a project-level package and a solution-level package?**

A solution-level package (NuGet 3.x and later) is installed only once in a solution and is then available for all projects in the solution. A project-level package is installed in each project that uses it. Typically, a solution-level package installs new commands that can be called from within the Package Manager Console.

**Is it possible to install NuGet package without Internet connectivity?**

Yes, see Scott Hanselman's Blog post [How to access NuGet when NuGet.org is down (or you're on a plane)](http://www.hanselman.com/blog/HowToAccessNuGetWhenNuGetorgIsDownOrYoureOnAPlane.aspx) (hanselman.com).

**How do I install packages in a different location from the default packages folder?**

Set the [`repositoryPath`](/ndocs/schema/nuget.config#config-section) setting in `nuget.config` using `nuget config -set repositoryPath=&lt;path&gt;`. 

**How do I avoid checking in packages folder to source control?**

Set the [`disableSourceControlIntegration`](/ndocs/schema/nuget.config#solution-section) in `nuget.config` to `true`. This key works at the solution level and hence need to be added to the `$(Solutiondir)\.nuget\nuget.config` file. Enabling package restore from Visual Studio creates this file automaticatlly.

**How do I turn off package restore?**

See [Enabling and disabling package restore](/ndocs/consume-packages/package-restore#enabling-and-disabling-package-restore).

**Why do I get an "Unable to resolve dependency error" when installing a local package with remote dependencies?**

You need to select the **All** source when installing a local package into the project. This aggregates all the feeds instead of using just one. The reason this error appears is that users of a local repository often want to avoid accidentally installing a remote package due to corporate polices.

**I have multiple projects in the same folder, how can I use separate packages.config or project.json files for each project?**

In most projects where separate projects live in separate folders, this is not a problem as NuGet will identify the `packages.config` and `project.json` files in each project. With NuGet 3.3+ and multiple projects in the same folder, you can insert the name of the project into the `packages.config` or `project.json` filenames as below and NuGet will use that file:

	`packages.config`: use the pattern `packages.{project-name}.config`
	`project.json`: use the pattern `{project-name}.project.json`

**I don't see nuget.org in my list of repositories, how do I get it back?**

- Add `https://api.nuget.org/v3/index.json` to your list of sources, or
- Delete the `%appdata%\.nuget\NuGet.config` and let NuGet re-create it.

## Managing packages on nuget.org

**Is it possible to reserve names for packages that will be published in future?**

No. If you feel that an existing package has taken the name which suits your package better, try contacting the owner of the package (click on the owner name on the package page on nuget.org). If you don't get a response within a reasonable time, [contact support](https://www.nuget.org/policies/Contact) and we'll look into it.

**How do I claim ownership for packages ?**

See [Managing package owners on nuget.org](/ndocs/create-packages/publish-a-package#managing-package-owners-on-nuget-org).

**How do I deal with a package owner who is violating my software license?**

We encourage the NuGet community to work together to resolve any disputes that may arise between package owners and the owners of other software. We have crafted a [dispute resolution process](/ndocs/policies/dispute-resolution) to follow before asking nuget.org administrators to intercede.

**Is it recommended to upload my test packages to nuget.org?**

For test purposes, you can use [staging.nuget.org](http://staging.nuget.org), or alternative public NuGet  servers like [myget.org](https://myget.org) or [Visual Studio Team Services](https://blogs.msdn.microsoft.com/visualstudioalm/2015/08/27/announcing-package-management-support-for-vsotfs/).

Note that packages uploaded to staging.nuget.org may not be preserved. See [Goodbye preview](http://blog.nuget.org/20130419/goodbye-preview.html).

**What is the maximum size of packages I can upload to nuget.org?**

nuget.org allows packages up to 250MB, but we recommend keeping packages under 1MB if possible and using dependencies to link packages together. As a rule of thumb, packages contain only one assembly to avoid collisions.

NuGet's uses HTTP to download packages, so larger packages have a higher likelihood of failed installs than smaller ones.

It is possible to share dependencies between multiple packages, making the total download size for consumers of your NuGet packages smaller.

Dependencies are mostly static and never change. When fixing a bug in code, the dependencies may not have to be updated. If you bundle dependencies, you end up reshipping larger packages every time. By splitting NuGet packages into related dependencies, upgrades are much more fine-grained for consumers of your package.

**Why can't I download/upload packages to nuget.org?**

First make sure you're using the latest versions of NuGet. If that continues to faile, [contact support](https://www.nuget.org/policies/Contact) and provide additional connection troubleshooting information including:

- The version of NuGet you're using
- The package sources you're using
- A restore log with detailed verbosity
- MTR or a Fiddler traces (see below)
- Your geographical area
- Your operating system version
- Machine configuration (CPU, Network, hard drive)
- Whether is your machine behind a proxy or firewall
- The versions of .NET that are installed on the machine.
- Versions of cross-platform tools such as .NET CLI, or DNU that you're using.

*To capture MTR:*

- Download WinMTR from [http://winmtr.net/download/](http://winmtr.net/)
- Enter `api.nuget.org` as the hostname and click **Start**.
- Wait until the **Sent** column is >= 100.

	![Capturing MTR](/images/consume/mtr.png)
 
- Copy text to clipboard. 

*To capture Fiddler:*

- Install the latest version of [Fiddler](http://www.telerik.com/download/fiddler). 
- Start Fiddler and disable capturing traffic using the **File > Capture Traffic** menu.
- Remove all sessions (select all items in the list, press the **Delete** key).- - Configure Fiddler to capture HTTPS traffic by checking **Decrypt HTTPS traffic** in the **HTTPS** tab of the **Tools > Fiddler Options...** menu.
- Close Visual Studio.
- Enable the **File > Capture Traffic** menu.
- Start Visual Studio or nuget.exe .exe and perform the actions that are not working. The traffic generated by these actions should show up in Fiddler.
- Once the actions have run, use **File > Save > All Sessions** to store the captured sessions.

Note: it may be required to set the `HTTP_PROXY` environment variable to `http://127.0.0.1:8888` for routing NuGet traffic through Fiddler. 

If that fails, try the [tips mentioned in this StackOverflow post](http://stackoverflow.com/questions/21049908/using-fiddler-to-sniff-visual-studio-2013-requests-proxy-firewall).