---
title: Minimum package publish age
description: Delay automatic selection of newly published NuGet package versions by configuring a minimum publish age for each package source.
author: zivkan
ms.date: 11/03/2026
ms.topic: concept-article
ai-usage: ai-generated
---

# Minimum package publish age

Minimum package publish age lets you delay automatic selection of newly published package versions.
You configure a waiting period for each package source, so you can delay updates from a public source without delaying packages from a trusted internal source.
The delay gives package owners and the community time to discover and respond to malicious releases before you adopt them.
It doesn't guarantee that a package is safe.

The feature is available starting with NuGet 8.0, .NET SDK 11.0.100, and Visual Studio 18.12.
It's opt-in: package sources have no minimum publish age unless you configure one.

## Configure a minimum publish age

Add the `minPublishAgeHours` attribute to a source in the `packageSources` section of your `nuget.config` file.
Specify the age in whole hours.
Omitting the attribute or setting it to `0` allows immediate use of new versions from that source.

For example, the following configuration sets a 72-hour minimum age for nuget.org and no delay for an internal source:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <packageSources>
    <clear />
    <add key="nuget.org" value="https://api.nuget.org/v3/index.json" minPublishAgeHours="72" />
    <add key="Contoso" value="https://contoso.test/nuget/v3/index.json" />
  </packageSources>
  <minPublishAgeExceptions />
</configuration>
```

In this example, a version from nuget.org becomes eligible when at least 72 hours have elapsed since its publish date.
The empty `minPublishAgeExceptions` section prevents exceptions inherited from other configuration files from bypassing the delay.

To exempt packages with a specific prefix, replace the empty section with a package pattern:

```xml
<minPublishAgeExceptions>
  <package pattern="Contoso.*" />
</minPublishAgeExceptions>
```

This example exempts all packages whose IDs start with `Contoso.` from the minimum age on all sources.
Patterns are case-insensitive and use the [Package Source Mapping pattern syntax](package-source-mapping.md#package-pattern-syntax).
An exception applies to every version of a matching package.
The section replaces any inherited exception list rather than adding to it.
For the complete syntax, see the [`nuget.config` reference](../reference/nuget-config-file.md#minpublishageexceptions).

Commit a `nuget.config` file at the root of your repository so contributors use the same settings.
If a higher-priority file redefines a source, specify `minPublishAgeHours` again; omitting it resets that source's age to zero.
For information about configuration locations and precedence, see [Common NuGet configurations](configuring-nuget-behavior.md).

You can also configure the age and exceptions in [Visual Studio options](nuget-visual-studio-options.md#minimum-publish-age).
To edit a source's age from the command line, use the `--min-publish-age-hours` option with [`dotnet nuget add source`](/dotnet/core/tools/dotnet-nuget-add-source) or [`dotnet nuget update source`](/dotnet/core/tools/dotnet-nuget-update-source).

## How package selection uses the age

When [`dotnet package add`](/dotnet/core/tools/dotnet-package-add) and [`dotnet package update`](/dotnet/core/tools/dotnet-package-update) select the latest version for you, they exclude versions that haven't reached the source's minimum publish age.
Explicitly requesting a version bypasses automatic age filtering.
For updates, if no eligible newer version is available, the existing reference stays unchanged rather than being downgraded.
The update command also reports packages with otherwise suitable versions that haven't reached the minimum age.

With `--vulnerable`, `dotnet package update` instead selects the lowest eligible version without known vulnerabilities.
The minimum age still applies, even when a newer version fixes a known vulnerability.
If no eligible fix is available, the existing reference stays unchanged.

Each source is evaluated independently.
If a version is eligible on at least one source considered for that package, it can be selected even if another source's minimum age would exclude it.
For example, a source with no minimum age can make a recently published version eligible even when the same version is too new on another source.
Use [Package Source Mapping](package-source-mapping.md) to limit packages to their expected sources.

> [!IMPORTANT]
> Minimum publish age filters automatic version choices; it isn't a general rule that blocks package downloads.
> Don't rely on it to enforce the age of every package in your dependency graph.

### Package restore and floating versions

Ordinary package restore doesn't check minimum publish age or warn when a restored version is too new.
This also applies to transitive dependencies.
If you edit a package version in a project file yourself, restore doesn't reject it because of its age.

Floating versions, such as `1.2.*`, are different because restore selects a version for you.
Restore fails with [NU1020](../reference/errors-and-warnings/nu1020.md) when a floating package reference has an applicable source with a nonzero minimum publish age and the package isn't excepted.
If [Package Source Mapping](package-source-mapping.md) is enabled, this check considers only the sources mapped to that package.
Restore doesn't select an older version to satisfy the minimum age.

To keep the minimum age enabled, replace the floating reference with a non-floating version.
If your policy permits the package to bypass the delay, add a package exception instead.

### Publish date requirements

Automatic age filtering requires publish dates from the package source.
If a source has a nonzero minimum age and doesn't supply a publish date for a returned version of a package being evaluated, the command reports an error instead of assuming that version is old enough.
Resolve the source configuration or metadata problem before retrying.
Packages that match an exception don't require publish dates for age filtering.

For feeds that mirror public packages, check whether their publish dates represent the original publication or the time the package was copied.
The dates supplied by the feed determine when versions become eligible.

## Balance the delay with security updates

A minimum age can delay legitimate fixes as well as malicious releases.
In particular, `dotnet package update --vulnerable` doesn't automatically bypass the age requirement for a version that fixes a known vulnerability.
Choose whether to wait or adopt a specific release based on your risk assessment.

After you review a release, you can explicitly request its version without removing the source's minimum age.
A package exception is broader: it bypasses checks for all versions of that package until you remove it.
Continue to use [NuGet Audit](../concepts/Auditing-Packages.md) to identify known vulnerabilities.

External update tools, such as Dependabot, have their own settings.
Configure those tools separately rather than assuming they enforce NuGet's minimum publish age.
