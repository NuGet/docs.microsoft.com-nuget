---
# required metadata

title: NuGet Add-BindingRedirect PowerShell Reference | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 6/1/2017
ms.topic: article
ms.prod: nuget
#ms.service:
ms.technology: null
ms.assetid: 90f4dcb0-6e5a-4948-8ea9-62e0d061d95a

# optional metadata

description: Reference for Add-BindingRedirect PowerShell command in the NuGet Package Manager Console in Visual Studio.
keywords: NuGet package manager console, NuGet Powershell commands, NuGet Powershell reference, Add-BindingRedirect
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

# Add-BindingRedirect

Examines all assemblies within the output path for a project and adds binding redirects to the appropriate configuration files. For additional details on what this means, see [Redirecting Assembly Versions](https://msdn.microsoft.com/library/7wd6ex19.aspx) on MSDN.

> [!NOTE]
> NuGet 1.2+ automatically runs this command when installing a package.

## Usage

```ps
Add-BindingRedirect [-ProjectName <string>]
```


### Parameters

|  Parameter   | Description    |
| --- | --- |
| ProjectName | Specifies the project to which to add binding redirects, defaulting to the default project. The -ProjectName switch itself is optional. |

# Common Parameters

`Install-Package` supports the following [common PowerShell parameters](http://go.microsoft.com/fwlink/?LinkID=113216):

- Debug
- ErrorAction
- ErrorVariable
- OutBuffer
- OutVariable
- PipelineVariable
- Verbose
- WarningAction
- WarningVariable

## Examples

```ps
Add-BindingRedirect MyProjectName
```
 