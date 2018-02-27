---
title: NuGet CLI sign command | Microsoft Docs
author: rido-min
ms.author: rido-min
manager: unniravindranathan
ms.date: 02/26/2018
ms.topic: reference
ms.prod: nuget
ms.technology: null
description: Reference for the nuget.exe sign command
keywords: nuget sign reference, sign command
ms.reviewer:
- karann-msft
- unniravindranathan
---

# sign command (NuGet CLI)

**Applies to:** package creation &bullet; **Supported versions:** 4.6+

Signs all the packages matching the first argument with a certificate. The certificate with the private key can be obtained from a file with a password or from an installed certificate in the cert store providing a subject name or a thumbprint.

## Usage 

```cli
nuget sign <package(s)> -certificateSubjectName <certSubjectName> -timestamper <TimestampServiceURL> 
```

## Options

