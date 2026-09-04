---
title: Package signing certificates on NuGet.org
description: Register signing certificates on your NuGet.org account or organization to require author-signed packages.
author: pranathibora14
ms.author: prabora
ms.date: 07/23/2026
ms.topic: how-to
---

# Package signing certificates on NuGet.org

NuGet.org lets you register one or more code signing certificates on your individual account or organization.
You manage registered certificates in the **Certificates** section of the account or organization settings page.

Registering a certificate can change the author-signing requirements for the package IDs associated with that account.
NuGet.org determines these requirements separately for each package ID, based on its owners and any configured required signer.
When future versions of a package must be author-signed, this is referred to as **required author signing**.

This article describes the effect of registering a certificate and how to manage certificates on NuGet.org.
To learn how package signatures work and the requirements NuGet.org enforces on each signature, see [Signed package reference](../reference/Signed-Packages-Reference.md).

## What happens when you register a certificate

For a package ID that you solely own and that has no other required signer:

- Before you register any certificate, you can push both unsigned packages and packages signed with any (otherwise valid) certificate.
- After you register **one or more** certificates, new versions must be author-signed with one of your registered certificates. Unsigned packages, and packages signed with a certificate that isn't registered, aren't accepted.

For packages with more than one owner, see [Packages with multiple owners](#packages-with-multiple-owners).

These requirements apply to **new versions** you push after registering a certificate.
Versions that were already published remain available and are not affected.

NuGet.org enforces the signing requirement during asynchronous validation after a package is uploaded.
A submission that doesn't meet the requirement fails validation and isn't published, rather than being rejected when the push is first received.

> [!IMPORTANT]
> After you register a certificate, you must author-sign every subsequent version you publish for the affected package IDs with a registered certificate.
> If you submit an unsigned package, or a package signed with an unregistered certificate, the version fails validation and isn't published.
> For a walkthrough, see [Sign a package](../create-packages/Sign-a-Package.md).

## Signature requirements on NuGet.org

Registering a certificate doesn't change the requirements NuGet.org enforces on each signature.
For the full list of signature and certificate requirements, see [Signature requirements on NuGet.org](../reference/Signed-Packages-Reference.md#signature-requirements-on-nugetorg).

## Register a certificate

You register a certificate from the **Certificates** section of your account or organization settings page, uploading your public code signing certificate as a DER-encoded `.cer` file.
For the step-by-step walkthrough, see [Register the certificate on NuGet.org](../create-packages/Sign-a-Package.md#register-the-certificate-on-nugetorg).

Only the public certificate is uploaded.
Never upload or share the private key associated with your signing certificate.

You can register more than one certificate on an account.
This is useful when rotating to a new certificate or when different build environments use different certificates: a package author-signed with any one of the registered certificates is accepted.

## Remove a certificate

You can remove a registered certificate from the **Certificates** section of the account or organization settings page.

- If the account still has **at least one** registered certificate after removal, required author signing remains in effect and packages must be signed with one of the remaining certificates.
- If you remove the **last** registered certificate, the account is no longer enrolled in required author signing. The account can once again push unsigned packages or packages signed with any valid certificate.

## Packages with multiple owners

A package can have multiple owners, and each owner can independently register certificates.
NuGet.org computes the author-signing policy per package ID from that package's owners and any configured required signer:

- **No required signer:** signing is allowed when any owner has a registered certificate and required only when every owner has one. A signed package can use a certificate registered to any owner.
- **A required signer is set:** only that account's certificate state matters for these signing calculations. If it has certificates, signing is required and only its certificates are accepted. If it has none, asynchronous signature validation rejects every author-signed submission, even when another owner has certificates.

You can set the required signer from the **Manage Packages** page.

> [!NOTE]
> A publisher can also push on behalf of multiple organizations, each with independent certificate registrations, while each package ID retains its own policy.

## Related articles

- [Signed package reference](../reference/Signed-Packages-Reference.md)
- [Sign a package](../create-packages/Sign-a-Package.md)
- [Individual accounts on NuGet.org](individual-accounts.md)
- [Organizations on NuGet.org](organizations-on-nuget-org.md)
