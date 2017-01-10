---
# required metadata

title: Managing the NuGet Cache | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 1/9/2017
ms.topic: article
ms.prod: nuget
#ms.service:
ms.technology: nuget
ms.assetid: 3932217d-780d-4bd1-ad15-767acd3e8870

# optional metadata

#description:
#keywords:
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


#Managing the NuGet cache

NuGet manages several local caches to avoid downloading packages that are already on the machine, and to provide offline support. NuGet versions 2.8 and later automatically fall back to the cache when installing or reinstalling packages without a network connection.

To see the list of cache locations, use the [locals command](../tools/nuget.exe-cli-reference.md#locals):

    nuget locals all -list

Typical output is as follows:

    http-cache: C:\Users\kraigb\AppData\Local\NuGet\v3-cache   #3.x cache
    packages-cache: C:\Users\kraigb\AppData\Local\NuGet\Cache  #2.x cache
    global-packages: C:\Users\kraigb\.nuget\packages\          #Global cache
    temp: C:\Users\kraigb\AppData\Local\Temp\NuGetScratch      #Temp folder

If you encounter package installation problems or otherwise want to ensure that you're installing packages from the remote gallery, use the `locals -clear` option in one or more of the following ways:

    nuget locals http-cache -clear        #Clear the 3.x cache
    nuget locals packages-cache -clear    #Clear the 2.x cache
    nuget locals global-packages -clear   #Clear the global cache
    nuget locals temp -clear              #Clear the temporary cache
    nuget locals all -clear               #Clears all caches

Note that managing the cache is presently supported only from the NuGet command line, and not within Visual Studio or through the Package Manager Console. Also, managing the 2.x cache is not supported in NuGet 3.6 and later.

The following errors can occur when using `nuget locals`:

* **Clearing local resources failed: Unable to delete one or more files**
* **The directory is not empty**

These indicate that you either do not have permission to delete files in the cache, or that one or more files in the cache are in use by another process, which must be closed before the those files can be removed.
