# Deleting packages

NuGet.org does not support permanent deletion of packages. Doing so would break every project depending on the availabiliyy of the package, especially with build workflows that involve package restore.

NuGet.org does supports *unlisting* a package, which can be done in the package management page on the web site. Unlisted packages no longer appear on nuget.org or in the Visual Studio UI, and do not appear in search results. Unlisted packages, however, can still be downloaded and installed by using an exact version number, which supports package restore.

## Exceptions

In exceptional situations such as copyright infringement and potentially harmful content, packages can be deleted manually by the NuGet team. Submit a support request through [NuGet Gallery](http://www.nuget.org) to start the process.

## Prohibited use

Packages that meet any of the following criteria are not allowed on the public NuGet gallery and will be immediately removed without discussion. Package owners will, however, be notified of the removal.

- Contains malware, adware, or any kind of spyware.
- Are designed to harm a developer's workstation or their organization.
- Infringes copyrights or violates licenses.
- Contains illegal content.
- Are being used to squat on package identifiers, including packages that have zero productive content. Packages must contain code or the owners must concede the identifier to someone who actually has a product to ship.
- Attempt to make the gallery do something that it is not explicitly designed to do.

If you find a package that is in violation of any of these items, click the **Report Abuse** link on the package details page and submit a report.

Note that the NuGet team and the .NET Foundation reserves the right to change these criteria at any time.