---
title: Package Source Mapping
description: Describes package source mapping functionality and how to onboard
author: nkolev92
ms.author: nikolev
ms.date: 10/15/2021
ms.topic: conceptual
---

# Package Source Mapping

Safeguarding your software supply chain is crucial if you use a mix of public and private package sources.
Use Package Source Mapping along side other [best practices](..\concepts\Security-Best-Practices.md) to help you fortify your supply chain against attacks.

Starting with [NuGet 6.0](..\release-notes\NuGet-6.0.md), you can centrally declare which source each package in your solution should restore from in your nuget.config file.

The feature is available across all NuGet integrated tooling.

* [Visual Studio 2022 and later](https://visualstudio.microsoft.com/downloads/)
* [.NET SDK 6.0.100 and later](https://dotnet.microsoft.com/download/dotnet/6.0)
* [nuget.exe 6.0.0 and later](https://www.nuget.org/downloads)

Older tooling will ignore the Package Source Mapping configuration. To use this feature, ensure all your build environments use compatible tooling versions.

Package Source Mappings will apply to all project types – including .NET Framework – as long as compatible tooling is used.

## Video walkthrough

For a video-based overview of the Package Source Mapping feature, consider watching the [Secure your NuGet packages with Package Source Mapping](https://www.youtube.com/watch?v=G6P38Dn69Ro) video on YouTube.

## Enabling Package Source Mapping

To opt into this feature, you must have a `nuget.config` file. Having a single `nuget.config` at the root of your repository is considered a best practice. See [nuget.config documentation](../reference/nuget-config-file.md) to learn more.

* Declare your desired package sources in your `nuget.config` file.
* Following your source declarations, add a `<packageSourceMapping>` element that specifies the desired mappings for each source.
* Declare exactly one `packageSource` element for each source in use.
  * Add as many patterns as you find necessary.

```xml
<!-- Define the package sources, nuget.org and contoso.com. -->
<!-- `clear` ensures no additional sources are inherited from another config file. -->
<packageSources>
  <clear />
  <!-- `key` can be any identifier for your source. -->
  <add key="nuget.org" value="https://api.nuget.org/v3/index.json" />
  <add key="contoso.com" value="https://contoso.com/packages/" />
</packageSources>

<!-- Define mappings by adding package patterns beneath the target source. -->
<!-- Contoso.* packages and NuGet.Common will be restored from contoso.com, everything else from nuget.org. -->
<packageSourceMapping>
  <!-- key value for <packageSource> should match key values from <packageSources> element -->
  <packageSource key="nuget.org">
    <package pattern="*" />
  </packageSource>
  <packageSource key="contoso.com">
    <package pattern="Contoso.*" />
    <package pattern="NuGet.Common" />
  </packageSource>
</packageSourceMapping>
```

Package Source Mapping settings are applied following [nuget.config precedence rules](configuring-nuget-behavior.md#how-settings-are-applied) when multiple `nuget.config` files at various levels (machine-level, user-level, repo-level) are present.

## Package Source Mapping rules

For maximum flexibility and control, NuGet requires that all packages match a package pattern through a well defined precedence.

### Package Pattern requirements

All requested packages must map to one or more sources by matching a defined package pattern. In other words, once you have defined a `packageSourceMapping` element you must explicitly define which sources *every* package - *including transitive packages* - will be restored from.

* Both top-level *and transitive* packages must match defined patterns. There is no requirement that a top level package and its dependencies come from the same source.  
* The same ID pattern can be defined on multiple sources, allowing matching package IDs to be restored from any of the feeds that define the pattern. However, this isn't recommended due to the impact on restore predictability (a given package could come from multiple sources). This may be a valid configuration if you trust all respective sources.

### Package Pattern Syntax

| Pattern | Example syntax | Description |
|-|--------|---------|-------------|
| Package prefix pattern | `*`, `NuGet.*`, `NuGet.*` | Must end with a `*`, where `*` matches 0 or more characters. `*` is the shortest allowed prefix pattern and matches all packages ids. |
| Package ID pattern | `NuGet.Common`, `Contoso.Contracts` | Exact package ID. |

### Package Pattern precedence

When multiple unique patterns match a package ID, the most specific one will be preferred. Package ID patterns always have the highest precedence while the generic `*` always has the lowest precedence. For package prefix patterns, the longest has precedence.

![Package Pattern Precedence Examples](media/Package-Pattern-Examples.png)

### Setting default sources

The `*` pattern can be used to make a declare a de-facto default source - meaning any package that doesn't match other specified patterns will be restored from that source without throwing an error.
This configuration is advantageous if you primarily use packages from say, `nuget.org`, and only have a few internal packages, or use standard prefixes for all internal packages like `Contoso.*`.

If your team doesn't use standard prefixes for internal package IDs or vets `nuget.org` packages prior to installation, then making a private source the default will suit your needs better.

> [!Note]
> When the requested package already exists in the global packages folder, no source look-up will happen and the mappings will be ignored. Consider declaring a [global packages folder for your repo](../reference/nuget-config-file.md#config-section) to gain the full security benefits of this feature. Work to improve the experience with the default global packages folder in planned for a next iteration.
To learn more about how package installation works, see [the conceptual document.](../concepts/package-installation-process.md)

### Get started

To fully onboard your repository you may take the following steps:

1. Declare a new [global packages folder for your repo](../reference/nuget-config-file.md#config-section).
1. Run [`dotnet list package --include-transitive`](/dotnet/core/tools/dotnet-list-package#synopsis) to view all top-level and transitive packages in your solution.
    * For .NET framework projects using [`packages.config`](../reference/packages-config.md), the `packages.config` file will have a flat list of all direct and transitive packages.
1. Define mappings such that every package ID in your solution - *including transitive packages* - matches a pattern for the target source.
1. Run restore to validate that you have configured your mappings correctly. If your mappings don't fully cover every package ID in your solution, the error messages will help you identify the issue.
1. When restore succeeds, you are done! Optionally consider:
    * Simplifying the configuration to fewer declarations by using broader package ID prefixes or [setting a default source](#setting-default-sources) where possible.
    * Verifying the source each package was restored from by checking the [metadata files in the global packages folder or reviewing the restore logs](https://devblogs.microsoft.com/nuget/performance-and-polish-with-nuget-5-9/).

For an idea of how your source mappings may look like, refer to our [samples repo](https://github.com/NuGet/Samples/tree/main/PackageSourceMappingExample).


> [!Note]
> This feature is in active development. We appreciate you trying it out and providing any feedback you may have at [NuGet/Home](https://github.com/nuget/home/issues).
>
> * There are no nuget.exe or dotnet.exe commands for managing the package source mapping configuration, see [NuGet/Home#10735](https://github.com/NuGet/Home/issues/10735).
> * There are no means of mapping packages at package installation time, see [NuGet/Home#10730](https://github.com/NuGet/Home/issues/10730).
> * There is a limitation when using the `DotNetCoreCLI@2` Azure Pipelines task which can be worked around by using `feed-` prefixes in your source mapping configuration. It is recommended however to use `NuGetAuthenticate` for your authentication needs and call the dotnet cli directly from a script task. See [microsoft/azure-pipelines-tasks#15542](https://github.com/microsoft/azure-pipelines-tasks/issues/15542).
