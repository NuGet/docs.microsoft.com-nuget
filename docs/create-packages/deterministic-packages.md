---
title: Create Deterministic NuGet packages
description: Guidance for creating deterministic packages
author: Omair Majid
ms.author: 
ms.date: 05/06/2026
ms.topic: concept-article
---

# Building deterministic packages

Starting in .NET 11, NuGet packages are deterministic (or reproducible) by default.

That means rebuilding the same package should generate an identical nupkg file.

The one known caveat is the modification time on files. NuGet packages contain various files. The file modification timestamp on those files is based on the timestamp of the original files on  disk, which may change due to several reasons even if the file contents are otherwise identical.

To make bit-identical NuGet packages, you can set the `DeterministicTimestamp` property to a known/fixed value. To reproduce an existing package, you can find the DeterministicTimestamp value used by that build and pass it to a new build that you want to do.

The well-known [`SOURCE_DATE_EPOCH`](https://reproducible-builds.org/docs/source-date-epoch/) environment variable can be set to activate the same behaviour as setting `DeterministicTimestamp`.

## Building deterministic packages

Building in deterministic mode is the default starting with .NET 11.

- Using the command line:

    ```
    dotnet pack
    ```

If you want to set the time based on the commit time of the latest Git commit, you can use something like this:

```
export DeterministicTimestamp=$(git log -1 --pretty=%ct)
echo $DeterministicTimestamp
dotnet pack
```

This will set the environment variable named DeterministicTimestamp to the git commit timestamp, which msbuild will automatically use. Reproducing the package is then a matter of running the same set of command again.

## Reproducing a package

If you want to reproduce a bit-identical package, find out the `DeterministicTimestamp` value used in the first build and then use it again:

```
dotnet pack -p:DeterministicTimestamp=<value from previous build>
```

## Disabling fixed timestamps

If you want to keep the original file modification timestamps in your NuGet packages, you can set `DeterministicTimestamp` to `false`.

- Using the command line:

    ```
    dotnet pack -p:DeterministicTimestamp=false
    ```

- Using the `.csproj` file:

    ```xml
    <PropertyGroup>
        <DeterministicTimestamp>false</DeterministicTimestamp>
    </PropertyGroup>
    ```

