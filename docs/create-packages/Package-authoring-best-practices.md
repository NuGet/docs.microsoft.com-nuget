---
title: Package authoring best practices
description: A general guide of best practices for creating high quality NuGet packages.
author: chgill-MSFT
ms.author: chgill
ms.date: 09/17/2020
ms.topic: conceptual
---

# Package authoring best practices

This guidance is intended to give NuGet package authors a lightweight reference to create and publish high-quality packages. It will primarily focus on package-specific best practices such as metadata and packing. For more in-depth suggestions for building high quality libraries, see the .NET [Open-source library guidance](https://docs.microsoft.com/dotnet/standard/library-guidance/).

## Types of recommendations

Each article presents four types of recommendations: **Do**, **Consider**, **Avoid**, and **Do not**. The type of recommendation indicates how closely it should be followed.

You should almost always follow a **Do** recommendation. For example:

✔️ DO include a short description for your package that describes what it's for.

On the other hand, **Consider** recommendations should generally be followed, but there are legitimate exceptions to the rule:

✔️ CONSIDER choosing a NuGet package name with a prefix that meets NuGet's prefix reservation [criteria](https://docs.microsoft.com/nuget/reference/id-prefix-reservation).

**Avoid** recommendations mention things that are generally not a good idea, but breaking the rule sometimes makes sense:

❌ AVOID NuGet package references that demand an exact version.

And finally, **Do not** recommendations indicate something you should almost never do:

❌ DO NOT use the `LicenseUrl` metadata property.

## Create a NuGet package

The latest recommended way to to create a NuGet package is from an [SDK-style project](https://docs.microsoft.com/nuget/resources/check-project-format). SDK-style project properties, including [target framework](https://docs.microsoft.com/dotnet/standard/frameworks) and [package metadata](#package-metadata), are defined in the [project file](https://docs.microsoft.com/visualstudio/ide/solutions-and-projects-in-visual-studio#project-file).

Create a package from your SDK-style project by defining the required properties and packing in [Visual Studio](https://docs.microsoft.com/nuget/quickstart/create-and-publish-a-package-using-visual-studio?tabs=netcore-cli) or the [dotnet CLI](https://docs.microsoft.com/nuget/quickstart/create-and-publish-a-package-using-the-dotnet-cli).

✔️ DO create an SDK-style project and create (pack) your package using Visual Studio or the dotnet CLI.

For more detailed guidance regarding package creation including necessary client tools, project file example, and commands, see [Create a NuGet package using the dotnet CLI](https://docs.microsoft.com/nuget/create-packages/creating-a-package-dotnet-cli).

To help decide which .NET frameworks to target, see our [latest guidance for cross-platform targeting](https://docs.microsoft.com/dotnet/standard/library-guidance/cross-platform-targeting).

## Package metadata

Metadata is a foundational component of any NuGet package. The quality of your metadata can vastly influence the discoverability, usability, and trustworthiness of your package.

In Visual Studio, the recommended way to specify package metadata is to go Project > [Project Name] Properties > Package.

Package metadata elements can also be [specified directly in the project file](https://docs.microsoft.com/nuget/create-packages/creating-a-package-msbuild#set-properties).

Below is a table mapping and describing available package metadata elements:

| Visual Studio property name                   | [Project file/ MSBuild property name](https://docs.microsoft.com/dotnet/core/tools/csproj#packagereleasenotes)                          | [Nuspec property name](https://docs.microsoft.com/nuget/reference/nuspec#general-form-and-schema) | Description                                                                                                       |
|-----------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------|
| [`Package id`](#package-id)                   | [`PackageId`](https://docs.microsoft.com/dotnet/core/tools/csproj#packageid)                                                            | [`id`](https://docs.microsoft.com/nuget/reference/nuspec#id)                                      | The package name or identifier.                    |
| [`Package version`](#package-version)         | [`PackageVersion`](https://docs.microsoft.com/dotnet/core/tools/csproj#packageversion)                                                  | [`version`](https://docs.microsoft.com/nuget/reference/nuspec#version)                            | NuGet package version.                                           |
| [`Authors`](#authors)                         | [`Authors`](https://docs.microsoft.com/dotnet/core/tools/csproj#authors)                                                                | [`authors`](https://docs.microsoft.com/nuget/reference/nuspec#authors)                            | A comma-separated list of package authors, often using the individual's or an organization's "pretty name."                             |
| [`Description`](#description)                 | [`Description`](https://docs.microsoft.com/dotnet/core/tools/csproj#description)                                                        | [`description`](https://docs.microsoft.com/nuget/reference/nuspec#description)                    | A description of the package.                                                                |
| [`Copyright`](#copyright)                     | [`Copyright`](https://docs.microsoft.com/dotnet/core/tools/csproj#copyright)                                                            | [`copyright`](https://docs.microsoft.com/nuget/reference/nuspec#copyright)                        | Copyright details for the package.                                                                      |
| [`Licensing - Expression`](#licensing)        | [`PackageLicenseExpression`](https://docs.microsoft.com/nuget/reference/msbuild-targets#packing-a-license-expression-or-a-license-file) | [`license type="expression"`](https://docs.microsoft.com/nuget/reference/nuspec#license)          | An SPDX license expression.       |
| [`Licensing - File`](#licensing)              | [`PackageLicenseFile`](https://docs.microsoft.com/nuget/reference/msbuild-targets#packing-a-license-expression-or-a-license-file)       | [`license type="file"`](https://docs.microsoft.com/nuget/reference/nuspec#license)                | Path to a custom license file.                                               |
| [`Project URL`](#project-url)                 | `PackageProjectUrl`                                                                                                                     | [`projectUrl`](https://docs.microsoft.com/nuget/reference/nuspec#projecturl)                      | A URL for the project homepage.                                                                                   |
| [`Icon File`](#icon)                          | [`PackageIcon`](https://docs.microsoft.com/nuget/reference/msbuild-targets#packing-an-icon-image-file)                                  | [`icon`](https://docs.microsoft.com/nuget/reference/nuspec#icon)                                  | Path to the package icon image file.                                                                      |
| [`Repository URL`](#repository-type-and-url)  | [`RepositoryUrl`](https://docs.microsoft.com/dotnet/core/tools/csproj#repositoryurl)                                                    | [`repository url`](https://docs.microsoft.com/nuget/reference/nuspec#repository)               | URL to the repository from which the package was built.                                                           |
| [`Repository type`](#repository-type-and-url) | [`RespositoryType`](https://docs.microsoft.com/dotnet/core/tools/csproj#repositorytype)                                                 | [`repository type`](https://docs.microsoft.com/nuget/reference/nuspec#repository)              | Type of repository the repository URL is pointing to (i.e. "git").                                                   |
| [`Tags`](#tags)                               | [`PackageTags`](https://docs.microsoft.com/dotnet/core/tools/csproj#packagetags)                                                        | [`tags`](https://docs.microsoft.com/nuget/reference/nuspec#tags)                                  | A space-delimited list of tags and keywords that describe the package. Tags are used when searching for packages. |
| [`Release notes`](#release-notes)             | [`PackageReleaseNotes`](https://docs.microsoft.com/dotnet/core/tools/csproj#packagereleasenotes)                                          | [`releaseNotes`](https://docs.microsoft.com/nuget/reference/nuspec#releasenotes)                  | A description of the changes made in this release of the package.                                                 |  |

### Package ID

If you're publishing a completely new package:

✔️ DO choose a package ID that is unique and clearly differentiated from existing packages on NuGet.org.
> You can check if a package ID is unique and differentiable by searching for the ID on NuGet.org or checking if the following link exists: https://www.nuget.org/packages/<package name\>.

✔️ CONSIDER choosing a NuGet package name with a prefix that meets NuGet's [prefix reservation criteria](https://docs.microsoft.com/nuget/nuget-org/id-prefix-reservation#id-prefix-reservation-criteria).
> Reserving the prefix ID for your package will let you get the verified check mark:
> ![image](media/Verified-check-mark.png)
> 
> Check out the [Package ID prefix reservation docs](https://docs.microsoft.com/nuget/nuget-org/id-prefix-reservation) to learn more.

### Package Version

✔️ CONSIDER using [SemVer](https://semver.org/) to version your NuGet package.
> Essentially, this means using the Major.Minor.Patch[-prerelease] format.

✔️ DO publish a package as a [pre-release package](https://docs.microsoft.com/nuget/create-packages/prerelease-packages) if it is non-stable or a preview.

See the [.NET library versioning guide](https://docs.microsoft.com/dotnet/standard/library-guidance/versioning) for more advanced guidance.

### Authors

✔️ DO use the author field for your or your organization's "pretty name."
> For example, if my NuGet.org username is "jdoe" then using "Jane Doe" for the author field may help consumers recognize me as an author. If my organization's NuGet.org username is "ContosoToolkit" then using "Contoso Corporation" may be more recognizable and inspire more consumer trust.
### Description

✔️ DO include a short description (up to 4000 characters) to describe your package.
> Package descriptions are one of the most prominent fields surfaced in NuGet search and will likely be the first thing potential consumers looks at to determine if a package is right for them.

### Copyright

✔️ CONSIDER copyrighting your package with "Copyright (c) <name/company\> <year\>."
>A copyright notice essentially indicates that your work cannot be copied without your permission. Including a copyright notice in your package is easy and won't do any harm!

Example: Copyright (c) Contoso 2020

### Licensing

✔️ DO [include a license expression or license file in your package](https://docs.microsoft.com/nuget/reference/msbuild-targets#packing-a-license-expression-or-a-license-file).
> [!IMPORTANT]
> A project without a license defaults to [exclusive copyright](https://choosealicense.com/no-permission/), meaning that you have not granted anyone permission to use your project.

❌ DO NOT use the deprecated `LicenseUrl` metadata property.
> This presents legal ambiguity as license changes at the URL will retroactively change the displayed license for previous package versions.

#### If your package is [open source](https://opensource.org/osd)

✔️ DO [choose an open source license](https://choosealicense.com/) to make your package open source.
> *"Open source licenses are licenses that comply with the Open Source Definition — in brief, they allow software to be freely used, modified, and shared."* - Open Source Initiative. To learn more about open source software and the Open Source Initiative, check out https://opensource.org/.

✔️ CONSIDER [including a license expression in your package](https://docs.microsoft.com/nuget/reference/msbuild-targets#packing-a-license-expression-or-a-license-file).
> License expressions are surfaced the most clearly and make it more obvious to consumers if they can use your package or if the license has changed. 
> [!Note]
> NuGet.org only accepts license expressions for licenses that are approved by the Open Source Initiative or the Free Software Foundation.

#### If your package is not open source

✔️ DO [include a license file in your package](https://docs.microsoft.com/nuget/reference/msbuild-targets#packing-a-license-expression-or-a-license-file).
> Any license file (.txt or .md) can be added to your package, including non-standard licenses. 

### Project URL

✔️ CONSIDER including a link to an associated project, repository, or company website.
> Your project site should have everything users need to know about your package and will likely be where users look for documentation.

### Icon

✔️ CONSIDER [including an icon with your package](https://docs.microsoft.com/nuget/reference/msbuild-targets#packing-an-icon-image-file) to help visually differentiate it. It's a relatively small addition that can improve perception of package quality.
> Icons can be specific to individual packages or be a brand logo.

✔️ DO use an image that is 128x128 and has a transparent background (PNG) for best viewing results.
> NuGet will automatically scale your image to the client it is being displayed on.

❌ DO NOT use the deprecated `IconUrl` metadata property.

### Repository Type and URL

✔️ CONSIDER setting up [Source Link](https://docs.microsoft.com/dotnet/standard/library-guidance/sourcelink) to automatically add source control metadata to your NuGet package and make your library easier to debug.
> Source Link automatically adds `Repository URL` and `Repository Type` to the package metadata. It also adds the specific commit associated with your package version.

### Tags

✔️ DO include several tags with key terms related to your package to enhance discoverability.
> Tags are taken into account in NuGet.org's search algorithm and are especially helpful for terms that are not in the Package ID but are relevant.

For example, if I published a package to log strings to the console, I would include: "logging, log, console, string, output"

### Release notes

✔️ CONSIDER including release notes with each update describing what changes were made.
> While there is no specific format required for release notes, we recommend including:
>
> 1. Breaking changes
> 2. New features
> 3. Bug fixes
> 
> If you already track release notes or a changelog in your repo, you can also include a link to the relevant file.

## Related topics

- [Create and publish a package (dotnet CLI)](https://docs.microsoft.com/nuget/quickstart/create-and-publish-a-package-using-the-dotnet-cli)
- [Create and publish a package (Visual Studio)](https://docs.microsoft.com/nuget/quickstart/create-and-publish-a-package-using-visual-studio?tabs=netcore-cli)
