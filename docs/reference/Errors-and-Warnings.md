---
title: NuGet Errors and Warnings Reference
description: Complete reference for warnings and errors issued from NuGet during various NuGet operations.
author: karann-msft
ms.author: karann
manager: unnir
ms.date: 05/18/2018
ms.topic: reference
ms.reviewer: anangaur
---

# Errors and warnings

In NuGet 4.3.0+, errors and warnings are numbered as described in this topic and provide detailed information to help you address the issues involved.

The errors and warnings listed here are available only with [PackageReference-based](../consume-packages/package-references-in-project-files.md) projects and NuGet 4.3.0+. NuGet also honors MSBuild properties to suppress warnings or elevate them to errors. For more information, see [How to: Suppress Compiler Warnings](/visualstudio/ide/how-to-suppress-compiler-warnings) in the Visual Studio documentation.

**Errors**

| Group | Error Numbers |
| --- | --- |
| Invalid input errors | [NU1001](./errors-and-warnings/NU1001.md), [NU1002](./errors-and-warnings/NU1002.md), [NU1003](./errors-and-warnings/NU1003.md) |
| Missing package and project errors | [NU1100](./errors-and-warnings/NU1100.md), [NU1101](./errors-and-warnings/NU1101.md), [NU1102](./errors-and-warnings/NU1102.md), [NU1103](./errors-and-warnings/NU1103.md), [NU1104](./errors-and-warnings/NU1104.md), [NU1105](./errors-and-warnings/NU1105.md), [NU1106](./errors-and-warnings/NU1106.md), [NU1107](./errors-and-warnings/NU1107.md) (previously NU1607), [NU1108](./errors-and-warnings/NU1108.md) (previously NU1606) |
| Compatibility errors | [NU1201](./errors-and-warnings/NU1201.md), [NU1202](./errors-and-warnings/NU1202.md), [NU1203](./errors-and-warnings/NU1203.md), [NU1401](./errors-and-warnings/NU1401.md) |
| NuGet internal errors | [NU1000](./errors-and-warnings/NU1000.md) |
| Signed packages errors(creation and verification) | [NU3000](./errors-and-warnings/NU3000.md), [NU3001](./errors-and-warnings/NU3001.md), [NU3004](./errors-and-warnings/NU3004.md), [NU3008](./errors-and-warnings/NU3008.md) |

**Warnings**

| Group | Warning numbers |
| --- | --- |
| Invalid input warnings | [NU1501](./errors-and-warnings/NU1501.md), [NU1502](./errors-and-warnings/NU1502.md), [NU1503](./errors-and-warnings/NU1503.md) |
| Unexpected package version warnings | [NU1601](./errors-and-warnings/NU1601.md), [NU1602](./errors-and-warnings/NU1602.md), [NU1603](./errors-and-warnings/NU1603.md), [NU1604](./errors-and-warnings/NU1604.md), [NU1605](./errors-and-warnings/NU1605.md) |
| Resolver conflict warnings | [NU1608](./errors-and-warnings/NU1608.md) |
| Package fallback warnings | [NU1701](./errors-and-warnings/NU1701.md) |
| Feed warnings | [NU1801](./errors-and-warnings/NU1801.md) |
| NuGet internal warnings | [NU1500](./errors-and-warnings/NU1500.md) |
| Signed packages warnings(creation and verification) | [NU3002](./errors-and-warnings/NU3002.md), [NU3018](./errors-and-warnings/NU3018.md), [NU3028](./errors-and-warnings/NU3028.md) |
