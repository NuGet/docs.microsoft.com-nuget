---
# required metadata

title: NuGet CLI mirror command | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 10/24/2017
ms.topic: reference
ms.prod: nuget
ms.technology: null
ms.assetid: 190d7010-172e-44b8-8a32-94a2a63be4f3

# optional metadata

description: Reference for the nuget.exe mirror command
keywords: nuget mirror reference, mirror command
ms.reviewer:
- karann-msft
- unniravindranathan

---

# mirror command (NuGet CLI)

**Applies to:** package publishing &bullet; **Supported versions:** deprecated in 3.2+

Mirrors a package and its dependencies from the specified source repositories to the target repository.

> [!NOTE]
> To enable this command for NuGet versions before 3.2, go to [https://nuget.codeplex.com/releases](https://nuget.codeplex.com/releases), select the newest stable release, download `NuGet.ServerExtensions.dll` and `Nuget-Signed.exe` to your local disk and rename `Nuget-Signed.exe` to `nuget.exe`.

## Usage

```
nuget mirror <packageID | configFilePath> <listUrlTarget> <publishUrlTarget> [options]
```

where `<packageID>` is the package to mirror, or `<configFilePath>` identifies the `packages.config` file that lists the packages to mirror.

The `<listUrlTarget>` specifies the source repository, and `<publishUrlTarget>` specifies the target repository.

If your target repository is on `https://machine/repo` that's running [NuGet.Server](../hosting-packages/NuGet-Server.md), the list and push urls will be `https://machine/repo/nuget` and `https://machine/repo/api/v2/package`, respectively.

## Options

| Option | Description |
| --- | --- |
| ApiKey | The API key for the target repository. If not present,  the one specified in *%AppData%\NuGet\NuGet.Config* is used. |
| Help | Displays help information for the command. |
| NoCache | Prevents NuGet from using packages from local machine caches. |
| Noop | Logs what would be done but does not perform the actions; assumes success for push operations. |
| PreRelease | Includes prerelease packages in the mirroring operation. |
| Source | A list of package sources to mirror. If no sources are specified, the ones defined in *%AppData%\NuGet\NuGet.Config* are used, defaulting to nuget.org if none are specified. |
| Timeout | Specifies the timeout, in seconds, for pushing to a server. The default is 300 seconds (5 minutes). |
| Version | The version of the package to install. If not specified, the latest version is mirrored. |

Also see [Environment variables](cli-ref-environment-variables.md)

## Examples

```
nuget mirror packages.config  https://MyRepo/nuget https://MyRepo/api/v2/package -source https://nuget.org/api/v2 -apikey myApiKey -nocache

nuget mirror Microsoft.AspNet.Mvc https://MyRepo/nuget https://MyRepo/api/v2/package -version 4.0.20505.0

nuget mirror Microsoft.Net.Http https://MyRepo/nuget https://MyRepo/api/v2/package -prerelease
```
