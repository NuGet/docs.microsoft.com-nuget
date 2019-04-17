---
title: NuGet CLI environment variables
description: Reference for the nuget.exe environment variables
author: karann-msft
ms.author: karann
ms.date: 01/18/2018
ms.topic: reference
---

# NuGet CLI environment variables

The behavior of the nuget.exe CLI can be configured through a number of environment variables, which affect nuget.exe on computer-wide, user, or process levels. Environment variables always override any settings in `NuGet.Config` files, allowing build servers to change appropriate settings without modifying any files.

In general, options specified directly on the command line or in NuGet configuration files have precedence, but there are a few exceptions such as *FORCE_NUGET_EXE_INTERACTIVE*. If you find that nuget.exe behaves differently between different computers, an environment variable could be the cause. For example, Azure Web Apps Kudu (used during deployment) has *NUGET_XMLDOC_MODE* set to *skip* to speed up package restore performance and save disk space.

The NuGet CLI uses MSBuild to read the project files. All environment variables are available as [properties](/visualstudio/msbuild/msbuild-command-line-reference) during the MSBuild evaluation.
The list of properties documented in [NuGet pack and restore as MSBuild targets](../reference/msbuild-targets.md#restore-properties) can also be set as environment variables.

| Variable | Description | Remarks |
| --- | --- | --- |
| http_proxy | Http proxy used for NuGet HTTP operations. | This would be specified as `http://<username>:<password>@proxy.com`. |
| no_proxy | Configures domains to bypass from using proxy. | Specified as domains separated by comma (,). |
| EnableNuGetPackageRestore | Flag for if NuGet should implicitly grant consent if that's required by package on restore. | Specified flag is treated as *true* or *1*, any other value treated as flag not set. |
| NUGET_EXE_NO_PROMPT | Prevents the exe for prompting for credentials. | Any value except null or empty string will be treated as this flag set/true. |
| FORCE_NUGET_EXE_INTERACTIVE | Global environment variable to force interactive mode. | Any value except null or empty string will be treated as this flag set/true. |
| NUGET_PACKAGES | Path to use for the *global-packages* folder as described on [Managing the global packages and cache folders](../consume-packages/managing-the-global-packages-and-cache-folders.md). | Specified as absolute path. |
| NUGET_FALLBACK_PACKAGES | Global fallback packages folders. | Absolute folder paths separated by semicolon (;). |
| NUGET_HTTP_CACHE_PATH | Path to use for the *http-cache* folder as described on [Managing the global packages and cache folders](../consume-packages/managing-the-global-packages-and-cache-folders.md). | Specified as absolute path. |
| NUGET_PERSIST_DG | Flag indicating if dg files (data collected from MSBuild) should be persisted. | Specified as *true* or *false* (default), if NUGET_PERSIST_DG_PATH not set will be stored to temporary directory (NuGetScratch folder in current environment temp directory). |
| NUGET_PERSIST_DG_PATH | Path to persist dg files. | Specified as absolute path, this option is only used when *NUGET_PERSIST_DG* is set to true. |
| NUGET_RESTORE_MSBUILD_ARGS | Sets additional MSBuild arguments. | |
| NUGET_RESTORE_MSBUILD_VERBOSITY | Sets the MSBuild log verbosity. | Default is *quiet* ("/v:q"). Possible values *q[uiet]*, *m[inimal]*, *n[ormal]*, *d[etailed]*, and *diag[nostic]*. |
| NUGET_SHOW_STACK | Determines whether the full exception (including stack trace) should be displayed to the user. | Specified as *true* or *false* (default). |
| NUGET_XMLDOC_MODE | Determines how assemblies XML documentation file extraction should be handled. | Supported modes are *skip* (do not extract XML documentation files), *compress* (store XML doc files as a zip archive) or *none* (default, treat XML doc files as regular files). |
| NUGET_CERT_REVOCATION_MODE | Determines how the revocation status check of the certificate used to sign a package, is performed when a signed package is installed or restored. When not set, defaults to `online`.| Possible values *online* (default), *offline*.  Related to [NU3028](../reference/errors-and-warnings/NU3028.md) |

