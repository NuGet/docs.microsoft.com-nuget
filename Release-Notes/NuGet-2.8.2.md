# NuGet 2.8.2 Release Notes

[NuGet 2.8.1 Release Notes](nuget-2.8.1) | [NuGet 2.8.3 Release Notes](nuget-2.8.3)

NuGet 2.8.2 was released on May 22, 2014.  This release only included changes to the NuGet.exe command-line, the NuGet.Server package and other NuGet packages.  The release did not include an updated Visual Studio extension or WebMatrix extension.

## Notable Updates

The most notable updates were in the NuGet.exe command-line and the NuGet.Server package (for self-hosted NuGet feeds).

### Important NuGet.exe Bug Fixes

1. [NuGet.exe Push fails and keeps retrying](https://nuget.codeplex.com/workitem/4000)
1. [NuGet.exe Push does not send Basic Auth credentials correctly](https://nuget.codeplex.com/workitem/4109)
1. [NuGet.exe Push won't follow temporary redirect](https://nuget.codeplex.com/workitem/4050)

### Important NuGet.Server Bug Fix

1. [Wrong value of IsAbsoluteLatestVersion returned by NuGet.Server](https://nuget.codeplex.com/workitem/4147)

## Packages Updated

The NuGet.exe command-line and NuGet.Server fixes are shipped as NuGet package updates.  There were other packages updated with 2.8.2 as well.

Here's the list of updated packages:

1. [NuGet.Core](https://www.nuget.org/packages/NuGet.Core/)
1. [NuGet.CommandLine](https://www.nuget.org/packages/NuGet.CommandLine/)
1. [NuGet.Server](https://www.nuget.org/packages/NuGet.Server/)
1. [NuGet.Build](https://www.nuget.org/packages/NuGet.Build/)
1. [NuGet.VisualStudio](https://www.nuget.org/packages/NuGet.VisualStudio/) (the package, not the extension)

## All Changes
There were 10 issues addressed in the release. For a full list of the work items fixed in NuGet 2.8.2, please view the [NuGet Issue Tracker for this release](https://nuget.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=NuGet%202.8.2&assignedTo=All&component=All&sortField=LastUpdatedDate&sortDirection=Descending&page=0&reasonClosed=All).
