---
title: What happens when a package is installed?
description: Detailed information about the package installation process
author: JonDouglas
ms.author: jodou
ms.date: 06/20/2019
ms.topic: conceptual
---

# What happens when a NuGet package is installed?

Simply said, the different NuGet tools typically create a reference to a package in the project file or `packages.config`, then perform a package restore, which effectively installs the package. The exception is `nuget install`, which only expands the package into a `packages` folder and does not modify any other files.

The general process is as follows:

1. (All tools except `nuget.exe`) Record the package identifier and version into the project file or `packages.config`.

   If the installation tool is Visual Studio or the dotnet CLI, the tool first attempts to install the package. If it's incompatible, the package is not added to the project file or `packages.config`.

2. Acquire the package:
   - Check if the package (by exact identifer and version number) is already installed in the *global-packages* folder as described on [Managing the global packages and cache folders](../consume-packages/managing-the-global-packages-and-cache-folders.md).

   - If the package is not in the *global-packages* folder, attempt to retrieve it from the sources listed in the [configuration files](../consume-packages/Configuring-NuGet-Behavior.md). For online sources, attempt first to retrieve the package from the HTTP cache unless `-NoCache` is specified with `nuget.exe` commands or `--no-cache` is specified with `dotnet restore`. (Visual Studio and `dotnet add package` always use the cache.) If a package is used from the cache, "CACHE" appears in the output. The cache has an expiration time of 30 minutes.

   - If the package has been specified using a [floating version](../consume-packages/Package-References-in-Project-Files.md#floating-versions), or without a minimum version, NuGet *will* contact all sources to figure out the best match.
   Example: `1.*`, `(, 2.0.0]`.

   - If the package is not in the HTTP cache, attempt to download it from the sources listed in the configuration. If a package is downloaded, "GET" and "OK" appear in the output. NuGet logs http traffic on normal verbosity.

   - If the package cannot be successfully acquired from any sources, installation fails at this point with an error such as [NU1103](../reference/errors-and-warnings/NU1103.md). Note that errors from `nuget.exe` commands show only the last source checked, but implies that the package wasn't available from any source.

   When acquiring the package, the order of sources in the NuGet configuration may apply:

   - NuGet checks sources local folder and network shares before checking HTTP sources.

3. Save a copy of the package and other information in the *http-cache* folder as described on [Managing the global packages and cache folders](../consume-packages/managing-the-global-packages-and-cache-folders.md).

4. If downloaded, install the package into the per-user *global-packages* folder. NuGet creates a subfolder for each package identifier, then creates subfolders for each installed version of the package.

5. NuGet installs package dependencies as required. This process might update package versions in the process, as described in [Dependency Resolution](../concepts/dependency-resolution.md).

6. Update other project files and folders:

    - For projects using PackageReference, update the package dependency graph stored in `obj/project.assets.json`. Package contents themselves are not copied into any project folder.
    - Update `app.config` and/or `web.config` if the package uses [source and config file transformations](../create-packages/source-and-config-file-transformations.md).

7. (Visual Studio only) Display the package's readme file, if available, in a Visual Studio window.

Enjoy your productive coding with NuGet packages!

## Validating source of installed packages

Sometimes you might want to validate which source a specific package was installed from. Here are some ways you can check.

### `.nupkg.metadata` file in global packages folder

When a package is extracted into the *global-packages* folder, a file `.nupkg.metadata` is written. Starting from NuGet 5.9.0, NuGet will add the package source. See below to map NuGet versions to Visual Studio or .NET SDK versions. For example:

```json
{
  "version": 2,
  "contentHash": "bw3R9q8cVNhWXNpnvWb0OGP4HadS4zvClq+T1zf7AF+tLY1haZ2AvbHidQekf4PDv1T40c6brZeT/V0IBq7cEQ==",
  "source": "https://api.nuget.org/v3/index.json"
}
```

> [!Note]
> If your *global-packages* folder has packages extracted before you upgraded to a newer version of tools that has NuGet 5.9.0, the `.nupkg.metadata` file will be version 1 and will not contain the package source. You can [clear your *global-packages* folder](../consume-packages/managing-the-global-packages-and-cache-folders.md#clearing-local-folders) to ensure all packages will contain the package source.

> [!Tip]
> NuGet writes the `.nupkg.metadata` file to the *global-packages* folder only. Projects using `packages.config` use a solution packages folder, which does not create a `.nupkg.metadata` file. Unless `-NoCache` or `--no-cache` was used on the command line, the package will be in both the *global-packages* and solution packages folders.

### Installed package log message

Starting from NuGet 5.9.0, NuGet outputs the package source in the restore message informing that a package was installed. For example:

```text
Installed Moq 4.16.1 from https://api.nuget.org/v3/index.json with content hash bw3R9q8cVNhWXNpnvWb0OGP4HadS4zvClq+T1zf7AF+tLY1haZ2AvbHidQekf4PDv1T40c6brZeT/V0IBq7cEQ==.
```

> [!Tip]
> This message is output at normal/informational verbosity. Visual Studio and the `dotnet` CLI default to minimal verbosity, so this message will not be visible by default. The `msbuild` and `nuget` CLI tools default to normal verbosity, so this message will be visible by default.

### HTTP log message

When a package is not available locally, either in the *global-packages* folder, a fallback folder, or a local file source, NuGet will download it from any configured package source over HTTP. HTTP requests and responses are logged at the normal verbosity level, and you should see only a single request and response per package version. For example:

```text
info :   GET https://api.nuget.org/v3-flatcontainer/moq/index.json
info :   OK https://api.nuget.org/v3-flatcontainer/moq/index.json 56ms
info :   GET https://api.nuget.org/v3-flatcontainer/moq/4.16.1/moq.4.16.1.nupkg
info :   OK https://api.nuget.org/v3-flatcontainer/moq/4.16.1/moq.4.16.1.nupkg 3ms
```

If the files were recently downloaded, they might be retrieved from NuGet's *http-cache*

```text
CACHE https://api.nuget.org/v3-flatcontainer/moq/index.json
CACHE https://api.nuget.org/v3-flatcontainer/moq/4.16.1/moq.4.16.1.nupkg
```

The URL format may be different for different NuGet HTTP server implementations, and whether it's implementing NuGet V2 or V3 HTTP protocol.

If your `nuget.config` has multiple HTTP sources defined, you will see multiple requests to each package's `index.json` file, one for each source. But there will be only a single `nupkg` download for each version of the package.

### Package signature log message

If the package being downloaded is signed, NuGet will validate the signature and will log the following message at detailed verbosity:

```text
PackageSignatureVerificationLog: PackageIdentity: Moq.4.16.1 Source: https://api.nuget.org/v3/index.json PackageSignatureValidity: True
```

This message will be reported whether the package was downloaded from an HTTP package source, or copied from a local package source. It will not be output if the package is already available in the *global-packages* folder or a fallback folder.

### NuGet Version Map

The following versions of NuGet have important changes regarding package source logging:

|NuGet Version|Visual Studio Version|.NET SDK Version|
|---|---|---|
|NuGet 5.9.0|Visual Studio 2019 16.9.0|.NET 5 SDK 5.0.200|
