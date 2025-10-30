---
title: User Data Requests
description: Policies for requesting user data export and delete
author: JonDouglas
ms.author: jodou
ms.date: 05/01/2018
ms.topic: article
---

# User Data Requests

nuget.org users can submit information delete requests and information export requests through [nuget.org](https://www.nuget.org). Both types are submitted in the form of support requests and are be executed by the nuget.org administrators within 30 days.

The following user data is directly accessible through nuget.org:

* Account related data such as email address, login account, profile picture, and email notification settings
* Owned API Keys
* List of owned packages

This data is not included in data exported through the support request.

## Identifying customer data

Customer data can be identified as nuget.org user account names.

## Deleting customer data

To request deleting user data from nuget.org:

1. The user must sign-in to [nuget.org](https://www.nuget.org)
1. The user must submit a request for their account to be deleted [nuget.org/account/delete](https://www.nuget.org/account/delete)

Users that are sole owners of packages are encouraged to find new owners before asking to have their account deleted. If package ownership is not transferred, the NuGet package is unlisted and, as a result, it is no longer available in search queries in Visual Studio or on the nuget.org website. Before deleting the account, the nuget.org administrators work with the user to find new owners for the packages they own.

The account delete action is completed by the nuget.org administrator within 30 days from the date of the request.

Upon account deletion, all of the user's data is be removed from the nuget.org system and the following actions are taken:

* The deleted account becomes unregistered with nuget.org
* All owned API Keys are deleted
* All reserved namespaces are released
* Any package ownership are removed

The owned packages are *not* deleted. Though unlisted from search results, they remain available through package restore to projects that depend on them.

## Exporting customer data

After sign-in to nuget.org, a user can submit an export request through [nuget.org/policies/Contact](https://www.nuget.org/policies/Contact)

The data exported is made available for 48 hours to the user for download through an Azure Blob. After 48 hours, access expires and the user must submit a new export request as needed.

The exported data includes:

* The user's support requests
* The user's actions (publish package, create account) as persisted in the audit logs
* Any user information as persisted in the IIS logs
