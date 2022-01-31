---
title: How to Publish a NuGet Package
description: Detailed instructions for how to publish a NuGet package to nuget.org or private feeds, and how to manage package ownership on nuget.org.
author: JonDouglas
ms.author: jodou
ms.date: 05/18/2018
ms.topic: conceptual
ms.reviewer: anangaur
---

# Publishing packages

Once you have created a package and have your `.nupkg` file in hand, it's a simple process to make it available to other developers, either publicly or privately:

- Public packages are made available to all developers globally through [nuget.org](https://www.nuget.org/packages/manage/upload) as described in this article (requires NuGet 4.1.0+).
- Private packages are available to only a team or organization, by hosting them either a file share, a private NuGet server, [Azure Artifacts](https://www.visualstudio.com/docs/package/nuget/publish), or a third-party repository such as myget, ProGet, Nexus Repository, and Artifactory. For additional details, see [Hosting Packages Overview](../hosting-packages/overview.md).

This article covers publishing to nuget.org; for publishing to Azure Artifacts, see [Package Management](https://www.visualstudio.com/docs/package/nuget/publish).

## Publish to nuget.org

For nuget.org, you must sign in with a Microsoft account, with which you'll be asked to register the account with nuget.org.

![NuGet sign in location](media/publish_NuGetSignIn.png)

Next, you can either upload the package through the nuget.org web portal, push to nuget.org from the command line (requires `nuget.exe` 4.1.0+) , or publish as part of a CI/CD process through Azure DevOps Services, as described in the following sections.

### Web portal: use the Upload Package tab on nuget.org

1. Select **Upload** on the top menu of nuget.org and browse to the package location.

    ![Upload a package on nuget.org](media/publish_UploadYourPackage.PNG)

1. nuget.org tells you if the package name is available. If it isn't, change the package identifier in your project, rebuild, and try the upload again.

1. If the package name is available, nuget.org opens a **Verify** section in which you can review the metadata from the package manifest. If you included a [readme file](../nuget-org/package-readme-on-nuget-org.md) in your package, check out the preview to ensure all content is being rendered properly. To change any of the metadata, edit your project (project file or `.nuspec` file), rebuild, recreate the package, and upload again.

2. When all the information is ready, select the **Submit** button

### Command line

To push packages to nuget.org, you first need an API key, which is created on nuget.org. You must use either dotnet.exe (.NET Core), or nuget.exe v4.1.0 or above, which implement the required NuGet protocols.
For more information, see [.NET Core](/dotnet/core/install/), [nuget.exe](https://www.nuget.org/downloads), and [NuGet protocols](../api/nuget-protocols.md).

#### Create API keys

[!INCLUDE [publish-api-key](../quickstart/includes/publish-api-key.md)]

#### Publish with dotnet nuget push

[!INCLUDE [publish-dotnet](../quickstart/includes/publish-dotnet.md)]

#### Publish with nuget push

1. At a command prompt, run the following command, replacing `<your_API_key>` with the key obtained from nuget.org:

    ```cli
    nuget setApiKey <your_API_key>
    ```

    This command stores your API key in your NuGet configuration so that you don't need to repeat this step again on the same computer.

    > [!NOTE]
    > API key is not used for authenticating with the private feed. Refer to [`nuget sources` command](../reference/cli-reference/cli-ref-sources.md) to manage credentials for authenticating with the source.
    > API keys can be obtained from the individual NuGet servers. To create and manange APIKeys for nuget.org refer to [Create API keys](#create-api-keys).

1. Push your package to NuGet Gallery using the following command:

    ```cli
    nuget push YourPackage.nupkg -Source https://api.nuget.org/v3/index.json
    ```

#### Publish signed packages

To submit signed packages, you must first [register the certificate](../create-packages/Sign-a-Package.md#register-the-certificate-on-nugetorg) used for signing the packages. 

> [!Warning]
> nuget.org rejects packages that don't satisfy the [signed package requirements](../reference/Signed-Packages-Reference.md#signature-requirements-on-nugetorg).

### Package validation and indexing

Packages pushed to nuget.org undergo several validations, such as virus checks. (All packages on nuget.org are periodically scanned.)

When the package has passed all validation checks, it might take a while for it to be indexed and appear in search results. Once indexing is complete, you receive an email confirming that the package was successfully published. If the package fails a validation check, the package details page will update to display the associated error and you also receive an email notifying you about it.

Package validation and indexing usually takes under 15 minutes. If the package publishing is taking longer than expected, visit [status.nuget.org](https://status.nuget.org/) to check if nuget.org is experiencing any interruptions. If all systems are operational and the package hasn't been successfully published within an hour, please login to nuget.org and contact us using the Contact Support link on the package page.

To see the status of a package, select **Manage packages** under your account name on nuget.org. You receive a confirmation email when validation is complete.

Note that it might take a while for your package to be indexed and appear in search results where others can find it, during which time you see the following message on your package page:

![Message indicating a package is not yet published](media/publish_NotYetIndexed.png)

### Azure DevOps Services (CI/CD)

If you push packages to nuget.org using Azure DevOps Services as part of your Continuous Integration/Deployment process, you must use `nuget.exe` 4.1 or above in the NuGet tasks. Details can be found on [Using the latest NuGet in your build](https://blogs.msdn.microsoft.com/devops/2017/09/29/using-the-latest-nuget-in-your-build/) (Microsoft DevOps blog).

## Managing package owners on nuget.org

Although each NuGet package's `.nuspec` file defines the package's authors, the nuget.org gallery does not use that metadata to define ownership. Instead, nuget.org assigns initial ownership to the person who publishes the package. This is either the logged-in user who uploaded the package through the nuget.org UI, or the users whose API key was used with `nuget SetApiKey` or `nuget push`.

All package owners have full permissions for the package, including adding and removing other owners, and publishing updates.

To change ownership of a package, do the following:

1. Sign in to nuget.org with the account that is the current owner of the package.
1. Select your account name, select **Manage packages**, and expand **Published Packages**.
1. Select on the package you want to manage, then on the right side select **Manage owners**.

From here you have several options:

1. Remove any owner listed under **Current Owners**.
1. Add an owner under **Add Owner** by entering their user name, a message, and selecting **Add**. This action sends an email to that new co-owner with a confirmation link. Once confirmed, that person has full permissions to add and remove owners. (Until confirmed, the **Current Owners** section indicates pending approval for that person.)
1. To transfer ownership (as when ownership changes or a package was published under the wrong account), add the new owner, and once they've confirmed ownership they can remove you from the list.

To assign ownership to a company or group, create a nuget.org account using an email alias that is forwarded to the appropriate team members. For example, various Microsoft ASP.NET packages are co-owned by the [microsoft](https://nuget.org/profiles/microsoft) and [aspnet](https://nuget.org/profiles/aspnet) accounts, which simply such aliases.

### Recovering package ownership

Occasionally, a package may not have an active owner. For example, the original owner may have left the company that produces the package, nuget.org credentials are lost, or earlier bugs in the gallery left a package ownerless.

If you are the rightful owner of a package and need to regain ownership, use the [contact form](https://www.nuget.org/policies/Contact) on nuget.org to explain your situation to the NuGet team. We then follow a process to verify your ownership of the package, including trying to locate the existing owner through the package's Project URL, Twitter, email, or other means. But if all else fails, we can send you a new invite to become an owner.
