# NuGet 1.7 Release Notes

[NuGet 1.6 Release Notes](nuget-1.6) | [NuGet 1.8 Release Notes](nuget-1.8)

NuGet 1.7 was released on April 4, 2012.

## Known Installation Issue
If you are running VS 2010 SP1, you might run into an installation error when attempting to upgrade 
NuGet if you have an older version installed.

The workaround is to simply uninstall NuGet and then install it from the VS Extension Gallery.  See
<a href="http://support.microsoft.com/kb/2581019">http://support.microsoft.com/kb/2581019</a> for more information.

Note: If Visual Studio won't allow you to uninstall the extension (the Uninstall button is disabled),
then you likely need to restart Visual Studio using "Run as Administrator."

## Features

### Support opening readme.txt file after installation
New in 1.7, if your package includes a <b>readme.txt</b> file at the root of the package, NuGet will 
automatically open this file after it's finished installing your package.

### Show prerelease packages in the Manage NuGet packages dialog
The Manage NuGet Packages dialog now includes a dropdown which provides option to show prerelease 
packages.

![Showing prerelease packages](Images/prerelease-dropdown.PNG)

### Show Package Restore button when package files are missing
When you open the Package Manager console or the Manager NuGet packages dialog, NuGet will check 
if the current solution has enabled the Package Restore mode and if any package files are missing 
from the <b>packages</b> folder. If these two conditions are met, NuGet will notify you and will show a 
convenient Restore button. Clicking this button will trigger NuGet to restore all the missing 
packages.

![Package restore button on dialog](Images/packagerestore-dialog.PNG)

![Package restore button on console](Images/packagerestore-console.PNG)

### Add solution-level packages.config file
In previous versions of NuGet, each project has a <b>packages.config</b> file which keeps track of what 
NuGet packages are installed in that project. However, there was no similar file at the solution 
level to keep track of solution-level packages. As a result, there was no way to restore 
solution-level packages.
This feature is now implemented in NuGet 1.7. The solution-level <b>packages.config</b> file is placed 
under the <b>.nuget</b> folder under solution root and will store only solution-level packages.

### Remove New-Package command
Due to low usage, the New-Package command has been removed. Developers are recommended to use 
nuget.exe or the handy NuGet Package Explorer to create packages.

## Bug Fixes
NuGet 1.7 has fixed many bugs around the Package Restore workflow and Network/Source Control 
scenarios.

For a full list of work items fixed in NuGet 1.7, please view the [NuGet Issue Tracker for this release](http://nuget.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=NuGet%201.7&assignedTo=All&component=All&sortField=Votes&sortDirection=Descending&page=0).
