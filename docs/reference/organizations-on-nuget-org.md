---
title: Organizations on nuget.org | Microsoft Docs
author: anangaur
ms.author: anangaur
manager: unnir
ms.date: 04/10/2018
ms.topic: article
ms.prod: nuget
ms.technology: null
description: Organizations on nuget.org helps you to manage packages published by group or in a team, company environment.
ms.reviewer:
- karann-msft
- kraigb
- camsoper
---

# Organization on nuget.org

Organizations enables businesses and open-source projects to collaborate across many packages at once using a single nuget.org identity. For a package consumer, an organization account appears same as an existing user account on nuget.org.

## User accounts vs. organization accounts

Your user account is your identity on nuget.org and can be a member of any number of organizations. A package can belong to an organization account like it can belong to a user account. Package consumers don't see any difference between an user account or the organization account: both appear as package `owners`.

An organization account has one or more user accounts as its members. These members can manage a set of packages while maintaining a single identity for ownership.

## Adding a new organization

To add a new organization, select your account on nuget.org, then select the **Manage Organizations...** menu command:

![Menu option on nuget.org for Manager Organizations](media/org-manage-option.png)

On the next page, select the **Add new organization** button:

![Button to create a new organization on nuget.org](media/org-add-new-option.png)

On the next page, provide the organization name and email address. Because nuget.org manages organization and user names together, the organization name must be different from any other existing organization or user accounts. The email address must also be unique across all accounts.

![Add new organization page on nuget.org](media/org-add-new-page.png)

Once the organization account is created, you are the administrator and can submit packages for the organization and add organization members.

### Transform existing account to an organization

If you're managing packages as a team using a single user account and would like to convert that account into an organization, use the **Transform your account to an organization** option on the **Manage Organizations** page:

![Option on nuget.org to transform an existing account to an organization](media/org-transform-option.png)

On the next page, specify different user account to assign as the administrator of the organization, then select **Transform**.

![Entering information for transforming a user account to an organization](media/org-transform-page.png)

> [!Warning]
> Account conversion is irreversible: you cannot transform an organization back to a user account.

## Managing organization members

As the organization administrator, you can add members by providing each member's nuget.org *user account name*; email addresses cannot be used. You then mark each member as a collaborator or administrator with the following permissions:

| Permission | Collaborator | Administrator |
| --- | --- | --- |
| Manage the organization's packages<br/>(submit new packages, update or unlist existing packages) | Yes | Yes |
| Change organization metadata<br/>(email address, notification settings) | No | Yes |
| Manage organization members | No | Yes |
| Request or act on co-ownership requests for organization packages | No | Yes |

## Managing packages

As an organization member, you can see all the organization's packages in the **Packages** section of the organization's details page. On the **Manage Packages** page you can also view packages across your account and all organizations of which you're a member. You can select the account filter to view packages by your account or any specific organization.

![Managing packages with the account filter](media/org-manage-packages-option.png)

## Publishing packages

You publish packages to an organization like you publish packages to a user account, by directly uploading the package to nuget.org or by pushing the package through the `nuget push` or `dotnet nuget push` CLI commands.

### Uploading packages

When you directly upload a new package on the [nuget.org Upload](https://www.nuget.org/packages/manage/upload) page, you assign the package owner to a user or organization account :

![Upload package with account option](media/org-upload-option.png)

When submitting updates to an existing package, you don't see the account dropdown because package ownership is already known to nuget.org.

### Using API keys

To push a package through the `nuget push` or `dotnet nuget push` CLI commands, you must obtain the API key needed by those commands. For details, see [Publish a package](https://docs.microsoft.com/en-us/nuget/quickstart/create-and-publish-a-package-using-visual-studio#publish-the-package).

When creating a new API key, select the appropriate organization in the **Package Owner** drop down. Any API key you create is applicable only to the chosen organization:

![API key with account option](media/org-apikey-option.png)

## Removing an organization

As a user, you can remove yourself from an organization by selecting the `X` button shown by your organization membership:

![Removing a user account from an organization](media/org-remove-self-option.png)

Administrators can remove any member from the organization, including other administrators. If you're the sole administrator for an organization, you cannot remove yourself unless you first add another member as an administrator.

You can delete an organization if the following criteria are met:

- You're the organization administrator.
- You're the sole member.
- The organization doesn't own any packages.
