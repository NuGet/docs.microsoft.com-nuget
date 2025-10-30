---
title: Manage package trust boundaries
description: Describes the process of installing signed NuGet packages and configuring package signature trust settings.
author: JonDouglas
ms.author: jodou
ms.date: 11/29/2018
ms.topic: install-set-up-deploy
---

# Manage package trust boundaries

Signed packages don't require any specific action to be installed; however, if the content has been modified since it was signed, the installation is blocked with error [NU3008](../reference/errors-and-warnings/NU3008.md).

> [!Warning]
> Packages signed with untrusted certificates are considered as unsigned and are installed without any warnings or errors like any other unsigned package.

## Configure package signature requirements

> [!Note]
> Requires NuGet 4.9.0+ and Visual Studio version 15.9 and later on Windows

You can configure how NuGet clients validate package signatures by setting the `signatureValidationMode` to `require` in the [nuget.config](../reference/nuget-config-file.md) file using the [`nuget config`](../reference/cli-reference/cli-ref-config.md) command.

```cmd
nuget.exe config -set signatureValidationMode=require
```

```xml
  <config>
    <add key="signatureValidationMode" value="require" />
  </config>
```

This mode will verify that all packages are signed by any of the certificates trusted in the `nuget.config` file. This file allows you to specify which authors and/or repositories are trusted based on the certificate's fingerprint.

### Trust package author

To trust packages based on the author signature use the [`trusted-signers`](../reference/cli-reference/cli-ref-trusted-signers.md) command to set the `author` property in the nuget.config.

```cmd
nuget.exe  trusted-signers Add -Name MyCompanyCert -CertificateFingerprint CE40881FF5F0AD3E58965DA20A9F571EF1651A56933748E1BF1C99E537C4E039 -FingerprintAlgorithm SHA256
```

```xml
<trustedSigners>
  <author name="MyCompanyCert">
    <certificate fingerprint="CE40881FF5F0AD3E58965DA20A9F571EF1651A56933748E1BF1C99E537C4E039" hashAlgorithm="SHA256" allowUntrustedRoot="false" />
  </author>
</trustedSigners>
```

>[!TIP]
>Use the `nuget.exe` [verify command](../reference/cli-reference/cli-ref-verify.md) to get the `SHA256` value of the certificate's fingerprint.


### Trust all packages from a repository

To trust packages based on the repository signature use the `repository` element:

```xml
<trustedSigners>  
  <repository name="nuget.org" serviceIndex="https://api.nuget.org/v3/index.json">
    <certificate fingerprint="0E5F38F57DC1BCC806D8494F4F90FBCEDD988B4676070...." 
                  hashAlgorithm="SHA256" 
                allowUntrustedRoot="false" />
  </repository>
</trustedSigners>
```

### Trust Package Owners

Repository signatures include additional metadata to determine the owners of the package at the time of submission. You can restrict packages from a repository based on a list of owners:

```xml
<trustedSigners>  
  <repository name="nuget.org" serviceIndex="https://api.nuget.org/v3/index.json">
    <certificate fingerprint="0E5F38F57DC1BCC806D8494F4F90FBCEDD988B4676070...." 
                  hashAlgorithm="SHA256" 
                allowUntrustedRoot="false" />
      <owners>microsoft;nuget</owners>
  </repository>
</trustedSigners>
```

If a package has multiple owners, and any one of those owners is in the trusted list, the package installation will succeed.

### Untrusted Root certificates

In some situations you may want to enable verification using certificates that do not chain to a trusted root in the local machine. You can use the `allowUntrustedRoot` attribute to customize this behavior.

### Sync repository certificates

Package repositories should announce the certificates they use in their [service index](../api/service-index.md). Eventually the repository will update these certificates, e.g. when the certificate expires. When that happens, clients with specific policies will require an update to the configuration to include the newly added certificate. You can easily upgrade the trusted signers associated to a repository by using the `nuget.exe` [trusted-signers sync command](../reference/cli-reference/cli-ref-trusted-signers.md#nuget-trusted-signers-sync--name-name).

### Schema reference

The complete schema reference for the client policies can be found in the [nuget.config reference](../reference/nuget-config-file.md#trustedsigners-section)

## Related articles

- [Signing NuGet Packages](../create-packages/Sign-a-Package.md)
- [Signed Packages Reference](../reference/Signed-Packages-Reference.md)
