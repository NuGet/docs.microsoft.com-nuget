---
title: Authenticate to NuGet feeds with HTTPS client certificates
description: Configure certificates from files or certificate stores to authenticate to HTTPS package sources with NuGet and the dotnet CLI.
author: zivkan
ms.author: zivkan
ms.date: 10/23/2026
ms.topic: how-to
ai-usage: ai-generated
---

# Authenticate to a feed with an HTTPS client certificate

Some private package sources require an HTTPS client certificate for mutual TLS (mTLS) authentication.
Starting with NuGet 5.7, you can configure a client certificate for a package source in `nuget.config`.
NuGet uses the certificate when it connects to that source, including during package restore in Visual Studio, MSBuild, `nuget.exe`, and the `dotnet` CLI.

Client certificates identify you to the server during the TLS handshake.
They don't replace server certificate validation or [package signature verification](installing-signed-packages.md).
Your feed might also require [HTTP credentials](consuming-packages-authenticated-feeds.md).

## Prerequisites

- Use NuGet 5.7 or later, or a version of Visual Studio or the .NET SDK that includes it.
See [NuGet 5.7 availability](../release-notes/NuGet-5.7.md).
- Obtain a client certificate that your feed accepts, along with access to its private key.
Ask your feed administrator about certificate issuance and requirements.
- Make the certificate and private key accessible to the account that runs NuGet.
For a build agent, this account might differ from your interactive user account.

## Configure the package source

Add the HTTPS package source to `nuget.config`:

```xml
<configuration>
    <packageSources>
        <add key="Contoso" value="https://nuget.contoso.com/v3/index.json" />
    </packageSources>
</configuration>
```

Use the source name, `Contoso`, when configuring the client certificate, not the source URL.
Match the source name's capitalization exactly.
Each source can have only one client certificate configuration, either a certificate store search or a file.

NuGet applies the usual [configuration file hierarchy](configuring-nuget-behavior.md#how-settings-are-applied).
Keep certificate passwords in your user-level configuration rather than a shared repository configuration.
The following commands use an explicit `nuget.config` path so that you can choose which file to modify.

## Use a certificate from a certificate store

Install the certificate and its private key in a certificate store available to the account that runs NuGet.
For example, on Windows, you can use the current user's **Personal** certificate store, represented by `CurrentUser` and `My`.
Store availability varies by operating system.

Add the following section inside the `<configuration>` element of `nuget.config`.
Replace the example thumbprint with your certificate's thumbprint:

```xml
<clientCertificates>
    <storeCert packageSource="Contoso"
               storeLocation="CurrentUser"
               storeName="My"
               findBy="Thumbprint"
               findValue="4894671AE5AA84840CC1079E89E82D426BC24EC6" />
</clientCertificates>
```

`storeLocation`, `storeName`, and `findBy` are optional.
Their defaults are `CurrentUser`, `My`, and `Thumbprint`, respectively.

> [!IMPORTANT]
> For `findBy`, omit the `FindBy` prefix from the .NET `X509FindType` name.
> For example, use `SubjectName`, not `FindBySubjectName`, in both `nuget.config` and CLI options.
> Prefer a thumbprint to identify a specific certificate rather than a broader search that might match several certificates.

Alternatively, configure the same certificate with either CLI:

```dotnetcli
dotnet nuget add client-cert --package-source Contoso --find-by Thumbprint --find-value 4894671AE5AA84840CC1079E89E82D426BC24EC6 --configfile .\nuget.config
```

```cli
nuget client-certs add -PackageSource Contoso -FindBy Thumbprint -FindValue 4894671AE5AA84840CC1079E89E82D426BC24EC6 -ConfigFile .\nuget.config
```

## Use a certificate from a file

Use a PKCS #12 certificate file (`.pfx` or `.p12`) that includes its private key.
For example, add a password-protected file on Windows with either CLI.
Replace `<certificate-password>` with the file's password:

```dotnetcli
dotnet nuget add client-cert --package-source Contoso --path C:\certificates\client.pfx --password <certificate-password> --configfile .\nuget.config
```

```cli
nuget client-certs add -PackageSource Contoso -Path C:\certificates\client.pfx -Password <certificate-password> -ConfigFile .\nuget.config
```

Omit the password option if the file doesn't require a password.
Don't combine file options with certificate store options.
If you already configured a store certificate for `Contoso`, remove that configuration before adding a file certificate.

The commands encrypt the password by default.
Encrypted passwords work only on Windows and can be decrypted only by the user and machine that encrypted them.
If you must store a password on another operating system, `dotnet nuget add client-cert` supports `--store-password-in-clear-text`.

You can also configure a file directly:

```xml
<clientCertificates>
    <fileCert packageSource="Contoso" path="C:\certificates\client.pfx" />
</clientCertificates>
```

This example assumes the file doesn't require a password.
Use the CLI to generate an encrypted `password` attribute for a password-protected file, or use `clearTextPassword` if encryption isn't available.
Don't specify both attributes.
For path resolution rules and all attributes, see the [`clientCertificates` reference](../reference/nuget-config-file.md#clientcertificates).

> [!WARNING]
> Protect certificate files containing private keys and configuration files containing clear-text passwords with appropriate file permissions.
> Don't commit them to source control.
> Passwords passed on the command line can also appear in shell history or build logs.

## Inspect, update, or remove a configuration

List configured certificates and check whether NuGet can load them:

```dotnetcli
dotnet nuget list client-cert --configfile .\nuget.config
```

```cli
nuget client-certs list -ConfigFile .\nuget.config
```

The output includes the source name, certificate settings, and certificate thumbprints, or an error if NuGet can't load a certificate.
Listing a certificate doesn't verify that your feed accepts it.
Run restore against your feed to check the connection.

Use `dotnet nuget update client-cert` or `nuget client-certs update` to change an existing configuration.
To switch between `storeCert` and `fileCert`, remove the configuration and add a new one.
Removing a configuration doesn't delete the certificate file or uninstall a certificate from its store.

For command syntax and examples, see [`dotnet nuget add client-cert`](/dotnet/core/tools/dotnet-nuget-add-client-cert) and the [NuGet CLI reference](../reference/cli-reference/cli-ref-client-certs.md).

## Troubleshoot certificate authentication

If NuGet can't find or use your certificate, check the following:

- Match `packageSource` to the configured source name, including capitalization.
Use the source name rather than its URL.
- For store certificates, check the store location, store name, search value, and account running NuGet.
Use `Thumbprint`, not `FindByThumbprint`, for a thumbprint search.
- For file certificates, check the file path, password, and file permissions.
Ensure that the file contains the private key.
- If a password can't be decrypted, recreate the encrypted configuration with the account and machine that run NuGet.
- If the TLS handshake fails, confirm with your feed administrator that the certificate is valid and trusted by the server.
A TLS handshake failure can occur before NuGet receives an HTTP response, so a restore error might not identify a missing or rejected client certificate.

The `--force` or `-Force` option lets you save a configuration when NuGet can't find or load its certificate.
It doesn't make an unusable certificate work during restore and doesn't disable server certificate validation.
