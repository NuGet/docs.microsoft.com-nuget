---
title: NuGet CLI mirror command
description: Reference for the nuget.exe mirror command
author: JonDouglas
ms.author: jodou
ms.date: 01/18/2018
ms.topic: reference
---

# mirror command (NuGet CLI)

**Applies to:** package publishing &bullet; **Supported versions:** deprecated in 3.2+

Mirrors a package and its dependencies from the specified source repositories to the target repository.

> [!NOTE]
> NuGet.ServerExtensions.dll and NuGet-Signed.exe that previously supported this command in NuGet 2.x (by renaming NuGet-Signed.exe to nuget.exe) are no longer available for download. To use a command similar to this, try [NuGetMirror](https://www.nuget.org/packages/NuGetMirror/).

## Usage

```cli
nuget mirror <packageID | configFilePath> <listUrlTarget> <publishUrlTarget> [options]
```

where `<packageID>` is the package to mirror, or `<configFilePath>` identifies the `packages.config` file that lists the packages to mirror.

The `<listUrlTarget>` specifies the source repository, and `<publishUrlTarget>` specifies the target repository.

If your target repository is on `https://machine/repo` that's running [NuGet.Server](../../hosting-packages/nuget-server.md), the list and push urls will be `https://machine/repo/nuget` and `https://machine/repo/api/v2/package`, respectively.

## Options

- **`-ApiKey`**

  The API key for the target repository. If not present,  the one specified in the config file is used (`%AppData%\NuGet\NuGet.Config` (Windows) or `~/.nuget/NuGet/NuGet.Config` (Mac/Linux)).

- **`-Help`**

  Displays help information for the command.

- **`-NoHttpCache`**

  Prevents NuGet from using http cached packages. See [Managing the global packages and cache folders](../../consume-packages/managing-the-global-packages-and-cache-folders.md).

- **`-Noop`**

  Logs what would be done but does not perform the actions; assumes success for push operations.

- **`-PreRelease`**

  Includes prerelease packages in the mirroring operation.

- **`-Source`**

  A list of package sources to mirror. If no sources are specified, the ones defined in the config file (see ApiKey above) are used, defaulting to nuget.org if none are specified.

- **`-Timeout`**

  Specifies the timeout, in seconds, for pushing to a server. The default is 300 seconds (5 minutes).

- **`-Version`**

  The version of the package to install. If not specified, the latest version is mirrored.

Also see [Environment variables](cli-ref-environment-variables.md)

## Examples

```cli
nuget mirror packages.config  https://MyRepo/nuget https://MyRepo/api/v2/package -source https://nuget.org/api/v2 -apikey myApiKey -nohttpcache

nuget mirror Microsoft.AspNet.Mvc https://MyRepo/nuget https://MyRepo/api/v2/package -version 4.0.20505.0

nuget mirror Microsoft.Net.Http https://MyRepo/nuget https://MyRepo/api/v2/package -prerelease
```
