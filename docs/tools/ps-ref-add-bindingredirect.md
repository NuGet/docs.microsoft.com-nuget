---
title: NuGet Add-BindingRedirect PowerShell Reference | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 12/07/2017
ms.topic: reference
ms.prod: nuget
ms.technology: null
description: Reference for Add-BindingRedirect PowerShell command in the NuGet Package Manager Console in Visual Studio.
keywords: NuGet package manager console, NuGet Powershell commands, NuGet Powershell reference, Add-BindingRedirect
ms.reviewer:
- karann-msft
- unniravindranathan
---

# Add-BindingRedirect (Package Manager Console in Visual Studio)

*Available only within the [NuGet Package Manager Console](package-manager-console.md) in Visual Studio on Windows.*

Examines all assemblies within the output path for a project and adds binding redirects to the application or web configuration file where necessary. This command is run automatically when installing a package.

For details on binding redirects and why they are used, see [Redirecting Assembly Versions](/dotnet/framework/configure-apps/redirect-assembly-versions) in the .NET documentation.

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

`Add-BindingRedirect` supports the following [common PowerShell parameters](/powershell/module/microsoft.powershell.core/about/about_commonparameters): Debug, Error Action, ErrorVariable, OutBuffer, OutVariable, PipelineVariable, Verbose, WarningAction, and WarningVariable.

## Examples

```ps
Add-BindingRedirect MyProject

Add-BindingRedirect -ProjectName MyProject
```