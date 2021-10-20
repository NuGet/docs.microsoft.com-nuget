---
title: NuGet Register-TabExpansion PowerShell Reference | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 12/07/2017
ms.topic: reference
ms.prod: nuget
ms.technology: null
description: Reference for Register-TabExpansion PowerShell command in the NuGet Package Manager Console in Visual Studio.
keywords: NuGet package manager console, NuGet Powershell commands, NuGet Powershell reference, Register-TabExpansion
ms.reviewer:
- karann-msft
- unniravindranathan
---

# Register-TabExpansion (Package Manager Console in Visual Studio)

*Available only within the [NuGet Package Manager Console](package-manager-console.md) in Visual Studio on Windows.*

Registers a tab expansion for the parameters of the specified command, such that when Tab is used when entering a command, the expanded values appear as available options for the parameter in question. Any previous expansions for the command are overwritten.

## Syntax

```ps
Register-TabExpansion [-Name] <String> [-Definition] <Object> [<CommonParameters>]
```

## Parameters

| Parameter | Description |
| --- | --- |
| Name | (Required) The command to which to register expansions. The -Name switch itself is optional. |
| Definition | (Required) An object describing the argument in the syntax `@{'<parameter>' = {'<value1>', '<value2>', ...}}` where `<parameter>` is the name of the parameter to modify and each `<value>` provides a specific expansion. Both single and double quotes are accepted. |

None of these parameters accept pipeline input or wildcard characters.

## Common Parameters

`Register-TabExpansion` supports the following [common PowerShell parameters](/powershell/module/microsoft.powershell.core/about/about_commonparameters): Debug, Error Action, ErrorVariable, OutBuffer, OutVariable, PipelineVariable, Verbose, WarningAction, and WarningVariable.

## Examples

Consider a solution that contains three projects names EventManager, Utilities, and SpecialParser. The developer frequently uses the `Update-Package` command at different times with each of those projects. She finds it convenient to have the `Update-Package` command provide auto-completion expansions for the `-ProjectName` argument so she doesn't need to type out a project name each time. 

The following command, then, registers those three project names as an expansion for the `-ProjectName` parameter:

```ps
Register-TabExpansion Update-Package @{'ProjectName' = {'EventManager', 'Utilities', 'SpecialParser'}}    
```

The developer can then type `Update-Package -ProjectName `, press Tab, and see the expansions offered as auto-completion options:

![Example of using Register-TabExpansion](media/Register-TabExpansion-Example.png)