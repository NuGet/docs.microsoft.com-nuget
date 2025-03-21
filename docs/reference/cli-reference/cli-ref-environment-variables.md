---
title: NuGet CLI environment variables
description: Reference for the nuget.exe environment variables
author: JonDouglas
ms.author: jodou
ms.date: 01/18/2018
ms.topic: reference
---

# NuGet CLI environment variables

The behavior of the nuget.exe CLI can be configured through a number of environment variables, which affect nuget.exe on computer-wide, user, or process levels. Environment variables always override any settings in `NuGet.Config` files, allowing build servers to change appropriate settings without modifying any files.

In general, options specified directly on the command line or in NuGet configuration files have precedence, but there are a few exceptions such as *FORCE_NUGET_EXE_INTERACTIVE*. If you find that nuget.exe behaves differently between different computers, an environment variable could be the cause. For example, Azure Web Apps Kudu (used during deployment) has *NUGET_XMLDOC_MODE* set to *skip* to speed up package restore performance and save disk space.

The NuGet CLI uses MSBuild to read the project files. All environment variables are available as [properties](/visualstudio/msbuild/msbuild-command-line-reference) during the MSBuild evaluation.
The list of properties documented in [NuGet pack and restore as MSBuild targets](../msbuild-targets.md#restore-properties) can also be set as environment variables.

| Variable | Description | Remarks |
| --- | --- | --- |
| http_proxy | Http proxy used for NuGet HTTP operations. | This would be specified as `http://<username>:<password>@proxy.com`. |
| no_proxy | Configures domains to bypass from using proxy. | Specified as domains separated by comma (,). |
| EnableNuGetPackageRestore | Flag for if NuGet should implicitly grant consent if that's required by package on restore. | Specified flag is treated as *true* or *1*, any other value treated as flag not set. |
| NUGET_CLI_LANGUAGE | Changes nuget.exe output language | Available in 6.5 and higher versions. Supported values are [supported Visual Studio languages](/visualstudio/install/use-command-line-parameters-to-install-visual-studio?view=vs-2022&preserve-view=true#list-of-language-locales) locale names: `zh-cn`, `zh-tw`, `cs-cz`, `en-us`, `es-es`, `fr-fr`, `de-de`, `it-it`, `ja-jp`, `ko-kr`, `pl-pl`, `pt-br`, `ru-ru`, and `tr-tr`. |
| NUGET_EXE_NO_PROMPT | Prevents the exe for prompting for credentials. | Any value except null or empty string will be treated as this flag set/true. |
| NUGET_MAX_RETRY_AFTER_DELAY_SECONDS | Maximum duration between retries when HTTP response contains Retry-After header | When NUGET_OBSERVE_RETRY_AFTER is enabled (as it is by default) and an HTTP response contains a Retry-After header, this environment variable sets the maximum duration that will be waited. Note that Retry-After can provide either a duration (delta) or a time stamp (date). If the server returns a time stamp and your machine's clock is out of sync with the server, the calculated duration might be very long (or negative). |
| FORCE_NUGET_EXE_INTERACTIVE | Global environment variable to force interactive mode. | Any value except null or empty string will be treated as this flag set/true. |
| NUGET_PACKAGES | Path to use for the *global-packages* folder as described on [Managing the global packages and cache folders](../../consume-packages/managing-the-global-packages-and-cache-folders.md). | Specified as absolute path. |
| NUGET_FALLBACK_PACKAGES | Global fallback packages folders. | Absolute folder paths separated by semicolon (;). |
| NUGET_HTTP_CACHE_PATH | Path to use for the *http-cache* folder as described on [Managing the global packages and cache folders](../../consume-packages/managing-the-global-packages-and-cache-folders.md). | Specified as absolute path. |
| NUGET_OBSERVE_RETRY_AFTER | Opt-out of using HTTP response message Retry-After delay durations | Prior to NuGet 6.5, NuGet would retry all failed requests with the same back-off strategy. Starting with NuGet 6.5, NuGet will by default use the Retry-After delay period provided by the server. Set this environment variable to `false` to ignore the Retry-After response header. Also see NUGET_MAX_RETRY_AFTER_DELAY_SECONDS |
| NUGET_RESTORE_MSBUILD_ARGS | Sets additional MSBuild arguments. | Pass arguments identical to how you would pass them to msbuild.exe. An example of setting a project property Foo from the command line to value Bar would be /p:Foo=Bar |
| NUGET_RETRY_HTTP_429 | Change retry behavior on HTTP 429 responses | Prior to NuGet 6.5, NuGet would not retry requests that result in HTTP 429 on 408. The default value is `true`, and this environment variable can be set to `false` to opt-in to older NuGet behaviour (don't retry the request) |
| NUGET_RESTORE_MSBUILD_VERBOSITY | Sets the MSBuild log verbosity. | Default is *quiet* ("/v:q"). Possible values *q[uiet]*, *m[inimal]*, *n[ormal]*, *d[etailed]*, and *diag[nostic]*. |
| NUGET_SHOW_STACK | Determines whether the full exception (including stack trace) should be displayed to the user. | Specified as *true* or *false* (default). |
| NUGET_UPDATEFILETIME_MAXRETRIES | Sets the number of times NuGet will attempt to retry seting the file timestamp when extracting packages when the first attempt failed. | On Windows anti-virus software might temporarily open files, preventing NuGet from changing the timestamp. NuGet uses an exponential back-off where the wait duration between attempts is `Math.Pow(2, retryNumber)`. The default max retries is 9, meaning the default total wait duration before failure will be approximately one second. |
| NUGET_XMLDOC_MODE | Determines how assemblies XML documentation file extraction should be handled. | Supported modes are *skip* (do not extract XML documentation files), *compress* (store XML doc files as a zip archive) or *none* (default, treat XML doc files as regular files). |
| NUGET_CERT_REVOCATION_MODE | Determines how the revocation status check of the certificate used to sign a package, is performed when a signed package is installed or restored. When not set, defaults to `online`.| Possible values *online* (default), *offline*.  Related to [NU3028](../errors-and-warnings/NU3028.md) |
| NUGET_ENABLE_ENHANCED_HTTP_RETRY | Enables or disables enhanced HTTP retry in NuGet. | Possible values are `true` (default) or `false`. |
| NUGET_ENHANCED_MAX_NETWORK_TRY_COUNT | Configures the maximum number of times an HTTP connection should be retried when enhanced retries are enabled. | A number representing how many retries to perform, the default value is `6`. |
| NUGET_ENHANCED_NETWORK_RETRY_DELAY_MILLISECONDS | Configures the amount of time to wait in milliseconds before retrying an HTTP connection when enhanced retries are enabled. | Number of millseconds to wait, the default value is `1000`. |
| NUGET_EXPERIMENTAL_USE_NJ_FOR_FILE_PARSING | Determines whether or not `Netwonsoft.Json` is used for parsing the assets file during restore | Possible values are `true` or `false` (default). |
