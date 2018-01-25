---
title: NuGet Get-Project PowerShell Reference | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 12/07/2017
ms.topic: reference
ms.prod: nuget
ms.technology: null
description: Reference for GetProject PowerShell command in the NuGet Package Manager Console in Visual Studio.
keywords: NuGet package manager console, NuGet Powershell commands, NuGet Powershell reference, Get-Project
ms.reviewer:
- karann-msft
- unniravindranathan
---

# Get-Project (Package Manager Console in Visual Studio)

*Available only within the [NuGet Package Manager Console](Package-Manager-Console.md) in Visual Studio on Windows.*

Displays information about the default or specified project. `Get-Project` specifically returns a referent to the Visual Studio DTE (Development Tools Environment) object for the project.

## Syntax

```ps
Get-Project [[-Name] <string>] [-All] [<CommonParameters>]
```

## Parameters

| Parameter | Description |
| --- | --- |
| Name | Specifies the project to display, defaulting to the default project selected in the Package Manager Console. The -Name switch is itself optional. |
| All | Displays information for every project in the solution; the order of projects is not deterministic. |

None of these parameters accept pipeline input or wildcard characters.

## Common Parameters

`Get-Project` supports the following [common PowerShell parameters](http://go.microsoft.com/fwlink/?LinkID=113216): Debug, Error Action, ErrorVariable, OutBuffer, OutVariable, PipelineVariable, Verbose, WarningAction, and WarningVariable.

## Examples

```ps
# Displays information for the default project
Get-Project

# Displays information for a project in the solution
Get-Project MyProjectName

    # Displays information for all projects in the solution
Get-Project -All
```