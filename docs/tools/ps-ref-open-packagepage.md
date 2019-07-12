---
title: NuGet Open-PackagePage PowerShell Reference
description: Reference for Open-PackagePage PowerShell command in the NuGet Package Manager Console in Visual Studio.
author: karann-msft
ms.author: karann
ms.date: 12/07/2017
ms.topic: reference
---

# Open-PackagePage (Package Manager Console in Visual Studio)

*Deprecated in 3.0+; available only within the [Package Manager Console](package-manager-console.md) in Visual Studio on Windows.*

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
| PassThru | Displays the URL; use with -WhatIf to suppress opening the browser. |

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
$url = Open-PackagePage Ninject -License -PassThru -WhatIf
```