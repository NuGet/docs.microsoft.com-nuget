---
title: Package Authoring Best Practices Guide
description: A general guide of best practices for creating high quality NuGet packages.
author: chgill-MSFT
ms.author: chgill
ms.date: 03/26/20
ms.topic: conceptual
---

# Package authoring best practices

This guidance is intended to give NuGet.org package authors a light-weight end to end reference for creating and publishing high quality packages. 

This guide will present four types of recommendations: DO, CONSIDER, AVOID, and DO NOT.

You should almost always follow a DO or an AVOID recommendation. For example:
✔️ DO include a short description for your package that describes what it is.
❌ AVOID including a netstandard1.x target.

You should generally follow a CONSIDER or AVOID recommendation, but there may be legitimate exceptions. For example:
✔️ CONSIDER including a link to an associated project website or repository.

## Framework targeting

Authors should consider making their packages as inclusive as possible if they would like to maximize the number of potential consumers and overall impact in the .NET ecosystem.

### Cross-platform and framework compatibility

Highly inclusive packages are cross-platform and compatible with as many frameworks as possible. Today, that means using .NET Standard. .NET Standard is a specification of .NET APIs that are available on all .NET implementations.

✔️ CONSIDER targeting `netstandard2.0` to maximize your potential impact.

## Package Metadata

Metadata is a foundational component of any NuGet package. The quality of your metadata can vastly influence the discoverability and usability of your package.

The recommended way to include/modify package metadata for Visual Studio users is to go to the "Package" section of your Project Properties. Otherwise, you can add metadata properties directly to the project file (csproj).

Below is a table mapping and describing all the packages metadata elements:

| Visual Studio property name | csproj/MSBuild property name | Nuspec property name        | Description                                                                                                       |
|-----------------------------|------------------------------|-----------------------------|-------------------------------------------------------------------------------------------------------------------|
| `Package id`                | `PackageId`                  | `id`                        | The package identifier. A prefix from the identifier can be reserved if it meets the  criteria.                   |
| `Package Version`           | `Version`                    | `version`                   | NuGet package version. For more information, see NuGet package version.                                           |
| `Authors`                   | `Authors`                    | `authors`                   | A comma-separated list of package authors, matching the profile names on nuget.org.                               |
| `Description`               | `Description`                | `description`               | A long description of the package displayed in UI.                                                                |
| `Copyright`                 | `Copyright`                  | `copyright`                 | Copyright details for the package. language.                                                                      |
| `Licensing - Expression`    | `PackageLicenseExpression`   | `license type="expression"` | An SPDX license expression or path to a license file within the package, often shown in UIs like nuget.org.       |
| `Licensing - File`          | `PackageLicenseFile`         | `license type="file"`       | File for custom license that isn't supported by license expressions                                               |
| `Project URL`               | `PackageProjectUrl`          | `projectUrl`                | A URL for the project homepage.                                                                                   |
| `Icon File`                 | `PackageIcon`                | `icon`                      | An image to use as the icon for the package.                                                                      |
| `Repository URL`            | `RepositoryUrl`              | `repository url=""`         | URL to the repository from which the package was built.                                                           |
| `Repository type`           | `RespositoryType`            | `repository type=""`        | Type of repository the repository URL is pointing to (i.e. git)                                                   |
| `Tags`                      | `PackageTags`                | `tags`                      | A space-delimited list of tags and keywords that describe the package. Tags are used when searching for packages. |
| `Release notes`             | `PackageReleaseNotes`        | `releaseNotes`              | A description of the changes made in this release of the package.                                                 |

#### Package ID 

✔️ CONSIDER choosing a NuGet package name with a prefix that meets NuGet's prefix reservation [criteria](https://docs.microsoft.com/en-us/nuget/reference/id-prefix-reservation).

Example: 

#### Package Version

✔️ CONSIDER using [SemVer 2.0.0](https://semver.org/) to version your NuGet package.
> Essentially, this means using the Major.Minor.Patch[-prerelease] format. Which number get incremented depends on the types of changes made since the last release.

✔️ DO include a pre-release suffix when releasing a non-stable package.
> For more detailed guidance about when to publish a pre-release package see <link>

#### Description

✔️ DO include a short description for your package that describes what it's for.
> Package descriptions are one of the most prominent fields surfaced in NuGet search and will likely be the first thing a potential consumers looks at to determine if a package is right for them.

#### Copyright

✔️ CONSIDER copyrighting your package with "Copyright name/company year"
>A copyright notice essentially indicates that your work cannot be copied without your permission. Including, a copyright notice in your package is easy and won't do any harm! 

Example: Copyright Contoso 2020

#### Licensing

✔️ DO specify a valid license for your package.
> [!IMPORTANT]
> A project without a license defaults to [exclusive copyright](https://choosealicense.com/no-permission/), making it legally impossible for other people to use.

✔️ CONSIDER using a license expression (SPDX identifier).
> License expressions are surfaced the most clearly to package consumers and make it clear if the license has changed with a new release.

✔️ CONSIDER specifying an MIT license.
> If you want your package as to be usable by as many consumers as possible, the MIT license is a very well known non-restrictive license.

#### Project URL

✔️ CONSIDER including a link to an associated project website or repository.
> If you have a non-repository website associated with your package, feel free to include it. Repository URLs (GitHub, BitBucket, etc.) belong in the `Repository URL` field. This is especially helpful is your repository is private!

#### Icon

✔️ DO use a package icon image that is 128x128 and has a transparent background (PNG) for best viewing results.
> NuGet will automatically scale your image to the client it is being displayed on.

#### Repository Type and URL

If your have a public repository for the your package source code, you should...

✔️ CONSIDER setting up [Source Link](https://docs.microsoft.com/en-us/dotnet/standard/library-guidance/sourcelink) to add source control metadata to your assemblies and NuGet package.
> Source Link will automatically adds `Repository URL` and `Repository Type` to the package metadata. It also adds the specific commit associated with your package version.

However, even if you don't want to use Source Link to automatically add your repository URL and type to the package metadata, filling them in yourself can still be very helpful! 

#### Tags

✔️ DO include tags with key terms related to your package to enhance package discoverability.
> Tags are taken into account in NuGet's search algorithm and are especially helpful for terms that are not in the Package ID.

#### Release Notes

✔️ CONSIDER including release notes with each update describing what changes were made.
> While there is no specific format required for release notes, we recommend including:
> 1. Breaking changes (very helpful)
> 2. Added features
> 3. Bug Fixes

## Dependency Management

#### Lean Packages

✔️ DO review your package for unnecessary dependencies.
> Unnecessary dependencies needlessly increase the size of your package as well as increase likelihood of version conflicts!

#### Version Ranges



## Publishing

### Packing

✔️ DO pack your package using Visual Studio `

### Security