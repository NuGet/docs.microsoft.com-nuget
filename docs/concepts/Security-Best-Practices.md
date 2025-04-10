---
title: Best practices for a secure software supply chain
description: Best practices for securing your software supply chain using NuGet & GitHub.
author: JonDouglas
ms.author: jodou
ms.topic: conceptual
---

# Best practices for a secure software supply chain

Open Source is everywhere.
It is in many proprietary codebases and community projects.
For organizations and individuals, the question today is not whether you are or are not using open-source code, but what open-source code you are using, and how much.

If you're not aware of what is in your software supply chain, an upstream vulnerability in one of your dependencies can be fatal, making you, and your customers, vulnerable to a potential compromise.
In this document, we will dive deeper into what the term “software supply chain” means, why it matters, and how you can help secure your project’s supply chain with best practices.

![The State of the Octoverse 2020 - Open Source](media/opensource-percent.png)

## Dependencies

The term software supply chain is used to refer to everything that goes into your software and where it comes from.
It is the dependencies and properties of your dependencies that your software supply chain depends on.
A dependency is what your software needs to run.
It can be code, binaries, or other components, and where they come from, such as a repository or package manager.

It includes who wrote the code, when it was contributed, how it was reviewed for security issues, known vulnerabilities, supported versions, license information, and just about anything that touches it at any point of the process.

Your supply chain also encompasses other parts of your stack beyond a single application, such as your build and packaging scripts or the software that runs the infrastructure your application relies on.

## Vulnerabilities

Today, software dependencies are pervasive.
It is quite common for your projects to use hundreds of open-source dependencies for functionality that you did not have to write yourself.
This may mean that most of your application consists of code that you did not author.

![The State of the Octoverse 2020 - Dependencies](media/dependencies.png)

Possible vulnerabilities in your third-party or open-source dependencies, are presumably dependencies you cannot control as tightly as the code you write, which can create potential security risks in your supply chain.

If one of these dependencies has a vulnerability, the chances are you have a vulnerability as well.
This can be scary as one of your dependencies may change without you even knowing.
Even if a vulnerability exists in a dependency today, but is not exploitable, it can be exploitable in the future.

Being able to leverage the work of thousands of open-source developers and library authors means that thousands of strangers can effectively contribute directly to your production code.
Your product, through your software supply chain, is affected by unpatched vulnerabilities, innocent mistakes, or even malicious attacks against dependencies.

## Supply chain compromises

The traditional definition of a supply chain comes from manufacturing; it is the chain of processes required to make and supply something.
It includes planning, supply of materials, manufacturing, and retail.
A software supply chain is similar, except instead of materials, it is code.
Instead of manufacturing, it is development.
Instead of digging ore from the ground, code is sourced from suppliers, commercial or open source, and, in general, the open-source code comes from repositories.
Adding code from a repository means your product takes a dependency on that code.

One example of a software supply chain attack occurs when malicious code is purposefully added to a dependency, using the supply chain of that dependency to distribute the code to its victims.
Supply chain attacks are real.
There are many methods to attack a supply chain, from directly inserting malicious code as a new contributor, to taking over a contributor’s account without others noticing, or even compromising a signing key to distribute software that is not officially part of the dependency.

A software supply chain attack is in and of itself rarely the end goal, rather it is the beginning of an opportunity for an attacker to insert malware or provide a backdoor for future access.

![The State of the Octoverse 2020 - Vulnerability Lifecycle](media/vulnerability-lifecycle.png)

## Unpatched software

The use of open source today is significant and is not expected to slow down anytime soon.
Given that we are not going to stop using open-source software, the threat to supply chain security is unpatched software.
Knowing that, how can you address the risk that a dependency of your project has a vulnerability?

- **Knowing what is in your environment.** This requires discovering your dependencies and any transitive dependencies to understand the risks of those dependencies such as vulnerabilities or licensing restrictions.
- **Manage your dependencies.** When a new security vulnerability is discovered, you must determine whether you are impacted, and if so, update to the latest version and security patch available.
   This is especially important to review changes that introduce new dependencies or regularly auditing older dependencies.
- **Monitor your supply chain.** This is by auditing the controls you have in place to manage your dependencies.
   This will help you enforce more restrictive conditions to be met for your dependencies.

![The State of the Octoverse 2020 - Advisories](media/advisories.png)

We will cover various tools and techniques that NuGet and GitHub provides, which you can use today to address potential risks inside your project.

## Knowing what is in your environment

### Packages with known vulnerabilities

**📦 Package Consumer | 📦🖊 Package Author**

.NET 8 and Visual Studio 17.8 added [NuGetAudit](Auditing-Packages.md), which will warn about direct packages with known vulnerabilities during restore.
.NET 9 and Visual Studio 17.12 changed the default to warn about transitive packages as well.

NuGetAudit requires a source to provide a known vulnerabilities database, so if you're not using nuget.org as a package source, you should add it as an [audit source](Auditing-Packages.md#audit-sources).

By the time that NuGet is warning you, the vulnerability is publicly known.
Attackers can use this public disclosure to develop attacks for targets who have not patched their applications.
Therefore, when you get a warning that a package your project is using has a known vulnerability, you should quickly take action.

### NuGet dependency graph

**📦 Package Consumer**

You can view your NuGet dependencies in your project by looking directly at the respective project file.

This is typically found in one of two places:

- [`packages.config`](../reference/packages-config.md) – Located in the project root.
- [`<PackageReference>`](../consume-packages/package-references-in-project-files.md) – Located in the project file.

Depending on what method you use to manage your NuGet dependencies, you can also use Visual Studio to view your dependencies directly in [Solution Explorer](/visualstudio/ide/solutions-and-projects-in-visual-studio#solution-explorer) or [NuGet Package Manager](../consume-packages/install-use-packages-visual-studio.md).

For CLI environments, you can use the [`dotnet list package` command](/dotnet/core/tools/dotnet-list-package) to list out your project or solution’s dependencies.
You can also use the [`dotnet nuget why` command](/dotnet/core/tools/dotnet-nuget-why) to understand why transitive packages (those not directly referenced by your project) are being included in your project's package graph.

For more information on managing NuGet dependencies, [see the following documentation](../consume-packages/overview-and-workflow.md).

### GitHub dependency graph

**📦 Package Consumer | 📦🖊 Package Author**

You can use GitHub’s dependency graph to see the packages your project depends on and the repositories that depend on it.
This can help you see any vulnerabilities detected in its dependencies.

For more information on GitHub repository dependencies, [see the following documentation](https://github.co/dependency-graph).

### Dependency versions

**📦 Package Consumer | 📦🖊 Package Author**

To ensure a secure supply chain of dependencies, you will want to ensure that all of your dependencies & tooling are regularly updated to the latest stable version as they will often include the latest functionality and security patches to known vulnerabilities.
Your dependencies can include code you depend on, binaries you consume, tooling you use, and other components.
This may include:

- [Visual Studio](https://visualstudio.microsoft.com/downloads/)
- [.NET SDK & Runtime](https://dotnet.microsoft.com/download)
- [NuGet](https://www.nuget.org/downloads)
- [NuGet packages](../consume-packages/reinstalling-and-updating-packages.md)

## Manage your dependencies

### NuGet deprecated and vulnerable dependencies

**📦 Package Consumer | 📦🖊 Package Author**

You can use the [dotnet CLI](/dotnet/core/tools/dotnet-list-package) to list any known deprecated or vulnerable dependencies you may have inside your project or solution.
You can use the command `dotnet list package --deprecated` or `dotnet list package --vulnerable` to provide you a list of any known deprecations or vulnerabilities.
[NuGetAudit](Auditing-Packages.md) can warn you about known vulnerable dependencies, and is enabled by default when [a source provides a vulnerabilities database](Auditing-Packages.md#audit-sources).

### GitHub vulnerable dependencies

**📦 Package Consumer | 📦🖊 Package Author**

If your project is hosted on GitHub, you can leverage [GitHub Security](https://docs.github.com/en/free-pro-team@latest/github/finding-security-vulnerabilities-and-errors-in-your-code/automatically-scanning-your-code-for-vulnerabilities-and-errors) to find security vulnerabilities and errors in your project and Dependabot will fix them by opening up a pull request against your codebase.

Catching vulnerable dependencies before they are introduced is one goal of the [“Shift Left”](https://en.wikipedia.org/wiki/Shift-left_testing) movement.
Being able to have information about your dependencies such as their license, transitive dependencies, and the age of dependencies helps you do just that.

For more information about Dependabot alerts & security updates, [see the following documentation](https://docs.github.com/en/github/managing-security-vulnerabilities/about-alerts-for-vulnerable-dependencies).

### NuGet feeds

**📦 Package Consumer**

Use package sources that you trust.
When using multiple public & private NuGet source feeds, a package can be downloaded from any of the feeds.
To ensure your build is predictable and secure from known attacks such as [Dependency Confusion](https://medium.com/@alex.birsan/dependency-confusion-4a5d60fec610), knowing what specific feed(s) your packages are coming from is a best practice.
You can use a single feed or private feed with upstreaming capabilities for protection.

For more information to secure your package feeds, see [3 Ways to Mitigate Risk When Using Private Package Feeds](https://azure.microsoft.com/resources/3-ways-to-mitigate-risk-using-private-package-feeds/en-us/).

When using a private feed, refer to the [security best practices for managing credentials](../consume-packages/consuming-packages-authenticated-feeds.md#security-best-practices-for-managing-credentials).

### Client trust policies

**📦 Package Consumer**

There are policies that you can opt-into in which you require the packages you use to be signed.
This allows you to trust a package author, as long as it is author signed, or trust a package if it is owned by a specific user or account that is repository signed by NuGet.org.

To configure client trust policies, [see the following documentation](../consume-packages/installing-signed-packages.md).

### Lock files

**📦 Package Consumer**

Lock files store the hash of your package’s content.
If the content hash of a package you want to install matches with the lock file, it will ensure package repeatability.

To enable lock files, [see the following documentation](../consume-packages/package-references-in-project-files.md#locking-dependencies).

### Package Source mapping

**📦 Package Consumer**

Package Source Mapping allows you to centrally declare which source each package in your solution should restore from in your nuget.config file.

To enable package source mapping, [see the following documentation](../consume-packages/package-source-mapping.md).

## Secure computers

### Directory permissions

**📦 Package Consumer**

On Windows and Mac, and some Linux distributions, user account home directories are private by default.
However, some Linux distributions make user directories readable by other accounts on the same computer by default.
Additionally, there are [multiple configuration options to redirect NuGet's global packages folder and HTTP cache to non-default locations](../consume-packages/managing-the-global-packages-and-cache-folders.md).
Solutions, projects, and repositories might also be created outside of the user's home directory.

If you use any packages that are not on nuget.org, then if any other account on the computer can read NuGet's global packages or HTTP cache directories, or the project's build output directory, then these packages might be disclosed to people who should not have access to those packages.

On Linux, `dotnet nuget update source` will change *nuget.config* file permissions to make it only readable by the file owner.
However, if you edit the *nuget.config* file in any other way, and the file is in a location that other accounts can read the file, there might be information disclosure about package source URL or package source credentials.
You should ensure any nuget.config file cannot be read by other users of the same computer.

### Solutions within the downloads directory

**📦 Package Consumer**

Extra care should be taken if working on solutions or projects in your downloads directory.
NuGet will [accumulate settings from multiple config files](../consume-packages/configuring-nuget-behavior.md), and MSBuild will typically import *Directory.Build.props*, *Directory.NuGet.props*, *Directory.Build.targets*, and potentially other files, from any parent directory, right up to the filesystem root.

The downloads folder has additional risk, since it's usually the default location that web browsers will download files from the internet

### Build Agents

**📦 Package Consumer**

Build agents (CI agents) that are not reset to an initial state after every build have multiple risks that must be considered.

To learn about secure ways to manage credentials, [see the docs on consuming packages from authenticated feeds](../consume-packages/consuming-packages-authenticated-feeds.md).

To learn about modifying the directories that NuGet stores data in, see [the docs on managing the global packages, cache, and temp folders](../consume-packages/managing-the-global-packages-and-cache-folders.md).
These directories should be configured to a directory that the CI agent cleans after every build.

Note that any packages used by your project might be left in your project's build output directory.
If your project uses packages from authenticated sources, then other users of the same CI agent might gain unauthorized access to the package assemblies.
Therefore, you should also clean your repo at the end of your build, even when the build fails or is cancelled.

## Monitor your supply chain

### GitHub secret scanning

**📦🖊 Package Author**

GitHub scans repositories for NuGet API keys to prevent fraudulent uses of secrets that were accidentally committed.

To learn more about secret scanning, see [About secret scanning](https://docs.github.com/en/github/administering-a-repository/about-secret-scanning).

### Author Package Signing

**📦🖊 Package Author**

[Author signing](../reference/signed-packages-reference.md) allows a package author to stamp their identity on a package and for a consumer to verify it came from you.
This protects you against content tampering and serves as a single source of truth about the origin of the package and the package authenticity.
When combined with client trust policies, you can verify a package came from a specific author.

To author sign a package, see [Sign a package](../create-packages/sign-a-package.md).

### Reproducible Builds

**📦🖊 Package Author**

Reproducible builds creates binaries that are byte-for-byte identical each time you build it, and contain source code links and compiler metadata that enable a package consumer to recreate the binary directly and validate that the build environment has not been compromised.

To learn more about reproducible builds, see [Producing Packages with Source Link](https://devblogs.microsoft.com/dotnet/producing-packages-with-source-link/) and the [Reproducible Build Validation](https://github.com/dotnet/designs/blob/main/accepted/2020/reproducible-builds.md) spec.

### Two-Factor Authentication (2FA)

**📦🖊 Package Author**

Every account on nuget.org has 2FA enabled.
This adds an extra layer of security when [logging into your GitHub account](https://docs.github.com/en/github/authenticating-to-github/securing-your-account-with-two-factor-authentication-2fa) or your [NuGet.org account](../nuget-org/individual-accounts.md#add-a-new-individual-account).

### Package ID prefix reservation

**📦🖊 Package Author**

To protect the identity of your packages, you can reserve a package ID prefix with your respective namespace to associate a matching owner if your package ID prefix properly falls under the [specified criteria](../nuget-org/id-prefix-reservation.md#id-prefix-reservation-criteria).

To learn about reserving ID prefixes, see [Package ID prefix reservation](../nuget-org/id-prefix-reservation.md).

### Deprecating and unlisting a vulnerable package

**📦🖊 Package Author**

To protect the .NET package ecosystem when you are aware of a vulnerability in a package you have authored, do your best to deprecate and unlist the package so it is hidden from users searching for packages.
If you are consuming a package that is deprecated and unlisted, you should avoid using the package.

To learn how to deprecate and unlist a package, see the following documentation on [deprecating](../nuget-org/deprecate-packages.md) and [unlisting packages](../nuget-org/policies/deleting-packages.md#unlisting-a-package).

Also consider reporting the known to the [GitHub Advisories Database](https://github.com/advisories).

## Summary

Your software supply chain is anything that goes into or affects your code.
Even though supply chain compromises are real and growing in popularity, they are still rare; so the most important thing you can do is protect your supply chain by **being aware of your dependencies, managing your dependencies** and **monitoring your supply chain.**

You learned about various methods that NuGet and [GitHub](/training/modules/maintain-secure-repository-github/) provide that are available to you today to be more effective in viewing, managing, and monitoring your supply chain.

For more information about securing the world's software, see [The State of the Octoverse 2020 Security Report](https://octoverse.github.com/static/github-octoverse-2020-security-report.pdf).
