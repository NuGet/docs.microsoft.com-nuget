---
title: NuGet CLI client-certs command
description: Reference for the nuget.exe client-certs command to list, add, update, and remove HTTPS client certificate configurations.
author: zivkan
ms.author: zivkan
ms.date: 10/23/2026
ms.topic: reference
ai-usage: ai-assisted
---

# client-certs command (NuGet CLI)

**Applies to:** package consumption and publishing.
**Supported versions:** 5.7 and later.

Manages HTTPS client certificate configurations in `nuget.config`.
For setup instructions, see [Authenticate to a feed with an HTTPS client certificate](../../consume-packages/https-client-certificates.md).
For configuration attributes, see [`clientCertificates`](../nuget-config-file.md#clientcertificates).

## Usage

```cli
nuget client-certs <list|add|update|remove> [options]
```

If you omit the operation, the command defaults to `list`.

## Operations

- `list`: Lists all client certificate configurations, including certificate thumbprints or an error if a certificate can't be loaded.
- `add`: Adds a configuration for the specified package source.
Fails if a client certificate configuration already exists for that source.
- `update`: Updates an existing configuration for the specified package source.
Fails if no configuration exists or if you try to switch between a file and a certificate store.
Unspecified settings retain their existing values.
- `remove`: Removes the configuration for the specified package source.
Doesn't delete the certificate file or remove a certificate from its store.

For `add`, choose either a file with `-Path` or a store search with `-FindValue`.
For `update`, provide at least one file setting (`-Path` or `-Password`) or store setting (`-StoreLocation`, `-StoreName`, `-FindBy`, or `-FindValue`).
Don't combine file and store settings.
By default, `add` and `update` fail if NuGet can't find or load the certificate.

## Options

| Option | Description |
| --- | --- |
| `-PackageSource <name>` | Required for `add`, `update`, and `remove`. The configured package source name, not its URL. Match capitalization exactly. |
| `-Path <path>` | For file certificates with `add` or `update`. The path to a PKCS #12 file (`.pfx` or `.p12`) containing the certificate and private key. See [path resolution](../nuget-config-file.md#filecert). |
| `-Password <password>` | For file certificates with `add` or `update`. The file's password, if needed. Encrypted before storage unless you specify `-StorePasswordInClearText`. |
| `-StorePasswordInClearText` | Stores a file certificate's password without encryption. Applies to `add` and `update`. |
| `-StoreLocation <location>` | For store certificates with `add` or `update`. `CurrentUser` or `LocalMachine`. Defaults to `CurrentUser` for `add`. |
| `-StoreName <name>` | For store certificates with `add` or `update`. A [StoreName](/dotnet/api/system.security.cryptography.x509certificates.storename) value. Defaults to `My` for `add`. Store availability depends on your operating system. |
| `-FindBy <type>` | For store certificates with `add` or `update`. An [X509FindType](/dotnet/api/system.security.cryptography.x509certificates.x509findtype) name **without the `FindBy` prefix**, such as `Thumbprint` or `SubjectName`. Defaults to `Thumbprint` for `add`. |
| `-FindValue <value>` | For store certificates with `add` or `update`. The search value for `-FindBy`. Required when adding a store certificate. |
| `-Force` | For `add` or `update`. Saves the configuration even if NuGet can't find or load the certificate. Doesn't bypass TLS validation during package operations. |
| `-ConfigFile <file>` | The NuGet configuration file to read or modify. Required when adding a file certificate with `add -Path`, even with `-Force`. If specified, only this file is used. For other operations, omitting this option uses the configuration hierarchy from the current directory. |
| `-ForceEnglishOutput` | Uses an invariant, English-based culture for command output. |
| `-NonInteractive` | Suppresses prompts for user input or confirmations. |
| `-Verbosity <level>` | Sets output detail to `normal` (the default), `quiet`, or `detailed`. |
| `-?` or `-Help` | Displays help for the command. |

> [!WARNING]
> Password encryption is supported only on Windows.
> An encrypted password can be decrypted only by the same user on the same machine.
> Don't commit private keys or passwords to source control, and avoid exposing passwords in shell history or build logs.

## Examples

Add a certificate from the current user's personal certificate store:

```cli
nuget client-certs add -PackageSource Contoso -FindBy Thumbprint -FindValue 4894671AE5AA84840CC1079E89E82D426BC24EC6 -ConfigFile .\nuget.config
```

Add a password-protected certificate file on Windows.
Replace `<certificate-password>` with the file's password:

```cli
nuget client-certs add -PackageSource Fabrikam -Path C:\certificates\client.pfx -Password <certificate-password> -ConfigFile .\nuget.config
```

Update a store search to use a renewed certificate:

```cli
nuget client-certs update -PackageSource Contoso -FindValue BA4D3CC1F011388626A23AF5627BB4721D44DB6E -ConfigFile .\nuget.config
```

List configurations, then remove one:

```cli
nuget client-certs list -ConfigFile .\nuget.config
nuget client-certs remove -PackageSource Contoso -ConfigFile .\nuget.config
```
