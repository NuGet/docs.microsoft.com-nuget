The package's optional description, displayed on the package's NuGet page, is either pulled in from the `<description></description` used in the `.csproj` file or pulled in via the `$description` in the [.nuspec file](../../reference/nuspec.md).

The description should include links to relevant sites including product and documentation sites.

An example of a _description_ field with links is shown shown in the following XML text of the `.csproj` file for a .NET package:

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <AssemblyTitle>Microsoft Azure.Storage.Blobs client library</AssemblyTitle>
    <Version>12.4.0-preview.1</Version>
    <PackageTags>Microsoft Azure Storage Blobs;Microsoft;Azure;Blobs;Blob;Storage;StorageScalable;$(PackageCommonTags)</PackageTags>
    <Description>
      This client library enables working with the Microsoft Azure Storage Blob service for storing binary and text data.
      For this release see notes - https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/storage/Azure.Storage.Blobs/README.md and https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/storage/Azure.Storage.Blobs/CHANGELOG.md
      in addition to the breaking changes https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/storage/Azure.Storage.Blobs/BreakingChanges.txt
      Microsoft Azure Storage quickstarts and tutorials - https://docs.microsoft.com/en-us/azure/storage/
      Microsoft Azure Storage REST API Reference - https://docs.microsoft.com/en-us/rest/api/storageservices/
      REST API Reference for Blob Service - https://docs.microsoft.com/en-us/rest/api/storageservices/blob-service-rest-api
    </Description>
    <PackageReleaseNotes>
        <![CDATA[
        This is a public release of the Azure Storage Blobs SDK.
        Changes in this release:
          - Include ...
        ]]></PackageReleaseNotes>
  </PropertyGroup>
</PropertyGroup>
```
