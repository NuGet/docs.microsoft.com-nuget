---
# required metadata

title: NuGet Add-BindingRedirect PowerShell Reference | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 10/17/2017
ms.topic: reference
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

Examines all assemblies within the output path for a project and adds binding redirects to the application or web configuration file where necessary.

For details on binding redirects and why they are used, see [Redirecting Assembly Versions](https://docs.microsoft.com/dotnet/framework/configure-apps/redirect-assembly-versions) in the .NET documentation.

> [!Note]
> NuGet 1.2+ automatically runs this command when installing a package.

## Syntax

```ps
Add-BindingRedirect [-ProjectName] <string> [<CommonParameters>]
```

## Parameters

| Parameter | Description |
| --- | --- |
| ProjectName | (Required) The project to which to add binding redirects. The -ProjectName switch itself is optional. |

None of these parameters accept pipeline input or wildcard characters.

## Common Parameters

`Add-BindingRedirect` supports the following [common PowerShell parameters](http://go.microsoft.com/fwlink/?LinkID=113216): Debug, Error Action, ErrorVariable, OutBuffer, OutVariable, PipelineVariable, Verbose, WarningAction, and WarningVariable.

## Examples

```ps
Add-BindingRedirect MyProject

Add-BindingRedirect -ProjectName MyProject
```
 