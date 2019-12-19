---
title: NuGet Get-Project PowerShell Reference
description: Reference for GetProject PowerShell command in the NuGet Package Manager Console in Visual Studio.
author: karann-msft
ms.author: karann
ms.date: 12/07/2017
ms.topic: reference
---

# Get-Project (Package Manager Console in Visual Studio)

*Available only within the [Package Manager Console](../../consume-packages/install-use-packages-powershell.md) in Visual Studio on Windows.*

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

`Get-Project` supports the following [common PowerShell parameters](https://go.microsoft.com/fwlink/?LinkID=113216): Debug, Error Action, ErrorVariable, OutBuffer, OutVariable, PipelineVariable, Verbose, WarningAction, and WarningVariable.

## Examples

```ps
# Displays information for the default project
Get-Project

# Displays information for a project in the solution
Get-Project MyProjectName

    # Displays information for all projects in the solution
Get-Project -All
```