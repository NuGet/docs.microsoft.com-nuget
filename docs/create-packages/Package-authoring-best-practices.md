---
title: Package authoring best practices
description: A general guide of best practices for creating high quality NuGet packages.
author: nkolev92
ms.author: nikolev
ms.date: 11/15/2021
ms.topic: best-practice
---

# Package authoring best practices

This guidance is intended to give NuGet package authors a lightweight reference to create and publish high-quality packages. It will primarily focus on package-specific best practices such as metadata and packing. For more in-depth suggestions for building high quality libraries, see the .NET [Open-source library guidance](/dotnet/standard/library-guidance/).

## Types of recommendations

Each article presents four types of recommendations: **Do**, **Consider**, **Avoid**, and **Do not**. The type of recommendation indicates how closely it should be followed.

You should almost always follow a **Do** recommendation. For example:

✔️ DO include a short description for your package that describes what it's for.

On the other hand, **Consider** recommendations should generally be followed, but there are legitimate exceptions to the rule:

✔️ CONSIDER choosing a NuGet package name with a prefix that meets NuGet's prefix reservation [criteria](../nuget-org/id-prefix-reservation.md).

**Avoid** recommendations mention things that are generally not a good idea, but breaking the rule sometimes makes sense:

❌ AVOID NuGet package references that demand an exact version.

And finally, **Do not** recommendations indicate something you should almost never do:

❌ DO NOT use the `LicenseUrl` metadata property.

## Create a NuGet package

The latest recommended way to to create a NuGet package is from an [SDK-style project](../resources/check-project-format.md). SDK-style project properties, including [target framework](/dotnet/standard/frameworks) and [package metadata](#package-metadata), are defined in the [project file](/visualstudio/ide/solutions-and-projects-in-visual-studio#project-file).

Create a package from your SDK-style project by defining the required properties and packing in [Visual Studio](../quickstart/create-and-publish-a-package-using-visual-studio.md?tabs=netcore-cli) or the [dotnet CLI](../quickstart/create-and-publish-a-package-using-the-dotnet-cli.md).

✔️ DO create an SDK-style project and create (pack) your package using Visual Studio or the dotnet CLI.

For more detailed guidance regarding package creation including necessary client tools, project file example, and commands, see [Create a NuGet package using the dotnet CLI](./creating-a-package-dotnet-cli.md).

To help decide which .NET frameworks to target, see our [latest guidance for cross-platform targeting](/dotnet/standard/library-guidance/cross-platform-targeting).

## Package metadata

Metadata is a foundational component of any NuGet package. The quality of your metadata can vastly influence the discoverability, usability, and trustworthiness of your package.

In Visual Studio, the recommended way to specify package metadata is to go Project > [Project Name] Properties > Package.

Package metadata elements can also be [specified directly in the project file](./creating-a-package-msbuild.md#set-properties).

Below is a table mapping and describing available package metadata elements:

| Visual Studio property name                   	| [Project file/ MSBuild property name](/dotnet/core/tools/csproj#packagereleasenotes)                          	| [Nuspec property name](/nuget/reference/nuspec#general-form-and-schema) 	| Description                                                                                                       	|
|-----------------------------------------------	|-----------------------------------------------------------------------------------------------------------------------------------------	|---------------------------------------------------------------------------------------------------	|-------------------------------------------------------------------------------------------------------------------	|
| [`Package id`](#package-id)                   	| [`PackageId`](/nuget/reference/msbuild-targets#pack-target)                                                            	| [`id`](/nuget/reference/nuspec#id)                                      	| The package name or identifier.                                                                                   	|
| [`Package version`](#package-version)         	| [`PackageVersion`](/nuget/reference/msbuild-targets#pack-target)                                                  	| [`version`](/nuget/reference/nuspec#version)                            	| NuGet package version.                                                                                            	|
| [`Authors`](#authors)                         	| [`Authors`](/nuget/reference/msbuild-targets#pack-target)                                                                	| [`authors`](/nuget/reference/nuspec#authors)                            	| A comma-separated list of package authors, often using the individual's or an organization's "pretty name."       	|
| [`Description`](#description)                 	| [`Description`](/nuget/reference/msbuild-targets#pack-target)                                                        	| [`description`](/nuget/reference/nuspec#description)                    	| A description of the package.                                                                                     	|
| [`Copyright`](#copyright)                     	| [`Copyright`](/nuget/reference/msbuild-targets#pack-target)                                                            	| [`copyright`](/nuget/reference/nuspec#copyright)                        	| Copyright details for the package.                                                                                	|
| [`Project URL`](#project-url)                 	| [`PackageProjectUrl`](/nuget/reference/msbuild-targets#pack-target)                                                       |      [`projectUrl`](/nuget/reference/nuspec#projecturl)                      	| A URL for the project homepage.                                                                                   	|
| [`Icon File`](#icon)                          	| [`PackageIcon`](/nuget/reference/msbuild-targets#packing-an-icon-image-file)                                  	| [`icon`](/nuget/reference/nuspec#icon)                                  	| Path to the package icon image file.                                                                              	|
| [`README`](#readme)                          	| [`PackageReadmeFile`](/nuget/reference/msbuild-targets#packagereadmefile)                                  	| [`readme`](/nuget/reference/nuspec#readme)                                  	| Path to the package README markdown file.                                                                              	|
| [`Repository URL`](#repository-type-and-url)  	| [`RepositoryUrl`](/nuget/reference/msbuild-targets#pack-target)                                                    	| [`repository url`](/nuget/reference/nuspec#repository)                  	| URL to the repository from which the package was built.                                                           	|
| [`Repository type`](#repository-type-and-url) 	| [`RepositoryType`](/nuget/reference/msbuild-targets#pack-target)                                                 	| [`repository type`](/nuget/reference/nuspec#repository)                 	| Type of repository the repository URL is pointing to (i.e. "git").                                                	|
| [`Tags`](#tags)                               	| [`PackageTags`](/nuget/reference/msbuild-targets#pack-target)                                                        	| [`tags`](/nuget/reference/nuspec#tags)                                  	| A space-delimited list of tags and keywords that describe the package. Tags are used when searching for packages. 	|
| [`Release notes`](#release-notes)             	| [`PackageReleaseNotes`](/nuget/reference/msbuild-targets#pack-target)                                        	| [`releaseNotes`](/nuget/reference/nuspec#releasenotes)                  	| A description of the changes made in this release of the package.                                                 	|
| [`Licensing - Expression`](#licensing)        	| [`PackageLicenseExpression`](/nuget/reference/msbuild-targets#packing-a-license-expression-or-a-license-file) 	| [`license type="expression"`](/nuget/reference/nuspec#license)          	| An SPDX license expression.                                                                                       	|
| [`Licensing - File`](#licensing)              	| [`PackageLicenseFile`](/nuget/reference/msbuild-targets#packing-a-license-expression-or-a-license-file)       	| [`license type="file"`](/nuget/reference/nuspec#license)                	| Path to a custom license file.                                                                                    	|
### Package ID

If you're publishing a completely new package:

✔️ DO choose a package ID that is unique and clearly differentiated from existing packages on NuGet.org.
> You can check if a package ID is unique and differentiable by searching for the ID on NuGet.org or checking if the following link exists: https://www.nuget.org/packages/<package name\>.

✔️ CONSIDER choosing a NuGet package name with a prefix that meets NuGet's [prefix reservation criteria](../nuget-org/id-prefix-reservation.md#id-prefix-reservation-criteria).
> Reserving the prefix ID for your package will let you get the verified check mark:
> ![image](media/Verified-check-mark.png)
> 
> Check out the [Package ID prefix reservation docs](../nuget-org/id-prefix-reservation.md) to learn more.

### Package Version

✔️ CONSIDER using [SemVer](https://semver.org/) to version your NuGet package.
> Essentially, this means using the Major.Minor.Patch[-prerelease] format.

✔️ DO publish a package as a [pre-release package](./prerelease-packages.md) if it is non-stable or a preview.

See the [.NET library versioning guide](/dotnet/standard/library-guidance/versioning) for more advanced guidance.

### Authors

✔️ DO use the author field for your or your organization's "pretty name."
> For example, if my NuGet.org username is "jdoe" then using "Jane Doe" for the author field may help consumers recognize me as an author. If my organization's NuGet.org username is "ContosoToolkit" then using "Contoso Corporation" may be more recognizable and inspire more consumer trust.

### Description

✔️ DO include a short description (up to 4000 characters) to describe your package.
> Package descriptions are one of the most prominent fields surfaced in NuGet search and will likely be the first thing potential consumers looks at to determine if a package is right for them.

### Copyright

✔️ DO add a copyright notice to your package with "Copyright (c) <name/company\> <year\>."
> A copyright notice essentially indicates that your work cannot be copied without your permission. Including a copyright notice in your package is easy and won't do any harm!

Example: Copyright (c) Contoso 2020

### Project URL

✔️ DO include a link to an associated project, repository, or company website.
> Your project site should have everything users need to know about your package and will likely be where users look for documentation.

### Icon

✔️ CONSIDER [including an icon with your package](../reference/msbuild-targets.md#packing-an-icon-image-file) to help visually differentiate it. It's a relatively small addition that can improve perception of package quality.
> Icons can be specific to individual packages or be a brand logo.

✔️ DO use an image that is 128x128 and has a transparent background (PNG) for best viewing results.
> NuGet will automatically scale your image to the client it is being displayed on.

❌ DO NOT use the deprecated `IconUrl` metadata property.

### README
✔️ DO [add a README markdown file](/nuget/reference/msbuild-targets#packagereadmefile) that provides an overview of what your package does and how to get started.
> A package README will significantly improve the quality perception of your package as well as new user onboarding. Also consider [previewing your README](../nuget-org/package-readme-on-nuget-org.md#preview-your-readme) before you upload it! See [how to include a README file in your NuGet package](/nuget/reference/msbuild-targets#packagereadmefile) for more details.

### Repository Type and URL

✔️ CONSIDER setting up [Source Link](/dotnet/standard/library-guidance/sourcelink) to automatically add source control metadata to your NuGet package and make your library easier to debug.
> Source Link automatically adds `Repository URL` and `Repository Type` to the package metadata. It also adds the specific commit associated with your package version.

### Tags

✔️ DO include several tags with key terms related to your package to enhance discoverability.
> Tags are taken into account in NuGet.org's search algorithm and are especially helpful for terms that are not in the Package ID but are relevant.

For example, if I published a package to log strings to the console, I would include: "logging, log, console, string, output"

### Release notes

✔️ DO include release notes with each update describing what changes were made.
> While there is no specific format required for release notes, we recommend including:
>
> 1. Breaking changes
> 2. New features
> 3. Bug fixes
> 
> If you already track release notes or a changelog in your repo, you can also include a link to the relevant file.

### Licensing

✔️ DO [include a license expression or license file in your package](../reference/msbuild-targets.md#packing-a-license-expression-or-a-license-file).
> [!IMPORTANT]
> A project without a license defaults to [exclusive copyright](https://choosealicense.com/no-permission/), meaning that you have not granted anyone permission to use your project.

❌ DO NOT use the deprecated `LicenseUrl` metadata property.
> This presents legal ambiguity as license changes at the URL will retroactively change the displayed license for previous package versions.

#### If your package is [open source](https://opensource.org/osd)

✔️ DO [choose an open source license](https://choosealicense.com/) to make your package open source.
> *"Open source licenses are licenses that comply with the Open Source Definition — in brief, they allow software to be freely used, modified, and shared."* - Open Source Initiative. To learn more about open source software and the Open Source Initiative, check out https://opensource.org/.

✔️ CONSIDER [including a license expression in your package](../reference/msbuild-targets.md#packing-a-license-expression-or-a-license-file).
> License expressions are surfaced the most clearly and make it more obvious to consumers if they can use your package or if the license has changed. 
> [!Note]
> NuGet.org only accepts license expressions for licenses that are approved by the Open Source Initiative or the Free Software Foundation.

#### If your package is not open source

✔️ DO [include a license file in your package](../reference/msbuild-targets.md#packing-a-license-expression-or-a-license-file).
> Any license file (.txt or .md) can be added to your package, including non-standard licenses. 

## Related topics

- [Create and publish a package (dotnet CLI)](../quickstart/create-and-publish-a-package-using-the-dotnet-cli.md)
- [Create and publish a package (Visual Studio)](../quickstart/create-and-publish-a-package-using-visual-studio.md?tabs=netcore-cli)
