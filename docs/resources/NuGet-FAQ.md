---
title: NuGet Frequently-Asked Questions
description: Common questions and answers for using NuGet on the command line and in Visual Studio
author: shishirx34
ms.author: shishirh
ms.date: 06/05/2019
ms.topic: conceptual
---

# NuGet frequently-asked questions

For frequently-asked questions pertaining to NuGet.org, such as NuGet.org account questions, see [NuGet.org frequently-asked questions](../nuget-org/nuget-org-faq.md).

**What is required to run NuGet?**

All the information around both UI and command-line tools is available in the [Install guide](../install-nuget-client-tools.md).

**Does NuGet support Mono?**

The command-line tool, `nuget.exe`, builds and runs under Mono 3.2+ and can create packages in Mono.

Although `nuget.exe` works fully on Windows, there are known issues on Linux and OS X. Refer to [Mono issues](https://github.com/NuGet/Home/issues?utf8=%E2%9C%93&q=is%3Aissue+is%3Aopen+mono) on GitHub.

A [graphical client](https://github.com/mrward/monodevelop-nuget-addin) is available as an add-in for MonoDevelop.

**How can I determine what a package contains and whether it's stable and useful for my application?**

The primary source for learning about a package is its listing page on nuget.org (or another private feed). Each package page on nuget.org includes a description of the package, its version history, and usage statistics. The **Info** section on the package page also contains a link to the project's web site where you typically find many examples and other documentation to help you learn how the package is used.

For more information, see [Finding and choosing packages](../consume-packages/finding-and-choosing-packages.md).

## NuGet in Visual Studio

**How is NuGet supported in different Visual Studio products?**

- Visual Studio on Windows supports the [Package Manager UI](../consume-packages/install-use-packages-visual-studio.md) and the [Package Manager Console](../consume-packages/install-use-packages-powershell.md).
- Visual Studio for Mac has built-in NuGet capabilities as described on [Including a NuGet package in your project](/visualstudio/mac/nuget-walkthrough).
- Visual Studio Code (all platforms) does not have any direct NuGet integration. Use the [NuGet CLI](../reference/nuget-exe-cli-reference.md) or the [dotnet CLI](../reference/dotnet-commands.md).
- Azure DevOps provides [a build step to restore NuGet packages](/vsts/build-release/tasks/package/nuget). You can also [host private NuGet package feeds on Azure DevOps](https://docs.microsoft.com/azure/devops/artifacts/nuget/publish).

**How do I check the exact version of the NuGet tools that are installed?**

In Visual Studio, use the **Help > About Microsoft Visual Studio** command and look at the version displayed next to **NuGet Package Manager**.

Alternatively, launch the Package Manager Console (**Tools > NuGet Package Manager > Package Manager Console**) and enter `$host` to see information about NuGet including the version.

**What programming languages are supported by NuGet?**

NuGet generally works for .NET languages and is designed to bring .NET libraries into a project. Because it also supports MSBuild and Visual Studio automation in some project types, it also supports other projects and languages to various degrees.

The most recent version of NuGet supports C#, Visual Basic, F#, WiX, and C++.

**What project templates are supported by NuGet?**

NuGet has full support for a variety of project templates like Windows, Web, Cloud, SharePoint, Wix, and so on.

**How do I update packages that are part of Visual Studio templates?**

Go to the **Updates** tab in the Package Manager UI and select **Update All**, or use the [`Update-Package` command](../reference/ps-reference/ps-ref-update-package.md) from the Package Manager Console.

To update the template itself, you need to manually update the template repository. See [Xavier Decoster's blog](http://www.xavierdecoster.com/update-project-template-to-latest-nuget-packages) on this subject. Note that this is done at your own risk, because manual updates might corrupt the template if the latest version of all dependencies are not compatible with each other.

**Can I use NuGet outside of Visual Studio?**

Yes, NuGet works directly from the command line. See the [Install guide](../install-nuget-client-tools.md) and the [CLI reference](../reference/nuget-exe-cli-reference.md).

## NuGet command line

**How do I get the latest version of NuGet command line tool?**

See the [Install guide](../install-nuget-client-tools.md). To check the current installed version of the tool, use `nuget help`.

**What is the license for nuget.exe?**

You are allowed to redistribute nuget.exe under the terms of the MIT license. You are responsible for updating and servicing any copies of nuget.exe that you choose to redistribute.

**Is it possible to extend the NuGet command line tool?**

Yes, it's possible to add custom commands to `nuget.exe`, as described in [Rob Reynold's post](http://geekswithblogs.net/robz/archive/2011/07/15/extend-nuget-command-line.aspx).

## NuGet Package Manager Console (Visual Studio on Windows)

**How do I get access to the DTE object in the Package Manager console?**

The top-level object in the Visual Studio automation object model is called the DTE (Development Tools Environment) object. The console provides this through a variable named `$DTE`. For more information, see [Automation Model Overview](/visualstudio/extensibility/internals/automation-model-overview) in the Visual Studio Extensibility documentation.

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

A solution-level package (NuGet 3.x+) is installed only once in a solution and is then available for all projects in the solution. A project-level package is installed in each project that uses it. A solution-level package might also install new commands that can be called from within the Package Manager Console.

**Is it possible to install NuGet packages without Internet connectivity?**

Yes, see Scott Hanselman's Blog post [How to access NuGet when nuget.org is down (or you're on a plane)](http://www.hanselman.com/blog/HowToAccessNuGetWhenNuGetorgIsDownOrYoureOnAPlane.aspx) (hanselman.com).

**How do I install packages in a different location from the default packages folder?**

Set the [`repositoryPath`](../reference/nuget-config-file.md#config-section) setting in `Nuget.Config` using `nuget config -set repositoryPath=<path>`.

**How do I avoid adding the NuGet packages folder into to source control?**

Set the [`disableSourceControlIntegration`](../reference/nuget-config-file.md#solution-section) in `Nuget.Config` to `true`. This key works at the solution level and hence need to be added to the `$(Solutiondir)\.nuget\Nuget.Config` file. Enabling package restore from Visual Studio creates this file automatically.

**How do I turn off package restore?**

See [Enabling and disabling package restore](../consume-packages/package-restore.md#enable-and-disable-package-restore-in-visual-studio).

**Why do I get an "Unable to resolve dependency error" when installing a local package with remote dependencies?**

You need to select the **All** source when installing a local package into the project. This aggregates all the feeds instead of using just one. The reason this error appears is that users of a local repository often want to avoid accidentally installing a remote package due to corporate polices.

**I have multiple projects in the same folder, how can I use separate packages.config files for each project?**

In most projects where separate projects live in separate folders, this is not a problem as NuGet identifies the `packages.config` files in each project. With NuGet 3.3+ and multiple projects in the same folder, you can insert the name of the project into the `packages.config` filenames use the pattern `packages.{project-name}.config`, and NuGet uses that file.

This is not an issue when using PackageReference, as each project file contains its own list of dependencies.

**I don't see nuget.org in my list of repositories, how do I get it back?**

- Add `https://api.nuget.org/v3/index.json` to your list of sources, or
- Delete `%appdata%\.nuget\NuGet.Config` (Windows) or `~/.nuget/NuGet/NuGet.Config` (Mac/Linux) and let NuGet re-create it.
