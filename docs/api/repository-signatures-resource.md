---
title: Repository Signatures, NuGet API | Microsoft Docs
author: joelverhagen
ms.author: jver
ms.date: 3/2/2018
ms.topic: reference
description: The repository signatures resource allows clients package sources to announce their repository signing capabilities.
ms.reviewer: 
  - karann
  - unniravindranathan
---

# Repository signatures

If a package source supports adding repository signatures to published packages, it is possible for a client to
determine the signing certificates that are used by the package source. This resource allows clients to detect
whether a repository signed package has been tampered or has an unexpected signing certificate.

The resource used for fetching this repository signature information is the `RepositorySignatures` resource found in
the [service index](service-index.md).

## Versioning

The following `@type` value is used:

@type value                | Notes
-------------------------- | -----
RepositorySignatures/4.7.0 | The initial release
RepositorySignatures/4.9.0 | Supported by NuGet v4.9+ clients
RepositorySignatures/5.0.0 | Allows enabling `allRepositorySigned`. Supported by NuGet v5.0+ clients

## Base URL

The entry point URL for the following APIs is the value of the `@id` property associated with the aforementioned
resource `@type` value. This topic uses the placeholder URL `{@id}`.

Note that unlike other resources, the `{@id}` URL is required to be served over HTTPS.

## HTTP methods

All URLs found in the repository signatures resource support only the HTTP methods `GET` and `HEAD`.

## Repository signatures index

The repository signatures index contains two pieces of information:

1. Whether or not all packages found on the source are repository signed by this package source.
1. The list of certificates used by the package source to sign packages.

In most cases, the list of certificates will only ever be appended to. New certificates would be added to the list when
the previous signing certificate has expired and the package source needs to start using a new signing certificate. If
a certificate is removed from the list, that means that all package signatures created with the removed signing
certificate should no longer be considered valid by the client. In this case, the package signature (but not
necessarily the package) is invalid. A client policy may allow installing the package as unsigned.

In the case of certificate revocation (e.g. key compromise), the package source is expected to re-sign all packages signed by the affected certificate. Additionally, the package source should remove the affected certificate from the
signing certificate list.

The following request fetches the repository signatures index.

```
GET {@id}
```

The repository signature index is a JSON document that contains an object with the following properties:

Name                | Type             | Required | Notes
------------------- | ---------------- | -------- | -----
allRepositorySigned | boolean          | yes      | Must be `false` on 4.7.0 and 4.9.0 resources
signingCertificates | array of objects | yes      | 

The `allRepositorySigned` boolean is set to false if the package source has some packages that have no repository
signature. If the boolean is set to true, all packages available on the source must have a repository
signature produced by one of the signing certificates mentioned in `signingCertificates`.

> [!Warning]
> The `allRepositorySigned` boolean must be false on the 4.7.0 and 4.9.0 resources. NuGet v4.7, v4.8, and v4.9 clients cannot
> install packages from sources that have `allRepositorySigned` set to true.

There should be one or more signing certificates in the `signingCertificates` array if the `allRepositorySigned` boolean
is set to true. If the array is empty and `allRepositorySigned` is set to true, all packages from the source should be
considered invalid, although a client policy may still allow consumption of packages. Each element in this array is a
JSON object with the following properties.

Name         | Type   | Required | Notes
------------ | ------ | -------- | -----
contentUrl   | string | yes      | Absolute URL to the DER-encoded public certificate
fingerprints | object | yes      |
subject      | string | yes      | The subject distinguished name from the certificate
issuer       | string | yes      | The distinguished name of the certificate's issuer
notBefore    | string | yes      | The starting timestamp of the certificate's validity period
notAfter     | string | yes      | The ending timestamp of the certificate's validity period

Note that the `contentUrl` is required to be served over HTTPS. This URL has no specific URL pattern and must be
dynamically discovered using this repository signatures index document. 

All properties in this object (aside from `contentUrl`) must be derivable from the certificate found at `contentUrl`.
These derivable properties are provided as a convenience to minimize round trips.

The `fingerprints` object has the following properties:

Name                   | Type   | Required | Notes
---------------------- | ------ | -------- | -----
2.16.840.1.101.3.4.2.1 | string | yes      | The SHA-256 fingerprint

The key name `2.16.840.1.101.3.4.2.1` is the OID of the SHA-256 hash algorithm.

All hash values must be lowercase, hex-encoded string representations of the hash digest.

### Sample request

```
GET https://api.nuget.org/v3-index/repository-signatures/4.7.0/index.json
```

### Sample response

[!code-JSON [repository-signatures-index.json](./_data/repository-signatures-index.json)]
