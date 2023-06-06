---
title: Auditing package dependencies for security vulnerabilities
description: How to audit package dependencies for security vulnerabilities and acting on security audit reports.
author: JonDouglas
ms.author: jodou
ms.date: 05/04/2023
ms.topic: conceptual
---

# Auditing package dependencies for security vulnerabilities

## About security audits

A security audit for package managers like NuGet is a process that involves analyzing the security of the packages that are included in a software project. This involves identifying vulnerabilities, evaluating risks, and making recommendations for improving security. The audit can include a review of the packages themselves, as well as any dependencies and their associated risks. The goal of the audit is to identify and mitigate any security vulnerabilities that could be exploited by attackers, such as code injection or cross-site scripting attacks. 

> [!IMPORTANT]
> Security auditing at restore time is available in .NET 8 Preview 4+ and Visual Studio 17.7 Preview 2+.

## Running a security audit with `restore`

The `restore` command automatically runs when you do a common package operation such as loading a project for the first time, adding a new package, updating a package version, or removing a package from your project in your favorite IDE. A description of your dependencies is checked against a report of known vulnerabilities on the [GitHub Advisory Database](https://github.com/advisories?query=type%3Areviewed+ecosystem%3Anuget).

> [!IMPORTANT]
> For Audit to check packages, a package source that provides a vulnerability database must be used.
> NuGet.org's V3 URL is one such example (https://api.nuget.org/v3/index.json), but note that NuGet.org's V2 endpoint does not.

> [!NOTE]
> .NET 8 preview 5+ enables Audit by default, but Visual Studio 17.7 does not ship .NET 8.
> To opt-in to Audit explicitly, set `<NuGetAudit>true</NuGetAudit>` in your project file, or a *Directory.Build.props* file.

1. On the command line, navigate to your project or solution directory.
2. Ensure your project or solution contains a `.csproj` file.
3. Type `dotnet restore` or `restore` using your preferred tooling (i.e. MSBuild, NuGet.exe, etc).
4. Review the audit report and address the known security vulnerabilities.

## Reviewing and acting on the security audit report

Running `dotnet restore` will produce a report of security vulnerabilities with the affected package name, the severity of the vulnerability, and a link to the advisory for more details.

### Security vulnerabilities found with updates

If security vulnerabilities are found and updates are available for the package, you can either:

- Edit the `.csproj` or other package version location (`Directory.Packages.props`) with a newer version containing a security fix.
- Use the NuGet package manager user interface in Visual Studio to update the individual package.
- Run the `dotnet add package` command with the respective package ID to update to the latest version.

### Security vulnerabilities found with no updates

In the case that a known vulnerability exists in a package without a security fix, you can do the following.

- Check for any mitigating factors outlined in the advisory report.
- Use a suggested package if the package is marked deprecated or is abandoned.
- If the package is open source, consider contributing a fix.
- Open an issue in the package's issue tracker.

#### Check for mitigating factors

Review the security advisor for any mitigating factors that may allow you to continue using the package with the vulnerability. The vulnerability may only exist when the code is used on a specific framework, operating system, or a special function is called.

#### Use a suggested package

In the case that a security advisory is reported for the package you're using and the package is marked deprecated or seems abandoned, consider using any suggested alternate package the package author has declared or a package comprising of similar functionality that is maintained.

#### Contribute a fix

If a fix does not exist for the security advisory, you may want to suggest changes that addresses the vulnerability in a pull request on package's open source repository or contact the author through the `Contact owners` section on the NuGet.org package detail page.

#### Open an issue

If you do not want to fix the vulnerability or are unable to update or replace the package, open an issue in the package's issue tracker or preferred contact method. On NuGet.org, you can navigate to the package details page and click `Report package` which will guide you to get in contact with the author.

### No security vulnerabilities found

If no security vulnerabilities are found, this means that packages with known vulnerabilities were not found in your package graph at the present moment of time you checked. Since the advisory database can be updated at any time, we recommend regularly checking your `dotnet restore` output and ensuring the same in your continuous integration process.

### Setting a security audit level

In cases where you only care about a certain threshold of a security advisory severity, you can set the `<NuGetAuditLevel>` MSBuild property to the desired level in which auditing will fail. Possible values are `low`, `moderate`, `high`, and `critical`. For example if you only want to see `moderate`, `high`, and `critical` advisories, you can set the following:

```xml
<NuGetAuditLevel>moderate</NuGetAuditLevel>
```

### Excluding advisories

There is no support for excluding individual advisories at this time. You can use `<NoWarn>` to suppress `NU1901`-`NU1904` warnings or use the `<NuGetAuditLevel>` functionality to ensure your audit reports are useful to your workflow.

### Warning codes

| Warning Code | Severity |
|--------------|----------|
| NU1901 | low |
| NU1902 | moderate |
| NU1903 | high |
| NU1904 | critical |

### Disabling security auditing

At any time you wish to not receive security audit reports, you can opt-out of the experience entirely by setting the following MSBuild property in a `.csproj` or MSBuild file being evaluated as part of your project:

```xml
<NuGetAudit>false</NuGetAudit>
```

## Summary

Security auditing features are crucial for maintaining the security and integrity of software projects. These features provide you with an additional layer of protection against security vulnerabilities and ensures that you can use open source packages with confidence.
