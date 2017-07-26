---
# required metadata

title: How to manage package caching in NuGet | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 7/26/2017
ms.topic: article
ms.prod: nuget
#ms.service:
ms.technology: null
ms.assetid: 3932217d-780d-4bd1-ad15-767acd3e8870

# optional metadata

description: How to manage the different NuGet package caches that exist on a machine, which are used when installing or restoring packages.
keywords: NuGet package cache, package caching, NuGet caches, managing caches, local NuGet cache, global NuGet cache, NuGet locals command, clearing a cache
#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer:
- karann
- unnir
#ms.suite:
#ms.tgt_pltfrm:
#ms.custom:

---

# Managing the NuGet cache

NuGet manages several local caches to avoid downloading packages that are already on the computer, and to provide offline support. NuGet 2.8+ automatically falls back to the cache when installing or reinstalling packages without a network connection.

Cache locations are available using the [locals command](../tools/nuget-exe-cli-reference.md#locals):

```
nuget locals all -list
```

Typical output is as follows:

    http-cache: C:\Users\user\AppData\Local\NuGet\v3-cache   #NuGet 3.x+ cache
    packages-cache: C:\Users\user\AppData\Local\NuGet\Cache  #NuGet 2.x cache
    global-packages: C:\Users\user\.nuget\packages\          #Global cache
    temp: C:\Users\user\AppData\Local\Temp\NuGetScratch      #Temp folder

If you encounter package installation problems or otherwise want to ensure that you're installing packages from a remote gallery, use the `locals -clear` option:

```
nuget locals http-cache -clear        #Clear the 3.x+ cache
nuget locals packages-cache -clear    #Clear the 2.x cache
nuget locals global-packages -clear   #Clear the global cache
nuget locals temp -clear              #Clear the temporary cache
nuget locals all -clear               #Clear all caches
```

Note that managing the cache is presently supported only from the NuGet command line, and not within Visual Studio or through the Package Manager Console. Also, managing the 2.x cache is not supported in NuGet 3.6 and later.

The following errors can occur when using `nuget locals`:

* **Clearing local resources failed: Unable to delete one or more files**
* **The directory is not empty**

These indicate that you either do not have permission to delete files in the cache, or that one or more files in the cache are in use by another process, which must be closed before the those files can be removed.
