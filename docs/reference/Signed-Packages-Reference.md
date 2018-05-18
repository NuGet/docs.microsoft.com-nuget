---
title: Signed Packages
description: Requirements for NuGet package signing.
author: rido-min
ms.author: rmpablos
manager: unnir
ms.date: 05/18/2018
ms.topic: reference
ms.reviewer: ananguar
---

# Signed packages

*NuGet 4.6.0+ and Visual Studio 2017 version 15.6 and later*

NuGet packages can include a digital signature that provides protection against tampered content. This signature is produced from an X.509 certificate that also adds authenticity proofs to the actual origin of the package.

Signed packages provide the strongest end-to-end validation. An author signature guarantees that the package has not been modified since the author signed the package, no matter from which repository or what transport method the package is delivered.

Additionally, author-signed packages provide an extra authentication mechanism to the nuget.org publishing pipeline because the signing certificate must be registered ahead of time. For more information, see [Register certificates](#register-certificate-on-nugetorg).

For details on creating a signed package, see [Signing Packages](../create-packages/Sign-a-package.md) and the [nuget sign command](../tools/cli-ref-sign.md).

> [!Important]
> Package signing is currently supported only when using nuget.exe on Windows. Verification of signed packages is currently supported only when using nuget.exe or Visual Studio on Windows.

## Certificate requirements

Package signing requires a code signing certificate, which is a special type of certificate that is valid for the `id-kp-codeSigning` purpose [[RFC 5280 section 4.2.1.12](https://tools.ietf.org/html/rfc5280#section-4.2.1.12)]. Additionally, the certificate must have an RSA public key length of 2048 bits or higher.

## Get a code signing certificate

Valid certificates may be obtained from a public certificate authority like:

- [Symantec](https://trustcenter.websecurity.symantec.com/process/trust/productOptions?productType=SoftwareValidationClass3)
- [DigiCert](https://www.digicert.com/code-signing/)
- [Go Daddy](https://www.godaddy.com/web-security/code-signing-certificate)
- [Global Sign](https://www.globalsign.com/en/code-signing-certificate/)
- [Comodo](https://www.comodo.com/e-commerce/code-signing/code-signing-certificate.php)
- [Certum](https://www.certum.eu/certum/cert,offer_en_open_source_cs.xml) 

The complete list of certification authorities trusted by Windows can be obtained from [http://aka.ms/trustcertpartners](http://aka.ms/trustcertpartners).

## Create a test certificate

You can use self-issued certificates for testing purposes. To create a self-issued certificate, use the [New-SelfSignedCertificate PowerShell command](/powershell/module/pkiclient/new-selfsignedcertificate.md).

```ps
New-SelfSignedCertificate -Subject "CN=NuGet Test Developer, OU=Use for testing purposes ONLY" `
                          -FriendlyName "NuGetTestDeveloper" `
                          -Type CodeSigning `
                          -KeyUsage DigitalSignature `
                          -KeyLength 2048 `
                          -KeyAlgorithm RSA `
                          -HashAlgorithm SHA256 `
                          -Provider "Microsoft Enhanced RSA and AES Cryptographic Provider" `
                          -CertStoreLocation "Cert:\CurrentUser\My" 
```

This command creates a testing certificate available in the current user's personal certificate store. You can open the certificate store by running `certmgr.msc` to see the newly created certificate.

> [!Warning]
> nuget.org does not accept packages signed with self-issued certificates.

## Timestamp requirements

Signed packages should include an RFC 3161 timestamp to ensure signature validity beyond the package signing certificate's validity period. The certificate used to sign the timestamp must be valid for the `id-kp-timeStamping` purpose [[RFC 5280 section 4.2.1.12](https://tools.ietf.org/html/rfc5280#section-4.2.1.12)]. Additionally, the certificate must have an RSA public key length of 2048 bits or higher.

Additional technical details can be found in the [package signature technical specs](https://github.com/NuGet/Home/wiki/Package-Signatures-Technical-Details) (GitHub).

## Signature requirements on nuget.org

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

## Register certificate on nuget.org

To submit a signed package, you must first register the certificate with nuget.org. You need the certificate as a `.cer` file in a binary DER format. You can export an existing certificate to a binary DER format by using the Certificate Export Wizard.

![Certificate Export Wizard](media/CertificateExportWizard.png)

Advanced users can export the certificate using the [Export-Certificate PowerShell command](/powershell/module/pkiclient/export-certificate).

To register the certificate with nuget.org, go to `Certificates` section on `Account settings` page (or the Organization's settings page) and select `Register new certificate`.

![Registered Certificates](media/registered-certs.png)

> [!Tip]
> One user can submit multiple certificates and the same certificate can be registered by multiple users.

Once a user has a certificate registered, all future package submissions **must** be signed with one of the certificates.

Users can also remove a registered certificate from the account. Once a certificate is removed, packages signed with that certificate fail at submission. Existing packages aren't affected.

## Configure package signing requirements

If you are the sole owner of a package, you are the required signer. That is, you can use any of the registered certificates to sign your packages and submit to nuget.org.

If a package has multiple owners, by default, "Any" owner's certificates can be used to sign the package. As a co-owner of the package, you can override "Any" with yourself or any other co-owner to be the required signer. If you make an owner  who does not have any certificate registered, then unsigned packages will be allowed. 

Similarly, if the default "Any" option is selected for a package where one owner has a certificate registered and another owner does not have any certificate registered, then nuget.org accepts either a signed package with a signature registered by one of its owners or an unsigned package (because one of the owners does not have any certificate registered).

![Configure package signers](media/configure-package-signers.png)
