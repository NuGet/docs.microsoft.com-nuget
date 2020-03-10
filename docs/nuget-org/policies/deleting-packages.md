---
title: Deleting NuGet Packages from nuget.org
description: Policies for unlisting packages from nuget.org; permanent deletion is not supported except when packages violate other policies.
author: karann-msft
ms.author: karann
ms.date: 01/18/2018
ms.topic: conceptual
---

# Deleting packages

nuget.org does not support permanent deletion of packages. Doing so would break every project depending on the availability of the package, especially with build workflows that involve package restore.

nuget.org does supports *unlisting* a package, which can be done in the package management page on the web site. Unlisted packages don't appear on nuget.org or in the Visual Studio UI, and do not appear in search results. Unlisted packages, however, can still be downloaded and installed by using an exact version number, which supports package restore. In addition, unlisted packages may still be discovered in the following specific scenarios:

- Package restore using floating versions (for example, `1.0.0-*`), if the latest available package matching the version or dependency constraints is an unlisted package.
- Replication of packages through the catalog (as the catalog also contains unlisted packages).

## Exceptions

In exceptional situations such as copyright infringement and potentially harmful content, packages can be deleted manually by the NuGet team. You can report a package using the "Report abuse" button on the NuGet.org package details page. If you are the package owner, login to your NuGet.org account to reach NuGet support using the "Contact support" button on the NuGet.org package details page.

## Prohibited use

Packages that meet any of the following criteria are not allowed on the public NuGet gallery and will be immediately removed without discussion. Package owners will, however, be notified of the removal.

- Contains malware, adware, or any kind of spyware.
- Are designed to harm a developer's workstation or their organization.
- Infringes copyrights or violates licenses.
- Contains illegal content.
- Are being used to squat on package identifiers, including packages that have zero productive content. Packages must contain code or the owners must concede the identifier to someone who actually has a product to ship.
- Attempt to make the gallery do something that it's not explicitly designed to do.

If you find a package that is in violation of any of these items, click the **Report Abuse** link on the package details page and submit a report.

Note that the NuGet team and the .NET Foundation reserves the right to change these criteria at any time.

#### Unlisting a package
In order to unlist a specific package version please follow these steps:

** Create a API Key **
- Login to your Nuget.org organization account (click the "Sign in" link at the top right corner at nuget.org.
- You will now see your organization name instead of the "Sign in" link at the top right corner. 
- Click the link that displays your organization name and chose "API Keys"
- You will now see "Create" and "Manage" sections. Click the plus sign beside the "Create" section.
- Provide a "Key name" and make sure to check the “Unlist package” checkbox. 
- In the “Available packages” check which of your nuget package(s) you want to unlist. 
- Click the “Create” button. 
- Under the ”Manage” section you will now be able to copy your newly created "API Key". Make sure to save it, since you will not be go back and copy it. 

** Unlist a specific package version **
Now that the API Key that enables unlisting packages has been created, it is time to use it. 
- Type in the url to the nuget package version that you want to delete e.g: "https://www.nuget.org/packages/YOUR-PACKAGE-NAME/3.0.1/"
- Add delete at the end of the url so it becomes: "https://www.nuget.org/packages/YOUR-PACKAGE-NAME/3.0.1/Delete"
- You will now be transferred to the "Manage" page of that specific nuget package version. The "Manage" page will display following sections: "Owners", "Deprecation", "Listing" and "Documentation".
- Click the plus sign beside "Listing" and uncheck the checkbox that says: “List in search results”.
- Click the “Save” button.

** Verification **
The specific package version has now been unlisted. In order to verify this open a incognito instans of your browser and move to the url of the package (without the version part) e.g.: https://www.nuget.org/packages/YOUR-PACKAGE-NAME/. You will see all versions of that package that has * * not * * been unlisted. However if you see the same page while logged in you will see all packages with their status; listed or unlisted. 
