The package's optional description appears on the **README** tab of the package's nuget.org page. The description pulls from the `<Description>` in the project file or the `$description` in the [.nuspec file](../../reference/nuspec.md).

The following example shows a `Description` in the *.csproj* file for a .NET package:

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <PackageId>Azure.Storage.Blobs</PackageId>
    <Version>12.4.0</Version>
    <PackageTags>Microsoft Azure Storage Blobs;Microsoft;Azure;Blobs;Blob;Storage;StorageScalable</PackageTags>
    <Description>
      This client library enables working with the Microsoft Azure Storage Blob service for storing binary and text data.
      For this release see notes - https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/storage/Azure.Storage.Blobs/README.md and https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/storage/Azure.Storage.Blobs/CHANGELOG.md
      in addition to the breaking changes https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/storage/Azure.Storage.Blobs/BreakingChanges.txt
      Microsoft Azure Storage quickstarts and tutorials - https://learn.microsoft.com/azure/storage/
      Microsoft Azure Storage REST API Reference - https://learn.microsoft.com/rest/api/storageservices/
      REST API Reference for Blob Service - https://learn.microsoft.com/rest/api/storageservices/blob-service-rest-api
    </Description>
  </PropertyGroup>
</Project>
```
