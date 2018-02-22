---
title: How to Create a Signed NuGet Package | Microsoft Docs
author: rido-min
ms.author: rido-min
manager: unniravindranathan
ms.date: 02/23/2018
ms.topic: article
ms.prod: nuget
ms.technology: null
description: Explains how signed packages can be used to enable content integrity verification
keywords: NuGet package signing, NuGet security, creating signed packages
ms.reviewer:
- karann-msft
- anangaur
---

# Signed packages

NuGet packages can include a digital signature which provides protection against tampered content. This signature is produced from an x509 certificate that also adds authenticity proofs to the actual origin of the package. 

This feature has been introduced in nuget.exe 4.6, while signature verification requires Visual Studio 2017 Update 15.6


> [!Warning] 
> NuGet.org does not accept signed packages yet. This feature will be announced using the [announcements](https://github.com/NuGet/Announcements) github repository.

## Types of signed packages

NuGet supports two types of signatures:

- **Author Signatures** are created by the package author before the package is submitted to any repository. These signatures provide validation end to end, from authors to consumers independent from of the repository from which the package has been obtained.
- **Repository Signatures** provide an integrity guarantee for all packages in a repository whether they are author signed or not, even if those packages are obtained from a different location than the original repository where they were signed.

There are four allowed package signing configurations:

|Package Type|Notes    |
|------------|---------|
|Unsigned|The package does not include any signature.|
|Author|The primary signature is an author signature.|
|Repo|The primary signature is a repository signature.|
|Author and Repo|The primary signature is an author signature, and the signature is repository countersigned.|

> [!Note] 
> In general, package repositories can host packages of any of these four types. Once nuget.org implements repository signatures, however, it will support only *Repo* and *Author and Repo* types.

The technical specification for NuGet package signatures can be found in [Package Signatures Technical Details](https://github.com/NuGet/Home/wiki/Package-Signatures-Technical-Details). 

### Benefits of author-signed packages

Packages that have been signed with an author certificate provide the strongest end to end validation. An author signature guarantees that the package has not been modified since it left the author computer, no matter from which repository or transport method has been used

Consumers who demand a locked-down environment can require packages signed with an specific author certificate.

Additionally, author-signed packages provide an extra authentication mechanism to the publishing mechanism to nuget.org, because the certificate used to sign must be registered ahead of time, as described in the [Registering Certificates](https://github.com/NuGet/Home/wiki/Register-package-signing-certificates) specification.

### Benefits of repository-signed packages

Package repositories that implement repository signatures will provide a signed version of all packages, no matter if the package is author-signed or not. This capability allows NuGet clients to verify the complete dependency graph even for cases where a signed package has a dependency on an unsigned package. 

Packages with the repository signature offer content integrity and proof of origin, even when the package is found in a different mirror/repository.

# How to author-sign a package

NuGet package authors who want to sign a package must obtain a X.509 code signing certificate. It’s strongly recommended to obtain the certificate from a public certification authority. The author can also use a certificate from a private certification authority but these signatures will generate a warning if the certificate is not trusted in the computer where the validation will occur.

## Certificate requirements

Authors require a code signing certificate, which are a special type of certificate that are valid for the `id-kp-codeSigning` purpose [[RFC 5280 section 4.2.1.12](https://tools.ietf.org/html/rfc5280#section-4.2.1.12)], and have a RSA public key length of 2048 bits or higher.

These certificates can be obtained from a public certificate authorities like Symmantec.com, DigiCert.com, GlobalSign.com or Certum.pl 
> [!Tip]
> Certum has special offers for [open source projects](https://www.certum.eu/certum/cert,offer_en_open_source_cs.xml).

## Signing with nuget.exe

NuGet packages can be signed with `nuget.exe` version 4.6 or higher using the `sign` command. Typically this command will require the following input parameters:
- **Certificate** to sign with. It can be specified from a PFX file, or from a certificate already installed in the certificate store.
- **Package** to be signed. It's the nupkg file produced with the `pack` command. If it's already signed it will produce an error unless the `-overwrite` option is used.
- **Timestamp** server. Optionally you can specify the URL of a timestamp server who implements [RFC 3161](https://tools.ietf.org/html/rfc3161).

```
nuget sign MyPackage.nupkg -CertificateSubjectName <MyCertSubjectName> -Timestamper http://timestamp.digicert.com 
```

In this example we are using the digicert.com free timestamp service, but you can use any other service that satisfies the timestamp requirements defined [here](https://github.com/NuGet/Home/wiki/Package-Signatures-Technical-Details).

For more information see nuget sign

> [!Note]
> Signed packages can be verified with the `verify -signatures` command.

# How to repository-sign a package

Repositories that want to add repository signatures to their packages should implement the requirements specified in the [Repository Signatures](https://github.com/NuGet/Home/wiki/Repository-Signatures) specification. The signing can be done using the NuGet api.

# How to consume signed packages

Visual Studio 2017 update 15.6 and later verifies signed packages with author or repository signatures, and will block the package installation/restore operation if the signature cannot be validated. 

The complete list of errors and warnings caused by signed related issues can be found in the [NuGet errors and warning reference](https://docs.microsoft.com/en-us/nuget/reference/errors-and-warnings).


