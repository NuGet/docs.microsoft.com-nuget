---
# required metadata

title: NuGet Get-Project PowerShell Reference | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 6/1/2017
ms.topic: article
ms.prod: nuget
#ms.service:
ms.technology: null
ms.assetid: 09c10ea3-ba26-4bfa-999e-de5350e6e920

# optional metadata

description: Reference for GetProject PowerShell command in the NuGet Package Manager Console in Visual Studio.
keywords: NuGet package manager console, NuGet Powershell commands, NuGet Powershell reference, Get-Project
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

# Get-Project

Displays information about the default or specified project.

## Usage

```ps
Get-Project [-Name <string>] [-All]
```

## Parameters

|  Parameter   | Description    |
| --- | --- |
Name | Specifies the project to display, defaulting to the default project. The -Name switch is itself optional.
All | Displays information for every project in the solution.

## Common Parameters

`Get-Project` supports the following [common PowerShell parameters](http://go.microsoft.com/fwlink/?LinkID=113216):

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
# Displays information for the default project
Get-Project

# Displays information for MyProjectName
Get-Project MyProjectName

    # Displays information for all projects in the solution
Get-Project -All
```