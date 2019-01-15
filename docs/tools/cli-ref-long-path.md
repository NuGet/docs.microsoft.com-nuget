---
title: NuGet CLI Long Path Support
description: Reference for nuget.exe long path support
author: zhili1208
ms.author: lzhi
ms.date: 07/12/2018
ms.topic: reference
---

# Long Path Support (NuGet CLI)

**Applies to:** all &bullet; **Supported versions:** 4.8+

NuGet.exe 4.8 and later support long paths for files and directories for scenarios like Pack, Restore, Install, and most other scenarios that need file paths.

## Required Operating System

-   Windows 10 (version 1607 or later)
-   Windows 10 (July 2015 release or version 1511) if you upgrade .NET Framework to versions 4.6.2 or later.
-   Windows Server 2016 (all versions)

## Enable "Win32 Long Paths" Group Policy

One needs to enable long path support on those systems by setting a group policy.

Steps:
1. Launch **Group Policy Editor** - Type "Edit group policy" in the Start search bar or Run "gpedit.msc" from the Run command (Windows-R).
2. In the **Local Group Policy Editor**, enable "Local Computer Policy/Computer Configuration/Administrative Templates/All Settings/Enable Win32 long paths".

![Long Path Policy](media/LongPathPolicy.png)


> [!Note]
> Enabling Other NuGet Tools to Support Long Paths
>
> -   Dotnet CLI supports long paths regardless of the operating system or version.
> -   Visual Studio or msbuild -t:restore does not yet support long paths.
> -   Software that uses NuGet Libraries to execute restore and other commands, will support long paths on the same systems that NuGet.exe works on, if they also set longPathAware in their windows manifest and configure UseLegacyPathHandling to false via App.Config [See more information](https://blogs.msdn.microsoft.com/jeremykuhne/2016/07/30/net-4-6-2-and-long-paths-on-windows-10/)

