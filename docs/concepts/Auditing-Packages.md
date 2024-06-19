---
title: Auditing package dependencies for security vulnerabilities
description: How to audit package dependencies for security vulnerabilities and acting on security audit reports.
author: JonDouglas
ms.author: jodou
ms.date: 10/11/2023
ms.topic: conceptual
---

# Auditing package dependencies for security vulnerabilities

## About security audits

A security audit for package managers like NuGet is a process that involves analyzing the security of the packages that are included in a software project.
This involves identifying vulnerabilities, evaluating risks, and making recommendations for improving security.
The audit can include a review of the packages themselves, as well as any dependencies and their associated risks.
The goal of the audit is to identify and mitigate any security vulnerabilities that could be exploited by attackers, such as code injection or cross-site scripting attacks.

| Project Type | NuGet | .NET SDK | Visual Studio |
|--------------|-------|----------|---------------|
| PackageReference | 6.8 |  .NET 8 SDK (8.0.100) | Visual Studio 2022 17.8 |
| packages.config | 6.10 | N/A | Visual Studio 2022 17.10 |

## Running a security audit with `restore`

The `restore` command automatically runs when you do a common package operation such as loading a project for the first time, adding a new package, updating a package version, or removing a package from your project in your favorite IDE.
A description of your dependencies is checked against a report of known vulnerabilities on the [GitHub Advisory Database](https://github.com/advisories?query=type%3Areviewed+ecosystem%3Anuget).

> [!IMPORTANT]
> For Audit to check packages, a package source that provides a vulnerability database must be used.
> NuGet.org's V3 URL is one such example (https://api.nuget.org/v3/index.json), but note that NuGet.org's V2 endpoint does not.

1. On the command line, navigate to your project or solution directory.
1. Run `restore` using your preferred tooling (i.e. dotnet, MSBuild, NuGet.exe, VisualStudio etc).
1. Review the warnings and address the known security vulnerabilities.

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

Review the security advisor for any mitigating factors that may allow you to continue using the package with the vulnerability.
The vulnerability may only exist when the code is used on a specific framework, operating system, or a special function is called.

#### Use a suggested package

In the case that a security advisory is reported for the package you're using and the package is marked deprecated or seems abandoned, consider using any suggested alternate package the package author has declared or a package comprising of similar functionality that is maintained.

#### Contribute a fix

If a fix does not exist for the security advisory, you may want to suggest changes that addresses the vulnerability in a pull request on package's open source repository or contact the author through the `Contact owners` section on the NuGet.org package detail page.

#### Open an issue

If you do not want to fix the vulnerability or are unable to update or replace the package, open an issue in the package's issue tracker or preferred contact method.
On NuGet.org, you can navigate to the package details page and click `Report package` which will guide you to get in contact with the author.

### No security vulnerabilities found

If no security vulnerabilities are found, this means that packages with known vulnerabilities were not found in your package graph at the present moment of time you checked.
Since the advisory database can be updated at any time, we recommend regularly checking your `dotnet restore` output and ensuring the same in your continuous integration process.

### Configuring NuGet audit

Audit can be configured via MSBuild properties in a `.csproj` or MSBuild file being evaluated as part of your project.
We recommend that audit is configured at a repository level.

| MSBuild Property | Default | Possible values | Notes |
|------------------|---------|-----------------|-------|
| NuGetAuditMode | direct | `direct` and `all` | If you'd like to audit both top-level and transitive dependencies, you can set the value to `all`. NuGetAuditMode is not applicable for packages.config projects |
| NuGetAuditLevel | low | `low`, `moderate`, `high`, and `critical` | If you'd like to see `moderate`, `high`, and `critical` advisories, set the value to `moderate` |
| NuGetAudit | true | `true` and `false` | If you wish to not receive security audit reports, you can opt-out of the experience entirely by setting the value to `false` |

### Excluding advisories

You can choose to exclude specific advisories from the audit report by adding a new `NuGetAuditSuppress` MSBuild item for each advisory. Define a `NuGetAuditSuppress` item with the `Include=` metadata set to the advisory URL you wish to suppress.

```xml
    <NuGetAuditSuppress Include="https://github.com/advisories/XXXX" />
```

Similar to the other NuGet audit configuration properties, `NuGetAuditSuppress` items can be defined at the project or repository level.

`NuGetAuditSuppress` is available for PackageReference projects, but is not currently available for packages.config projects.

### Warning codes

| Warning Code | Reason |
|--------------|----------|
| NU1900 | Error communicating with package source, while getting vulnerability information. |
| NU1901 | Package with low severity detected |
| NU1902 | Package with moderate severity detected |
| NU1903 | Package with high severity detected |
| NU1904 | Package with critical severity detected |

You can customize your build to treat these warnings as errors to [treat warnings as errors, or treat warnings not as errors](/dotnet/csharp/language-reference/compiler-options/errors-warnings#warningsaserrors-and-warningsnotaserrors).
For example, if you're already using `<TreatWarningsAsErrors>` to treat all (C#, NuGet, MSBuild, etc) warnings as errors, you can use `<WarningsNotAsErrors>NU1901;NU1902;NU1903;NU1904</WarningsNotAsErrors>` to prevent vulnerabilities discovered in the future from breaking your build.
Alternatively, if you want to keep low and moderate vulnerabilities as warnings, but treat high and critical vulnerabilities as errors, and you're not using `TreatWarningsAsErrors`, you can use `<WarningsAsErrors>NU1903;NU1904</WarningsAsErrors>`.

> [!NOTE]
> MSBuild properties for message severity such as `NoWarn` and `TreatWarningsAsErrors` are not supported for packages.config projects.

## Summary

Security auditing features are crucial for maintaining the security and integrity of software projects.
These features provide you with an additional layer of protection against security vulnerabilities and ensures that you can use open source packages with confidence.
