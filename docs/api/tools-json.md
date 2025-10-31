---
title: tools.json for discovering nuget.exe versions
description: The endpoint for
author: jver
ms.author: jver
ms.date: 08/16/2018
ms.topic: article
ms.reviewer: kraigb
---

# tools.json for discovering nuget.exe versions

Today, there are a few ways to get the latest version of nuget.exe on your machine in a scriptable fashion. For example,
you can download and extract the [`NuGet.CommandLine`](https://www.nuget.org/packages/NuGet.CommandLine/) package from
nuget.org. This has some complexity since it either requires that you already have nuget.exe (for `nuget.exe install`)
or you have to unzip the .nupkg using a basic unzip tool and find the binary inside.

If you already have nuget.exe, you can also use `nuget.exe update -self`, however this also requires having an existing
copy of nuget.exe. This approach also updates you to the latest version. It does not allow the use of a specific
version.

The `tools.json` endpoint is available to both solve the bootstrapping problem and to give control of the version of
nuget.exe that you download. This can be used in CI/CD environments or in custom scripts to discover and download any
released version of nuget.exe.

The `tools.json` endpoint can be fetched using an unauthenticated HTTP request (e.g. `Invoke-WebRequest` in PowerShell
or `wget`). It can be parsed using a JSON deserializer and subsequent nuget.exe download URLs can also be fetched using
unauthenticated HTTP requests.

The endpoint can be fetched using the `GET` method:

```
GET https://dist.nuget.org/tools.json
```

The [JSON schema](https://json-schema.org/) for the endpoint is available here:

```
GET https://dist.nuget.org/tools.schema.json
```

## Response

The response is a JSON document containing all of the available versions of nuget.exe.

The root JSON object has the following property:

Name      | Type             | Required
--------- | ---------------- | --------
nuget.exe | array of objects | yes

Each object in the `nuget.exe` array has the following properties:

Name     | Type   | Required | Notes
-------- | ------ | -------- | -----
version  | string | yes      | A SemVer 2.0.0 string
url      | string | yes      | An absolute URL for downloading this version of nuget.exe
stage    | string | yes      | An enum string
uploaded | string | yes      | An approximate ISO 8601 timestamp of when the version was made available

The items in the array will be sorted in descending, SemVer 2.0.0 order. This guarantee is meant to reduce the burden
of a client that is interested in highest version number. However this does mean that the list is not sorted in
chronological order. For example, if a lower major version is serviced at a date later than a higher major version, 
this serviced version will not appear at the top of the list. If you want the latest version released by
*timestamp*, simply sort the array by the `uploaded` string. This works because the `uploaded` timestamp is in the
[ISO 8601](https://www.iso.org/iso-8601-date-and-time-format.html) format which can be sorted chronologically by
using a lexicographical sort (i.e. a simple string sort).

The `stage` property indicates how vetted this version of the tool is. 

Stage              | Meaning
------------------ | ------
EarlyAccessPreview | Not yet visible on the [download web page](https://www.nuget.org/downloads) and should be validated by partners
Released           | Available on the download site but is not yet recommended for wide-spread consumption
ReleasedAndBlessed | Available on the download site and is recommended for consumption

One simple approach for having the latest, recommended version is to take the first version in the list that has the
`stage` value of `ReleasedAndBlessed`. This works because the versions are sorted in SemVer 2.0.0 order.

The `NuGet.CommandLine` package on nuget.org is typically only updated with `ReleasedAndBlessed` versions.

### Sample request

```
GET https://dist.nuget.org/tools.json
```

### Sample response

[!code-JSON [tools-json.json](./_data/tools-json.json)]
