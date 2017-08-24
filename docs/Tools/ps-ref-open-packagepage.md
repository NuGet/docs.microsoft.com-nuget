---
# required metadata

title: NuGet Open-PackagePage PowerShell Reference | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 6/1/2017
ms.topic: reference
ms.prod: nuget
#ms.service:
ms.technology: null
ms.assetid: e9f84530-6b3d-43b0-a832-0acb2997f6fc

# optional metadata

description: Reference for Open-PackagePage PowerShell command in the NuGet Package Manager Console in Visual Studio.
keywords: NuGet package manager console, NuGet Powershell commands, NuGet Powershell reference, Open-PackagePage
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

# Open-PackagePage

*Deprecated in 3.0+*

Launches the default browser with the project, license, or report abuse URL for the specified package.

## Syntax

```ps
Open-PackagePage [-Id] <string> [-Version] [-Source] [-License] [-ReportAbuse]
    [-PassThru] [<CommonParameters>]
```

## Parameters

| Parameter | Description |
| --- | --- |
| Id | The package ID of the desired package. The -Id switch itself is optional. |
| Version | The version of the package, defaulting to the latest version. |
| Source | The package source, defaulting to the selected source in the source drop-down. |
| License | Opens the browser to the package's License URL. If neither -License nor -ReportAbuse is specified, the browser opens the package's Project URL. |
| ReportAbuse | Opens the browser to the package's Report Abuse URL. If neither -License nor -ReportAbuse is specified, the browser opens the package's Project URL. |
| PassThru | Displays the URL but does not open it in the browser. |

None of these parameters accept pipeline input or wildcard characters.

## Common Parameters

`Open-PackagePage` supports the following [common PowerShell parameters](http://go.microsoft.com/fwlink/?LinkID=113216): Debug, Error Action, ErrorVariable, OutBuffer, OutVariable, PipelineVariable, Verbose, WarningAction, and WarningVariable.

## Examples

```ps
# Opens a browser with the Ninject package's project page
Open-PackagePage Ninject

# Opens a browser with the Ninject package's license page
Open-PackagePage Ninject -License

# Opens a browser with the Ninject package's report abuse page  
Open-PackagePage Ninject -ReportAbuse

# Assigns the license URL to the variable, $url, without launching the browser
$url = Open-PackagePage Ninject -License -PassThru
```
  