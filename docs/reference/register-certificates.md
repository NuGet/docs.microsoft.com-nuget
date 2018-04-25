---
title: Register Certificates Reference
description: Register certificates allows signed packages submissions.
author: rido-min
ms.author: rido-min
manager: unnir
ms.date: 04/25/2018
ms.topic: reference
ms.reviewer: ananguar
---

# Register certificates

To submit signed packages to NuGet.org the certificate used to sign must be registered in NuGet.org before. This topic describes the options to register certificates and how to configure packages to require one.

## Export  Certificate  

Prior to register a certificate, it must  be exported as `.cer` file using a binary DER format. You can export the certificate using the Certificate Export Wizard.

![Certificate Export Wizard](media/CertificateExportWizard.png) 

Advanced users can export the certificate using the `Export-Certificate` powershell [command](https://docs.microsoft.com/en-us/powershell/module/pkiclient/export-certificate?view=win10-ps).

## Register a new certificate

The NuGet.org account page includes a section to manage the certificates associated to that account.

![Registered Certificates](media/registered-certs.png)

>[!Tip]
>One user can submit multiple certificates and the same certificate can be registered by multiple users.

Once a user has a certificate registered, all future submissions must be signed with one of the certificates.

### Unregister a certificate

Users can remove a registered certificate from his account, from that point packages signed with that certificate will fail at submission. Existing packages won't be affected.


## Configure package signing requirements

If the package only has one owner any of the registered certificates must be used, in the case of multiple owners the package can be configured to use a certificate from `Any` of the certificates associated with any account, or the certificates of one specific account.

![Configure package signers](media/configure-package-signers.png)

## How to exclude a package from requiring signed versions

To exclude a package from the signing requirements you can add a Co-Owner with no certificates selected and make sure the package is configured to allow `Any` or the account without certificates.





