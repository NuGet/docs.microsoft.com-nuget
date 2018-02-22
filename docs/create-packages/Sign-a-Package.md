---
title: How to Create a Localized NuGet Package | Microsoft Docs
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

> [!Warning] 
> NuGet.org does not accept signed packages yet. This feature will be announced using the [announcements](https://github.com/NuGet/Announcements) github repository.

## Types of signed packages

NuGet supports two types of signatures:

- **Author Signatures** are created by the package author before the package is being submitted to any repository. These signatures provide validation end to end, from authors to consumers independent from which repository the package has been obtained.
- **Repository Signatures** provide an integrity guarantee for all packages in a repository whether they are author signed or not, even if those packages are obtained from a different location than the original repository where they were signed.

Considering author and repository signatures there are four possible package signing configurations; no other configurations of these signature types are allowed:

|Package Type|Notes    |
|------------|---------|
|Unsigned|The package does not include any signature.|
|Author|The primary signature is an author signature.|
|Repo|The primary signature is a repository signature.|
|Author and Repo|The primary signature is an author signature, and the signature is repository countersigned.|

> [!Note] 
> Package repositories can host packages of any of these four types, however once NuGet.org implements repository signatures, all packages in NuGet.org will fall in the last two types.

The technical specification for NuGet package signatures can be found in https://github.com/NuGet/Home/wiki/Package-Signatures-Technical-Details. 

### Benefits of author-signed packages

Packages that have been signed with an author certificate provide the strongest end to end validation, since the author signatures guarnatees that the package has not been modified since it leaves the author machine, until it is downloaded into the consumer machine, no matter fro which repository or transport method has been used.

Consumers who demand a locked-down environment can require packages signed with an specific author certificate.

Additionally, it provides an extra authentication mechanism to the publishing mechanism to NuGet.org, since the certificate used to sign must be registered before, as described in the [Registering Certificates](https://github.com/NuGet/Home/wiki/Register-package-signing-certificates) specification.

### Benefits of repository-signed packages

Package repositories who implement repository signatures will provide a signed version of all packages, no matter if the package is author-signed or not. This capability allows to verify the complete dependency graph even for cases where a signed package has a dependency on an unsigned package. 

Packages with the repository signature offer content integrity and proof of origin, even when the package is found in a different mirror/repository.

# How to Author-Sign a package

NuGet package authors who want to sign a package must obtain a X.509 code signing certificate. It’s strongly recommended to obtain the certificate from a public certification authority. The author can also use a certificate from a private certification authority but these signatures will generate a warning if the certificate is not trusted in the machine where the validation will occur.

## Certificate Requirements

Authors require a code signing certificate, these are a special type of certificate that are valid for the `id-kp-codeSigning` purpose [[RFC 5280 section 4.2.1.12](https://tools.ietf.org/html/rfc5280#section-4.2.1.12)], and have a RSA public key length of 2048 bits or higher.

These certificates can be obtaied from a public CA like Symmantec.com, DigiCert.com, GlobalSign.com or Certum.pl 
> [!Tip]
> Certum has special offers for [open source projects](https://www.certum.eu/certum/cert,offer_en_open_source_cs.xml).

## Signing with NuGet.exe

NuGet packages can be signed with `NuGet.exe` version 4.6 or higher using the `sign` command. Typically this command will require the next input parameters:
- **Certificate** to sign with. It can be specified from a PFX file, or from a certificate already installed in the certificate store.
- **Package** to be signed. It's the nupkg file produced with the `pack` command. If it's already signed it will produce an error unless the `-overwrite` option is used.
- **Timestamp** server. Optionally you can specify the URL of a timestamp server who implements [RFC 3161](https://tools.ietf.org/html/rfc3161).

```
nuget sign MyPackage.nupkg -CertificateSubjectName <MyCertSubjectName> -Timestamper http://timestamp.digicert.com 
```

> [!Note]
> In this example we are using the digicert.com free timestamp service, but you can use any other service that satisfies the timestamp requirements defined [here](https://github.com/NuGet/Home/wiki/Package-Signatures-Technical-Details).


More details on how to use the `sign` command can be found in the [command line reference documentation](#).

> [!Note]
> Signed packages can be verified with the `verify -signatures` command.

# How to Repository-Sign a package

Repositories interested to add repository signatures to their packages should implement the requirements specified in the [Repository Signatures](https://github.com/NuGet/Home/wiki/Repository-Signatures) specification. The signing can be done using the NuGet api.

# How to consume signed packages

Visual Studio 2017 update 15.6 and beyond verifies signed packages with author or repository signatures, and will block the package installation/restore operation if the signature can not be validated. 

The complete list of errors and warnings caused by signed related issues can be found in the [NuGet errors and warning reference](https://docs.microsoft.com/en-us/nuget/reference/errors-and-warnings).


