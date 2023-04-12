---
title: Signing NuGet Packages
description: Explains how signed packages can be used to enable content integrity verification.
author: rido-min
ms.author: rmpablos
ms.date: 03/06/2018
ms.topic: conceptual
ms.reviewer: anangaur
---

# Sign a NuGet package

A signed package allows for content integrity verification checks, which provides protection against content tampering. The package signature also serves as the single source of truth about the actual origin of the package and bolsters package authenticity for the consumer. This guide assumes you have already [created a package](creating-a-package.md).

## Get a code signing certificate

Valid certificates may be obtained from any of the following Microsoft trusted certificate authorities:

- [Certum](https://www.certum.eu/certum/cert,offer_en_open_source_cs.xml)
- [Comodo](https://www.comodo.com/e-commerce/code-signing/code-signing-certificate.php)
- [DigiCert](https://www.digicert.com/code-signing/)
- [GlobalSign](https://www.globalsign.com/en/code-signing-certificate/)
- [SSL.com](https://www.ssl.com/certificates/code-signing/)

The complete list of certification authorities trusted by Windows can also be obtained from [http://aka.ms/trustcertpartners](/security/trusted-root/participants-list).

You can use self-issued certificates for testing purposes. However, packages signed using self-issued certificates are not accepted by NuGet.org. Learn more about [creating a test certificate](#create-a-test-certificate)

## Export the certificate file

* You can export an existing certificate to a binary DER format by using the Certificate Export Wizard.

  ![Certificate Export Wizard](../reference/media/CertificateExportWizard.png)

* You can also export the certificate using the [Export-Certificate PowerShell command](/powershell/module/pki/export-certificate).

## Sign the package

Sign the package using [dotnet nuget sign](/dotnet/core/tools/dotnet-nuget-sign) (requires .NET 6.0.100 SDK or later).

```cli
dotnet nuget sign MyPackage.nupkg --certificate-path <PathToTheCertificate> --timestamper <TimestampServiceURL>
```

or

Sign the package using [nuget sign](../reference/cli-reference/cli-ref-sign.md) (requires nuget.exe 4.6.0 or later):

```cli
nuget sign MyPackage.nupkg -CertificatePath <PathToTheCertificate> -Timestamper <TimestampServiceURL>
```

> [!Tip]
> The certificate provider often also provides a timestamping server URL which you can use for the `Timestamper` optional argument show above. Consult with your provider's documentation and/or support for that service URL.

* You can use a certificate available in the certificate store or use a certificate from a file. See CLI reference for [nuget sign](../reference/cli-reference/cli-ref-sign.md).
* Signed packages should include a timestamp to make sure the signature remains valid when the signing certificate has expired. Else the sign operation will produce a [warning](../reference/errors-and-warnings/NU3002.md).
* You can see the signature details of a given package using [nuget verify](../reference/cli-reference/cli-ref-verify.md).

## Register the certificate on NuGet.org

To publish a signed package, you must first register the certificate with NuGet.org. You need the certificate as a `.cer` file in a binary DER format.

1. [Sign in](https://www.nuget.org/users/account/LogOn?returnUrl=%2F) to NuGet.org.
1. Go to `Account settings` (or `Manage Organization` **>** `Edit Organization` if you would like to register the certificate with an Organization account).
1. Expand the `Certificates` section and select `Register new`.
1. Browse and select the certficate file that was exported earlier.
  ![Registered Certificates](../reference/media/registered-certs.png)

> [!NOTE]
>
> * One user can submit multiple certificates and the same certificate can be registered by multiple users.
> * Once a user has a certificate registered, all future package submissions **must** be signed with one of the certificates. See [Manage signing requirements for your package on NuGet.org](#manage-signing-requirements-for-your-package-on-nugetorg)
> * Users can also remove a registered certificate from the account. Once a certificate is removed, new packages signed with that certificate will fail at submission. Existing packages aren't affected.

## Publish the package

You're now ready to publish the package to NuGet.org. See [Publishing packages](../nuget-org/Publish-a-package.md).

## Create a test certificate

You can use self-issued certificates for testing purposes. To create a self-issued certificate, use the [New-SelfSignedCertificate PowerShell command](/powershell/module/pki/new-selfsignedcertificate).

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
> NuGet.org does not accept packages signed with self-issued certificates.

## Manage signing requirements for your package on NuGet.org

1. [Sign in](https://www.nuget.org/users/account/LogOn?returnUrl=%2F) to NuGet.org.

1. Go to `Manage Packages` 
   ![Configure package signers](../reference/media/configure-package-signers.png)

* If you are the sole owner of a package, you are the required signer, that is, you can use any of the registered certificates to sign and publish your packages to NuGet.org.

* If a package has multiple owners, by default, "Any" owner's certificates can be used to sign the package. As a co-owner of the package, you can override "Any" with yourself or any other co-owner to be the required signer. If you make an owner who does not have any certificate registered, then unsigned packages will be allowed. 

* Similarly, if the default "Any" option is selected for a package where one owner has a certificate registered and another owner does not have any certificate registered, then NuGet.org accepts either a signed package with a signature registered by one of its owners or an unsigned package (because one of the owners does not have any certificate registered).

## Related articles

- [Manage package trust boundaries](../consume-packages/installing-signed-packages.md)
- [Signed Packages Reference](../reference/Signed-Packages-Reference.md)
- [.NET signed package verification](/dotnet/core/tools/nuget-signed-package-verification)
