---
# required metadata

title: NuGet Frequently-Asked Questions | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 10/30/2017
ms.topic: article
ms.prod: nuget
#ms.service:
ms.technology: null
ms.assetid: 199a915d-9595-4ae2-a1fb-b15da6d7735a

# optional metadata

description: Common questions and answers for using NuGet on the command line and in Visual Studio, and working with the NuGet gallery.
keywords: NuGet Q&A, questions and answers, common problems, NuGet versions, package versions
#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer:
- karann-msft
- unniravindranathan
#ms.suite:
#ms.tgt_pltfrm:
#ms.custom:
---
# NuGet frequently-asked questions

In this topic:

- [Getting started](#getting-started)
- [NuGet in Visual Studio](#nuget-in-visual-studio)
- [NuGet command line](#nuget-command-line)
- [NuGet Package Manager Console](#nuget-package-manager-console)
- [Creating and publishing packages](#creating-and-publishing-packages)
- [Working with packages](#working-with-packages)
- [Managing packages on nuget.org](#managing-packages-on-nugetorg)
- [nuget.org not accessible](#nugetorg-not-accessible)

## Getting started

**What is required to run NuGet?**

All the information around both UI and command-line tools is available in the [Install guide](../guides/install-nuget.md).

**Does NuGet support Mono?**

The command-line tool, `nuget.exe`, builds and runs under Mono 3.2+ and can create packages in Mono.

Although `nuget.exe` works fully on Windows, there are known issues on Linux and OS X. Refer to [Mono issues](https://github.com/NuGet/Home/issues?utf8=%E2%9C%93&q=is%3Aissue+is%3Aopen+mono) on GitHub.

A [graphical client](https://github.com/mrward/monodevelop-nuget-addin) is available as an add-in for MonoDevelop.

## NuGet in Visual Studio

**How do I check the exact version of NuGet installed?**

In Visual Studio, use the **Help > About Microsoft Visual Studio** command and look at the version displayed next to **NuGet Package Manager**.

Alternatively, launch the Package Manager Console (**Tools > NuGet Package Manager > Package Manager Console**) and enter `$host` to see information about NuGet including the version.

**What languages are supported by NuGet?**

NuGet generally works for .NET languages and is designed to bring .NET libraries into a project. Because it also supports MSBuild and Visual Studio automation in some project types, it also supports other projects and languages to various degrees.

The most recent version of NuGet supports C#, Visual Basic, F#, WiX, and C++.

**What project templates are supported by NuGet?**

NuGet has full support for a variety of project templates like Windows, Web, Cloud, SharePoint, Wix, and so on.

**How do I update packages that are part of Visual Studio templates?**

Go to the **Updates** tab in the Package Manager UI and select **Update All**, or use the [`Update-Package` command](../Tools/ps-ref-update-package.md) from the Package Manager Console.

To update the template itself, you need to manually update the template repository. See [Xavier Decoster's blog](http://www.xavierdecoster.com/update-project-template-to-latest-nuget-packages) on this subject. Note that this is done at your own risk, because manual updates might corrupt the template if the latest version of all dependencies are not compatible with each other.

**Can I use NuGet outside of Visual Studio?**

Yes, NuGet works directly from the command line. See the [Install guide](../guides/install-nuget.md) and the [CLI reference](../tools/nuget-exe-CLI-Reference.md).

## NuGet command line

**How do I get the latest version of NuGet command line tool?**

See the [Install guide](../guides/install-nuget.md).

**Is it possible to extend the NuGet command line tool?**

Yes, it's possible to add custom commands to `nuget.exe`, as described in [Rob Reynold's post](http://geekswithblogs.net/robz/archive/2011/07/15/extend-nuget-command-line.aspx).

## NuGet Package Manager Console

**How do I get access to the DTE object in the Package Manager console?**

The top-level object in the Visual Studio automation object model is called the DTE (Development Tools Environment) object. The console provides this through a variable named `$DTE`. For more information, see [Automation Model Overview](https://docs.microsoft.com/visualstudio/extensibility/internals/automation-model-overview) in the Visual Studio Extensibility documentation.

**I try to cast the $DTE variable to the type DTE2, but I get an error: Cannot convert the "EnvDTE.DTEClass" value of type "EnvDTE.DTEClass" to type "EnvDTE80.DTE2". What's wrong?**

This is a known issue with how PowerShell interacts with a COM object. Try the following:

```ps
`$dte2 = Get-Interface $dte ([EnvDTE80.DTE2])`
```

`Get-Interface` is a helper function added by the NuGet PowerShell host.

## Creating and publishing packages

**How do I list my package in a feed?**

See [Creating and publishing a package](../quickstart/create-and-publish-a-package.md).

**I have multiple versions of my library that target different versions of the .NET Framework. How do I build a single package that supports this?**

See [Supporting Multiple .NET Framework Versions and Profiles](../create-packages/supporting-multiple-target-frameworks.md).

**How do I set up my own repository or feed?**

See the [Hosting packages overview](../hosting-packages/overview.md).

**How can I upload packages to my NuGet feed in bulk?**

See [Bulk publishing NuGet packages](http://jeffhandley.com/archive/2012/12/13/Bulk-Publishing-NuGet-Packages.aspx) (jeffhandly.com).

## Working with packages

**What is the difference between a project-level package and a solution-level package?**

A solution-level package (NuGet 3.x and later) is installed only once in a solution and is then available for all projects in the solution. A project-level package is installed in each project that uses it. A solution-level package might also install new commands that can be called from within the Package Manager Console.

**Is it possible to install NuGet packages without Internet connectivity?**

Yes, see Scott Hanselman's Blog post [How to access NuGet when nuget.org is down (or you're on a plane)](http://www.hanselman.com/blog/HowToAccessNuGetWhenNuGetorgIsDownOrYoureOnAPlane.aspx) (hanselman.com).

**How do I install packages in a different location from the default packages folder?**

Set the [`repositoryPath`](../Schema/nuget-config-file.md#config-section) setting in `Nuget.Config` using `nuget config -set repositoryPath=<path>`.

**How do I avoid checking in packages folder to source control?**

Set the [`disableSourceControlIntegration`](../Schema/nuget-config-file.md#solution-section) in `Nuget.Config` to `true`. This key works at the solution level and hence need to be added to the `$(Solutiondir)\.nuget\Nuget.Config` file. Enabling package restore from Visual Studio creates this file automatically.

**How do I turn off package restore?**

See [Enabling and disabling package restore](../consume-packages/package-restore.md#enabling-and-disabling-package-restore).

**Why do I get an "Unable to resolve dependency error" when installing a local package with remote dependencies?**

You need to select the **All** source when installing a local package into the project. This aggregates all the feeds instead of using just one. The reason this error appears is that users of a local repository often want to avoid accidentally installing a remote package due to corporate polices.

**I have multiple projects in the same folder, how can I use separate packages.config or project.json files for each project?**

In most projects where separate projects live in separate folders, this is not a problem as NuGet identifies the `packages.config` and `project.json` files in each project. With NuGet 3.3+ and multiple projects in the same folder, you can insert the name of the project into the `packages.config` or `project.json` filenames as below and NuGet uses that file:

    `packages.config`: use the pattern `packages.{project-name}.config`
    `project.json`: use the pattern `{project-name}.project.json`

This is not an issue when using PackageReference, as each project file contains its own list of dependencies.

**I don't see nuget.org in my list of repositories, how do I get it back?**

- Add `https://api.nuget.org/v3/index.json` to your list of sources, or
- Delete the `%appdata%\.nuget\NuGet.Config` and let NuGet re-create it.

## Managing packages on nuget.org

**Can I edit package metadata after it's been uploaded? Why do you require editing the nuspec and uploading a new package for making changes to package metadata?**

NuGet requires all packages to be signed. A design principle of package signing is that signed package content must be immutable, which includes the nuspec. Editing the package metadata results in changes to the nuspec, invalidating existing signatures. We recommend modifying existing workflows to not require editing the package metadata after the package has been created.

Note that dependencies listed for your package are generated automatically from the package itself and cannot be edited.

In addition, uploading packages to [staging.nuget.org](http://staging.nuget.org) is a great way to test and validate your package without making a package available in the public gallery.

**Is it possible to reserve names for packages that will be published in future?**

Yes. You can reserve IDs for packages on [nuget.org](https://www.nuget.org/) by requesting a package ID prefix for your account. In order to request a package ID prefix, send mail to account (at) nuget.org with the package owner display name, and the requested package ID prefix.  

**How do I claim ownership for packages ?**

See [Managing package owners on nuget.org](../create-packages/publish-a-package.md#managing-package-owners-on-nugetorg).

**How do I deal with a package owner who is violating my software license?**

We encourage the NuGet community to work together to resolve any disputes that may arise between package owners and the owners of other software. We have crafted a [dispute resolution process](../policies/dispute-resolution.md) to follow before asking nuget.org administrators to intercede.

**Is it recommended to upload my test packages to nuget.org?**

For test purposes, you can use [staging.nuget.org](http://staging.nuget.org), or alternative public NuGet servers like [myget.org](https://myget.org) or [Visual Studio Team Services](https://blogs.msdn.microsoft.com/visualstudioalm/2015/08/27/announcing-package-management-support-for-vsotfs/).

Note that packages uploaded to staging.nuget.org may not be preserved. See [Goodbye preview](http://blog.nuget.org/20130419/goodbye-preview.html).

**What is the maximum size of packages I can upload to nuget.org?**

nuget.org allows packages up to 250MB, but we recommend keeping packages under 1MB if possible and using dependencies to link packages together. As a rule of thumb, packages contain only one assembly to avoid collisions.

NuGet uses HTTP to download packages, so larger packages have a higher likelihood of failed installs than smaller ones.

It is possible to share dependencies between multiple packages, making the total download size for consumers of your NuGet packages smaller.

Dependencies are mostly static and never change. When fixing a bug in code, the dependencies may not need to be updated. If you bundle dependencies, you end up reshipping larger packages every time. By splitting NuGet packages into related dependencies, upgrades are much more fine-grained for consumers of your package.

## nuget.org not accessible

**Why can't I download packages from or upload packages to nuget.org?**

First, make sure you're using the latest versions of NuGet. If that version continues to fail, [contact support](https://www.nuget.org/policies/Contact) and provide additional connection troubleshooting information including:

- The version of NuGet you're using
- The package sources you're using
- A restore log with detailed verbosity
- MTR or a Fiddler traces (see below)
- Your geographical area
- Your operating system version
- Machine configuration (CPU, Network, hard drive)
- Whether is your machine behind a proxy or firewall
- The versions of .NET that are installed on the machine
- Versions of cross-platform tools such as .NET CLI, or DNU that you're using

*To capture MTR:*

- Download WinMTR from [http://winmtr.net/download/](http://winmtr.net/)
- Enter `api.nuget.org` as the hostname and click **Start**.
- Wait until the **Sent** column is >= 100.

    ![Capturing MTR](media/mtr.png)

- Copy text to clipboard.

*To capture Fiddler:*

- Install the latest version of [Fiddler](http://www.telerik.com/download/fiddler).
- Start Fiddler and disable capturing traffic using the **File > Capture Traffic** menu.
- Remove all sessions (select all items in the list, press the **Delete** key).
- Configure Fiddler to capture HTTPS traffic by checking **Decrypt HTTPS traffic** in the **HTTPS** tab of the **Tools > Fiddler Options...** menu.
- Close Visual Studio.
- Enable the **File > Capture Traffic** menu.
- Start Visual Studio or nuget.exe .exe and perform the actions that are not working. The traffic generated by these actions should show up in Fiddler.
- Once the actions have run, use **File > Save > All Sessions** to store the captured sessions.

Note: it may be required to set the `HTTP_PROXY` environment variable to `http://127.0.0.1:8888` for routing NuGet traffic through Fiddler.

If that fails, try the [tips mentioned in this StackOverflow post](http://stackoverflow.com/questions/21049908/using-fiddler-to-sniff-visual-studio-2013-requests-proxy-firewall).
