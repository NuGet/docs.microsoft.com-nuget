---
title: Your organization on NuGet.org
description: Organizations on NuGet.org helps you to manage packages published by group or in a team, company environment.
author: anangaur
ms.author: anangaur
ms.date: 04/10/2018
ms.topic: concept-article
ms.reviewer: 
  - kraigb
  - camsoper
ms.custom: sfi-image-nochange
---

# Your organization on NuGet.org

Organizations enable businesses and open-source projects to collaborate on packages using a single NuGet.org identity. For a package consumer, an organization account appears same as an existing user account on NuGet.org.

## Organization accounts vs. individual accounts

An organization account has one or more individual (user) accounts as its members. These members can manage a set of packages while maintaining a single identity for ownership.

Your individual account is your identity on NuGet.org and can be a member of any number of organizations. A package can belong to an organization account like it can belong to an individual account. Package consumers don't see any difference between an individual account or the organization account: both appear as package `owners`.

## Adding a new organization

To add a new organization, select your account on NuGet.org, then select the **Manage Organizations...** menu command:

![Menu option on NuGet.org for Manager Organizations](media/org-manage-option.png)

On the next page, select the **Add new organization** button:

![Button to create a new organization on NuGet.org](media/org-add-new-option.png)

On the next page, provide the organization name and email address. Since organization accounts share the same namespace as user accounts, the organization name must be different from any other existing organization or user accounts. The email address must also be unique across all accounts.

![Add new organization page on NuGet.org](media/org-add-new-page.png)

Once the organization account is created, you are the administrator and can submit packages for the organization and add organization members.

### Transform existing account to an organization

> [!Warning]
> Account conversion is irreversible: you cannot transform an organization back to a user account.

If you're managing packages as a team using a single user account and would like to convert that account into an organization, use the **Transform your account to an organization** option on the **Manage Organizations** page:

![Option on NuGet.org to transform an existing account to an organization](media/org-transform-option.png)

On the next page, specify different user account to assign as the administrator of the organization, then select **Transform**.

![Entering information for transforming a user account to an organization](media/org-transform-page.png)

## Managing organization members

As the organization administrator, you can add members by providing each member's NuGet.org *user account name*; email addresses cannot be used. You then mark each member as a collaborator or administrator with the following permissions:

| Permission | Collaborator | Administrator |
| --- | --- | --- |
| Manage the organization's packages<br/>(submit new packages, update or unlist existing packages) | Yes | Yes |
| Change organization metadata<br/>(email address, notification settings) | No | Yes |
| Manage organization members | No | Yes |
| Request or act on co-ownership requests for organization packages | No | Yes |

## Managing packages

You can view all the packages across your account and all organizations of which you're a member on the [Manage Packages](https://www.nuget.org/account/Packages) page. To view the packages specific to your account or any specific organization, use the accounts filter on the top right of the page.

![Managing packages with the account filter](media/org-manage-packages-option.png)

### Transferring packages to an organization
If you wish to transfer some of your packages to a newly created organization, you can do so by requesting the organization account to co-own the package and then removing yourself as the owner. If you are an administrator of the organization, there is no confirmation required to accept the ownership. However, if you are a collaborator, adding the organization as an owner requires one of the administrators to accept the ownership.

## Publishing packages

You publish packages to an organization like you publish packages to a user account: by directly uploading the package to NuGet.org or by pushing the package through the `nuget push` or `dotnet nuget push` CLI commands.

### Uploading packages

When you directly upload a new package on the [NuGet.org Upload](https://www.nuget.org/packages/manage/upload) page, you assign the package owner to a user or organization account :

![Upload package with account option](media/org-upload-option.png)

### Using API keys

To push a package through the `nuget push` or `dotnet nuget push` CLI commands, you must obtain an API key needed by those commands. For details, see [Publish a package](../quickstart/create-and-publish-a-package-using-visual-studio.md#publish-the-package).

When creating a new API key, select the appropriate organization in the **Package Owner** drop down. Any API key you create is applicable only to the chosen organization:

![API key with account option](media/org-apikey-option.png)

## Removing an organization

As a user, you can remove yourself from an organization by selecting the **X** button shown by your organization membership:

![Removing a user account from an organization](media/org-remove-self-option.png)

Administrators can remove any member from the organization, including other administrators. If you're the sole administrator for an organization, you cannot remove yourself unless you add another member as an administrator.

### Deleting an organization account

You can delete an organization account by clicking the **Delete** button shown in your organization page.

![Deleting an organization](media/org-delete-option.png)

To delete the organization, you must confirm it by clicking the **Delete organization** confirmation button.
