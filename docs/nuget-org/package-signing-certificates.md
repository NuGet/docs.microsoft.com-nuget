---
title: Package signing certificates on NuGet.org
description: Register signing certificates on your NuGet.org account or organization to require author-signed packages.
author: pranathibora14
ms.author: prabora
ms.date: 07/23/2026
ms.topic: how-to
---

# Package signing certificates on NuGet.org

NuGet.org lets you register one or more code signing certificates on your individual account or organization. You manage registered certificates in the **Certificates** section of the account or organization settings page.

Once you register at least one certificate, NuGet.org requires every package that account pushes to be author-signed with one of the registered certificates. This is referred to as **required author signing**.

This article describes the effect of registering a certificate and how to manage certificates on NuGet.org. For how package signatures work and the requirements NuGet.org enforces on each signature, see [Signed package reference](../reference/Signed-Packages-Reference.md).

## What happens when you register a certificate

Before you register any certificate, the account can push both unsigned packages and packages signed with any (otherwise valid) certificate.

Once you register **one or more** certificates on an account:

- NuGet.org **rejects unsigned packages** pushed by that account.
- NuGet.org **rejects packages author-signed with a certificate that isn't registered** to the account.
- Packages author-signed with **any one** of the registered certificates are accepted, provided the signature also meets the [signature requirements on NuGet.org](#signature-requirements-on-nugetorg).

This applies to **new versions** you push after enrollment. Package versions that were already published remain available and are not affected.

> [!IMPORTANT]
> After you register a certificate, you must author-sign every subsequent push from that account with a registered certificate. If you push an unsigned package, or a package signed with an unregistered certificate, the push is rejected. Author signing is produced with `nuget.exe sign`. For a walkthrough, see [Sign a package](../create-packages/Sign-a-Package.md).

## Signature requirements on NuGet.org

NuGet.org has additional requirements for accepting a signed package:

- The primary signature must be an author signature.
- The primary signature must have a single valid timestamp.
- The X.509 certificates for both the author signature and its timestamp signature:
  - Must have an RSA public key 2048 bits or greater.
  - Must be within its validity period per current UTC time at time of package validation on NuGet.org.
  - Must chain to a trusted root authority that is trusted by default on Windows. Packages signed with self-issued certificates are rejected.
  - Must be valid for its purpose:
    - The author signing certificate must be valid for code signing.
    - The timestamp certificate must be valid for timestamping.
  - Must not be revoked at signing time. (This may not be knowable at submission time, so NuGet.org periodically rechecks revocation status.)

## Register a certificate

To register a certificate, you must have a confirmed email address and be signed in with two-factor authentication (2FA).

1. Sign in to [NuGet.org](https://www.nuget.org).
1. Go to your [account settings](https://www.nuget.org/account) (for an individual account) or your [organization's settings](https://www.nuget.org/account/organizations) (for an organization).
1. Expand the **Certificates** section.
1. Select **Register new** and upload your public code signing certificate as a DER-encoded `.cer` file.

Only the public certificate is uploaded. Never upload or share the private key associated with your signing certificate.

You can register more than one certificate on an account. This is useful when rotating to a new certificate or when different build environments use different certificates: a package author-signed with any one of the registered certificates is accepted.

## Remove a certificate

You can remove a registered certificate from the **Certificates** section of the account or organization settings page.

- If the account still has **at least one** registered certificate after removal, required author signing remains in effect and packages must be signed with one of the remaining certificates.
- If you remove the **last** registered certificate, the account is no longer enrolled in required author signing. It can once again push unsigned packages or packages signed with any valid certificate.

## Packages with multiple owners

A package can have multiple owners, and each owner can independently register certificates. The signing requirement for a co-owned package depends on the owners' certificate state:

- If **all** owners have at least one registered certificate, future versions **must** be signed.
- If **no** owner has a registered certificate, future versions **must** be unsigned.
- If **some** owners have certificates and others don't, future versions **may** be unsigned or signed with a certificate registered to any owner.

To make the requirement unambiguous for a co-owned package, you can designate a **required signer**. The required signer's certificate state alone determines whether that package's future versions must be signed, and only the required signer's registered certificates are accepted. You can set the required signer from the **Manage Packages** page.

## Related articles

- [Signed package reference](../reference/Signed-Packages-Reference.md)
- [Sign a package](../create-packages/Sign-a-Package.md)
- [Individual accounts on NuGet.org](individual-accounts.md)
- [Organizations on NuGet.org](organizations-on-nuget-org.md)
