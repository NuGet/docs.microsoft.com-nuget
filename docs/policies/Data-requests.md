---
title: User Data Requests
description: Policies for requesting user data export and delete
author: karann-msft
ms.author: karann
manager: douge
ms.date: 05/01/2018
ms.topic: conceptual
---

# User Data Requests

NuGet.org users can submit information delete and export requests through https://www.nuget.org. Both types of request (delete and export) will be submitted in the form of support requests and will be executed by the NuGet.org administrators during an interval of a maximum 30 days.

The following user data is directly accessible through the NuGet.org site:

* Account related data such as email address, login account, profile picture, email notification settings
* Owned API Keys
* List of owned packages

This data will not be included in the data exported through the support request.

## Identifying customer data
Customer data can be identified as NuGet.org user account names.

## Deleting customer data
To request deleting user data from NuGet.org, there are 2 steps that need to be taken:

1. The user must sign-in to NuGet.org (https://www.nuget.org)
1. The user must submit a request for their account to be deleted (https://www.nuget.org/account/delete)

Users that are sole owners of packages are encouraged to find new owners before asking to have their account deleted. If package ownership is not transferred, the NuGet package will be unlisted and as a result, it will stop being available in search queries in Visual Studio or in the NuGet.org website. Before deleting the account, the NuGet.org administrators will work with the user to find new owners for the packages they own.

The account delete action will be completed by the NuGet.org administrator in a maximum of 30 days from when the request was submitted.

Upon account deletion, all of the users data will be removed from the NuGet.org system and the following actions will be taken:

* The deleted account will become unregistered with https://www.nuget.org
* All owned API Keys will be deleted
* All reserved namespaces will be released
* Any package ownership will be removed
* The owned packages will not be deleted

## Exporting customer data
After sign-in to https://www.nuget.org, the user can perform an export request by accessing https://www.nuget.org/policies/Contact and submitting an export support request.

The data exported will be made available to the user for download through an Azure Blob in an interval of 48 hours. After 48 hours the access will expire. The user will need to submit a new export request as needed.

The exported data will include:

* The user's support requests
* The user's actions (e.g. publish package, create account) as persisted in the audit logs
* Any user's information as persisted in the IIS logs
