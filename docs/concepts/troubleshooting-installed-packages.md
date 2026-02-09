---
title: Troubleshooting Installed Packages
description: How to find which package source was used for individual packages
author: JonDouglas
ms.author: jodou
ms.date: 03/26/2021
ms.topic: troubleshooting-general
---

# Troubleshooting Installed Packages

Sometimes you might want to validate which source a specific package was installed from. Here are some ways you can check.

> [!Note]
> Some package sources support a concept known as upstream sources. For example, [Azure Artifacts upstream sources](/azure/devops/artifacts/concepts/upstream-sources). NuGet clients do not know whether a package came from an upstream source. Therefore, any logging of the package source will list the configured source, not the upstream source.

## `.nupkg.metadata` file in global packages folder

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
> NuGet writes the `.nupkg.metadata` file to the *global-packages* folder only. Projects using `packages.config` use a solution packages folder, which does not create a `.nupkg.metadata` file.

## Installed package log message

Starting from NuGet 5.9.0, NuGet outputs the package source in the restore message informing that a package was installed. For example:

```text
Installed Moq 4.16.1 from https://api.nuget.org/v3/index.json with content hash bw3R9q8cVNhWXNpnvWb0OGP4HadS4zvClq+T1zf7AF+tLY1haZ2AvbHidQekf4PDv1T40c6brZeT/V0IBq7cEQ==.
```

> [!Tip]
> This message is output at normal/informational verbosity. Visual Studio and the `dotnet` CLI default to minimal verbosity, so this message will not be visible by default. The `msbuild` and `nuget` CLI tools default to normal verbosity, so this message will be visible by default.

## HTTP log message

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

## Package signature log message

If the package being downloaded is signed, NuGet will validate the signature and will log the following message at detailed verbosity:

```text
PackageSignatureVerificationLog: PackageIdentity: Moq.4.16.1 Source: https://api.nuget.org/v3/index.json PackageSignatureValidity: True
```

This message will be reported whether the package was downloaded from an HTTP package source, or copied from a local package source. It will not be output if the package is already available in the *global-packages* folder or a fallback folder.

> [!Important]
> Due to [removal of trust of VeriSign CA](https://github.com/dotnet/announcements/issues/180) NuGet has disabled signed package verification on certain platforms, in certain versions of NuGet and the .NET SDK. Therefore, the same packages may have `PackageSignatureVerificationLog` logs, or those logs may be missing, depending on what platform you're running restore on, and which version of .NET or NuGet you're using.

## NuGet Version Map

The following versions of NuGet have important changes regarding package source logging:

|NuGet Version|Visual Studio Version|.NET SDK Version|
|---|---|---|
|NuGet 5.9.0|Visual Studio 2019 16.9.0|.NET 5 SDK 5.0.200|
