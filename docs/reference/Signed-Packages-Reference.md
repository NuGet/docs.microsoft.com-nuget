---
title: Signed Packages
description: Requirements for NuGet package signing.
author: rido-min
ms.author: rmpablos
ms.date: 05/18/2018
ms.topic: reference
ms.reviewer: ananguar
---

# Signed packages

*NuGet 4.6.0+ and Visual Studio 2017 version 15.6 and later*

NuGet packages can include a digital signature that provides protection against tampered content. This signature is produced from an X.509 certificate that also adds authenticity proofs to the actual origin of the package.

Signed packages provide the strongest end-to-end validation. There are two different types of NuGet signatures:
- **Author Signature**. An author signature guarantees that the package has not been modified since the author signed the package, no matter from which repository or what transport method the package is delivered. Additionally, author-signed packages provide an extra authentication mechanism to the nuget.org publishing pipeline because the signing certificate must be registered ahead of time. For more information, see [Register certificates](#signature-requirements-on-nugetorg).
- **Repository Signature**. Repository signatures provide an integrity guarantee for **all** packages in a repository whether they are author signed or not, even if those packages are obtained from a different location than the original repository where they were signed.   

For details on creating an author signed package, see [Signing Packages](../create-packages/Sign-a-package.md) and the [nuget sign command](../reference/cli-reference/cli-ref-sign.md).

> [!Important]
> Package signing is currently supported only when using nuget.exe on Windows. Verification of signed packages is currently supported only when using nuget.exe or Visual Studio on Windows.

## Certificate requirements

Package signing requires a code signing certificate, which is a special type of certificate that is valid for the `id-kp-codeSigning` purpose [[RFC 5280 section 4.2.1.12](https://tools.ietf.org/html/rfc5280#section-4.2.1.12)]. Additionally, the certificate must have an RSA public key length of 2048 bits or higher.

## Timestamp requirements

Signed packages should include an RFC 3161 timestamp to ensure signature validity beyond the package signing certificate's validity period. The certificate used to sign the timestamp must be valid for the `id-kp-timeStamping` purpose [[RFC 5280 section 4.2.1.12](https://tools.ietf.org/html/rfc5280#section-4.2.1.12)]. Additionally, the certificate must have an RSA public key length of 2048 bits or higher.

Additional technical details can be found in the [package signature technical specs](https://github.com/NuGet/Home/wiki/Package-Signatures-Technical-Details) (GitHub).

## Signature requirements on NuGet.org

nuget.org has additional requirements for accepting a signed package:

- The primary signature must be an author signature.
- The primary signature must have a single valid timestamp.
- The X.509 certificates for both the author signature and its timestamp signature:
  - Must have an RSA public key 2048 bits or greater.
  - Must be within its validity period per current UTC time at time of package validation on nuget.org.
  - Must chain to a trusted root authority that is trusted by default on Windows. Packages signed with self-issued certificates are rejected.
  - Must be valid for its purpose: 
    - The author signing certificate must be valid for code signing.
    - The timestamp certificate must be valid for timestamping.
  - Must not be revoked at signing time. (This may not be knowable at submission time, so nuget.org periodically rechecks revocation status).
  
  
## Related articles

- [Signing NuGet Packages](../create-packages/Sign-a-Package.md)
- [Manage package trust boundaries](../consume-packages/installing-signed-packages.md)
