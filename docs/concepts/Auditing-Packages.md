---
title: Auditing package dependencies for security vulnerabilities
description: How to audit package dependencies for security vulnerabilities and acting on security audit reports.
author: JonDouglas
ms.author: jodou
ms.topic: conceptual
---

# Auditing package dependencies for security vulnerabilities

## About security audits

A security audit for package managers like NuGet is a process that involves analyzing the security of the packages that are included in a software project.
This involves identifying vulnerabilities, evaluating risks, and making recommendations for improving security.
The audit can include a review of the packages themselves, as well as any dependencies and their associated risks.
The goal of the audit is to identify and mitigate any security vulnerabilities that could be exploited by attackers, such as code injection or cross-site scripting attacks.

We also have a [blog post](https://devblogs.microsoft.com/nuget/nugetaudit-2-0-elevating-security-and-trust-in-package-management/) which discusses our recommended method for taking action when a package with a known vulnerability is found to be used by your project, and tools to help get more information.

### Feature availability

| NuGet | .NET SDK | Visual Studio | Feature |
|-------|----------|---------------|---------|
| [5.9](../release-notes/NuGet-5.9.md) | .NET 5 SDK (5.0.200) | N/A | [`dotnet list package --vulnerable`](#dotnet-list-package---vulnerable) |
| [6.8](../release-notes/NuGet-6.8.md) | .NET 8 SDK (8.0.100) | Visual Studio 2022 17.8 | [NuGetAudit](#running-a-security-audit-with-restore) for PackageReference |
| [6.10](../release-notes/NuGet-6.10.md) | N/A | Visual Studio 2022 17.10 | [NuGetAudit](#running-a-security-audit-with-restore) for packages.config|
| [6.11](../release-notes/NuGet-6.11.md) | .NET 8 SDK (8.0.400) | Visual Studio 2022 17.11 | [NuGetAuditSuppress](#excluding-advisories) for PackageReference |
| [6.12](../release-notes/NuGet-6.12.md) | .NET 9 SDK (9.0.100) | Visual Studio 2022 17.12 | [Audit sources](#audit-sources). [NuGetAuditSuppress](#excluding-advisories) for packages.config. |

## Running a security audit with `restore`

The `restore` command automatically runs when you do a common package operation such as loading a project for the first time, adding a new package, updating a package version, or removing a package from your project in your favorite IDE.
Your dependencies are checked against a list of known vulnerabilities provided by your [audit sources](#audit-sources).

1. On the command line, navigate to your project or solution directory.
1. Run `restore` using your preferred tooling (i.e. dotnet, MSBuild, NuGet.exe, VisualStudio etc).
1. Review the warnings and address the known security vulnerabilities.

### Configuring NuGet Audit

Audit can be configured via MSBuild properties in a `.csproj` or MSBuild file being evaluated as part of your project.
We recommend that audit is configured at a repository level.

| MSBuild Property | Default | Possible values | Notes |
|------------------|---------|-----------------|-------|
| NuGetAuditMode | direct | `direct` and `all` | If you'd like to audit top-level dependencies only, you can set the value to `direct`. NuGetAuditMode is not applicable for packages.config projects.  |
| NuGetAuditLevel | low | `low`, `moderate`, `high`, and `critical` | The minimum severity level to report. If you'd like to see `moderate`, `high`, and `critical` advisories (exclude `low`), set the value to `moderate` |
| NuGetAudit | true | `true` and `false` | If you wish to not receive security audit reports, you can opt-out of the experience entirely by setting the value to `false` |

#### Audit Sources

Restore downloads a server's [`VulnerabilityInfo` resource](../api/vulnerability-info.md) to check against the list of packages each project is using.
The list of sources are defined by [the `auditSources` element in NuGet.Config](../reference/nuget-config-file.md#auditsources), and [warning NU1905](#warning-codes) is raised if any of the audit sources do not provide any vulnerability info.
If `auditSources` is not defined or is cleared without adding any sources, then `packageSources` will be used and warning NU1905 is suppressed.

Since a common mitigation for package substitution attacks is [to use a single package source that upstreams from nuget.org, so that NuGet is not configured to use nuget.org as a package source](Security-Best-Practices.md#nuget-feeds), audit sources can be used to use nuget.org (or any other source that provides vulnerability information) without also using it as a package source.

The data source for nuget.org's vulnerability database is [GitHub Advisory Database](https://github.com/advisories?query=type%3Areviewed+ecosystem%3Anuget).
Note that the [V2 protocol is deprecated](../nuget-org/overview-nuget-org.md#api-endpoint-for-nugetorg), so if your nuget.config is still using the V2 endpoint, you must migrate to the V3 endpoint.

```xml
<configuration>
    <auditSources>
        <clear />
        <add key="nuget.org" value="https://api.nuget.org/v3/index.json" />
    </auditSources>
</configuration>
```

Audit sources are available from [NuGet 6.12, .NET 9.0.100 SDK, and Visual Studio 2022 17.12](../release-notes/NuGet-6.12.md).
Prior to this version, NuGet Audit will only use package sources to download vulnerability information.
Audit sources are not used by `dotnet list package --vulnerable` at this time.

#### Excluding advisories

You can choose to exclude specific advisories from the audit report by adding a new `NuGetAuditSuppress` MSBuild item for each advisory.
Define a `NuGetAuditSuppress` item with the `Include=` metadata set to the advisory URL you wish to suppress.

```xml
<ItemGroup>
    <NuGetAuditSuppress Include="https://github.com/advisories/XXXX" />
</ItemGroup>
```

Similar to the other NuGet audit configuration properties, `NuGetAuditSuppress` items can be defined at the project or repository level.

`NuGetAuditSuppress` is available for PackageReference projects starting from [NuGet 6.11, Visual Studio 17.11, and the .NET 8.0.400 SDK](../release-notes/NuGet-6.11.md).
It is available for packages.config from [Visual Studio 17.12 and NuGet 6.12](../release-notes/NuGet-6.12.md).

### Warning codes

| Warning Code | Reason |
|--------------|----------|
| [NU1900](../reference/errors-and-warnings/NU1900.md) | Error communicating with package source, while getting vulnerability information. |
| [NU1901](../reference/errors-and-warnings/NU1901-NU1904.md) | Package with low severity detected |
| [NU1902](../reference/errors-and-warnings/NU1901-NU1904.md) | Package with moderate severity detected |
| [NU1903](../reference/errors-and-warnings/NU1901-NU1904.md) | Package with high severity detected |
| [NU1904](../reference/errors-and-warnings/NU1901-NU1904.md) | Package with critical severity detected |
| [NU1905](../reference/errors-and-warnings/NU1905.md) | An audit source does not provide a vulnerability database |

You can customize your build to treat these warnings as errors to [treat warnings as errors, or treat warnings not as errors](/dotnet/csharp/language-reference/compiler-options/errors-warnings#warningsaserrors-and-warningsnotaserrors).
For example, if you're already using `<TreatWarningsAsErrors>` to treat all (C#, NuGet, MSBuild, etc) warnings as errors, you can use `<WarningsNotAsErrors>$(WarningsNotAsErrors);NU1901;NU1902;NU1903;NU1904</WarningsNotAsErrors>` to prevent vulnerabilities discovered in the future from breaking your build.
Alternatively, if you want to keep low and moderate vulnerabilities as warnings, but treat high and critical vulnerabilities as errors, and you're not using `TreatWarningsAsErrors`, you can use `<WarningsAsErrors>$(WarningsAsErrors);NU1903;NU1904</WarningsAsErrors>`.

> [!NOTE]
> MSBuild properties for message severity such as `NoWarn` and `TreatWarningsAsErrors` are not supported for packages.config projects.

## Ensure restore audited projects

NuGet in MSBuild 17.13 and .NET 9.0.200 added output properties `RestoreProjectCount`, `RestoreSkippedCount` and `RestoreProjectsAuditedCount` on the restore task.
This can be used to enforce that audit ran during a restore.

Since MSBuild is a scripting language, this can be achieved a number of different ways, but also has the same restrictions as MSBuild has.
One example is to create a file *Directory.Solution.targets* in the same directory as your solution file, whose contents has a target similar to the following.
Note that *Directory.Build.props* is commonly used, but is imported by projects.
However, NuGet's restore target and task runs at the solution level, so needs to be in MSBuild's solution extensibility file, not the project/build file.

```xml
<Project>
    <Target Name="AssertRestoreTaskOutputProperties"
            AfterTargets="Restore"
            Condition="'$(CI)' == 'true'">
        <Error
            Condition="'$(RestoreProjectsAuditedCount)' != '$(RestoreProjectCount)'"
            Text=""Restore did not audit every project in the solution. Expected: $(RestoreProjectCount) Found: $(RestoreProjectsAuditedCount)"" />
    </Target>
</Project>
```

Depending on your use-case, you may wish to use condition `'$(RestoreProjectCount)' != '$([MSBuild::Add($(RestoreProjectsAuditedCount), $(RestoreSkippedCount))'` on the error message, to account for projects that restore skipped because they were already up to date.
Similarly, think about if you want this error to happen everywhere, or only in CI pipelines, and what environment variables are defined in your CI environment, and factor this into the target's condition.
Again, since MSBuild is a scripting language, you can use any of its capabilities to customize your repo however you want.
Viewing [MSBuild's metaproj](/visualstudio/msbuild/how-to-build-specific-targets-in-solutions-by-using-msbuild-exe#troubleshooting) and [binlogs](/visualstudio/msbuild/msbuild-command-line-reference#switches-for-loggers) are useful to develop and troubleshoot solution level targets.

## `dotnet list package --vulnerable`

Once a project is successfully restored, [`dotnet list package`](/dotnet/core/tools/dotnet-list-package) has a `--vulnerable` argument to filter the packages based on which packages have known vulnerabilities.
Note that `--include-transitive` is not default, so should be included.

## Actions when packages with known vulnerabilities are reported

We also have a [blog post](https://devblogs.microsoft.com/nuget/nugetaudit-2-0-elevating-security-and-trust-in-package-management/) which discusses our recommended method for taking action when a package with a known vulnerability is found to be used by your project, and tools to help get more information.

### Security vulnerabilities found with updates

If security vulnerabilities are found and updates are available for the package, you can either:

- Edit the `.csproj` or other package version location (`Directory.Packages.props`) with a newer version containing a security fix.
- Use the NuGet package manager user interface in Visual Studio to update the individual package.
- Run the `dotnet add package` command with the respective package ID to update to the latest version.

#### Transitive Packages

If a known vulnerability exists in a top-level package's transitive dependencies, you have these options:

- Add the fixed package version as a direct package reference. **Note:** Be sure to remove this reference when a new package version update becomes available and be sure to maintain the defined attributes for the expected behavior.
- Use [Central Package Management with the transitive pinning functionality](../consume-packages/Central-Package-Management.md#transitive-pinning).
- [Suppress the advisory](#excluding-advisories) until it can be addressed.
- File an issue in the top-level package's tracker to request an update.

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

## Summary

Security auditing features are crucial for maintaining the security and integrity of software projects.
These features provide you with an additional layer of protection against security vulnerabilities and ensures that you can use open source packages with confidence.
