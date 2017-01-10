---
# required metadata

title: NuGet Packages and Source Control | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 1/9/2017
ms.topic: article
ms.prod: nuget
#ms.service:
ms.technology: nuget
ms.assetid: 2c874e6f-99eb-46dd-997f-f67d98d0237e

# optional metadata

#description:
#keywords:
#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer:
- karann
- unnir
#ms.suite:
#ms.tgt_pltfrm:
#ms.custom:

---

# Packages and source control

Developers typically omit binaries, such as NuGet packages, from their source control repository and rely instead on [package restore](../consume-packages/package-restore.md) to reinstall a project's dependencies before doing a build.

The reasons for doing so include the following:

1. Distributed version control systems, such as Git, include full copies of every version of every file within the repository. Binary files that are frequently updated will lead to significant bloat and lengthens the time it takes to clone the repository.
1. When packages are included in the repository, developers are liable to add references directly to package contents on disk rather than referencing packages through NuGet, which can lead to hard-coded path names in the project.
1. It becomes harder to "clean" your solution of any unused package folders, as you need to ensure you don't delete any package folders still in use.
1. By omitting packages, you maintain clean boundaries of ownership between your code and the packages from others that you depend upon. Many NuGet packages are maintained in their own source control repositories already.

Note that although package restore is the default behavior with NuGet, some manual work is necessary to omit packages (the `packages` folder in your project) from source control, as described in the following sections.

## Omitting packages with Git

Use the [.gitignore file](https://www.kernel.org/pub/software/scm/git/docs/gitignore.html) to have Git ignore the contents of the `packages` folder. For reference, see the [sample `.gitignore` for Visual Studio projects](https://github.com/github/gitignore/blob/master/VisualStudio.gitignore).

The important parts of the `.gitignore` file are:

    # Ignore NuGet Packages
    *.nupkg

    # Ignore the packages folder
    **/packages/*

    # except build/, which is used as an MSBuild target.
    !**/packages/build/

    # Uncomment if necessary; generally it will be regenerated when needed
    #!**/packages/repositories.config

    # Ignore other intermediate files that NuGet might create
    project.json.lock
    *.nuget.props

## Omitting packages with Team Foundation Version Control

> [!Note]
> Follow these instructions if possible *before* adding your project to source control. Otherwise, manually delete the `packages` folder from your repository and check in that change before continuing.

To disable source control integration with TFVC for selected files:

1. Create a folder called `.nuget` in your solution folder (where the `.sln` file is).
    * Tip: on Windows, to create this folder in Windows Explorer, use the name `.nuget.` *with* the training dot.
1. In that folder, create a file named `NuGet.config` and open it for editing.
1. Add the following text as a minimum, where the [disableSourceControlIntegration](../schema/nuget.config-file.md#solution-section) setting instructs Visual Studio to skip everything in the `packages` folder:

        <?xml version="1.0" encoding="utf-8"?>
        <configuration>
            <solution>
                <add key="disableSourceControlIntegration" value="true" />
            </solution>
        </configuration>

1. If you are using TFS 2010 or earlier, cloak the `packages` folder in your workspace mappings.
1. On TFS 2012 or later, or with Visual Studio Team Services, add a [`.tfignore`](https://msdn.microsoft.com/en-us/library/ms245454.aspx#tfignore) file with the content below to explicitly ignore modifications to the `\packages` folder on the repository level and a few other intermediate files. (You can create the file in Windows Explorer using the name a `.tfignore.` with the trailing dot, but you might need to disable the "Hide known file extensions" option first.):

        # Ignore the NuGet packages folder in the root of the repository.
        # If needed, prefix 'packages' with additional folder names if it's
        # not in the same folder as .tfignore.
        packages

        # Include package target files which may be required for msbuild,
        # again prefixing the folder name as needed.
        !packages/*.targets

1. Add `NuGet.config` and `.tfignore` to source control and check in your changes.
