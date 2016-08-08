# Managing NuGet Packages Using the UI

This topic describes how to find, install, remove, and update NuGet packages using the **Manage NuGet Packages UI**. To use this UI, you must have a solution open in Visual Studio. As an alternative, you can install packages using PowerShell commands. For more information, see [Using the Package Manager Console](Package-Manager-Console).

<div class="block-callout-info">
    <strong>Note:</strong><br>
    This topic has been updated to reflect the latest UI in Visual Studio 2015.
</div>

<div class="block-callout-info">
    <strong>Note:</strong><br>
    For managing packages within multiple projects in the solution at the same time, skip to the section <a href="#managing-packages-for-the-solution">Managing Packages for the Solution</a>
</div>

## Finding a Package

In **Solution Explorer**, right-click the **References** node and click **Manage NuGet Packages...**. 

<div class="block-callout-info">
    <strong>Note:</strong><br>
    In a Website project, right click on the **Bin** node
</div>

![Manage NuGet Packages menu option](/images/docs/ManagePackagesUICommand.png)

Browse through the list, or search for a package using the search box at the top right of the dialog box. 
For example, to find the logging package named Json.NET, enter "json". When you select a package, an **Install** button appears, and a description is shown in the right-hand pane.

![Manage NuGet Packages Dialog Online tab](/images/docs/Search.png)

## Installing a Package

To install a package, select it and then click **Install**. NuGet installs the selected package and any other packages it is dependent on. 

You might be asked to accept license terms.

![License Acceptance dialog box](/images/docs/License.png)

When the installation completes, the package shows up in the **Installed** tab.

In **Solution Explorer**, you can see references that Visual Studio has added for the installed library or libraries. You can now use the library in your project. 

![Reference in Solution Explorer](/images/docs/References.png)

## Removing a Package

Open the **Manage NuGet Packages** dialog and make sure the **Installed Packages** tab is selected to display 
the list of installed packages.

Select the package you want to uninstall and then click **Uninstall** to remove the package.

## Updating a Package

Open the **Manage NuGet Packages** dialog and make sure the **Updates** tab is selected to display 
the list of packages that have newer versions available.

If any packages have updates available, they will be listed in the center pane. The following screenshot 
shows a newer version of jQuery available.

Select the package you want to update and click **Update** to update the package to the latest version. 

## Managing Packages for the Solution

<div class="block-callout-info">
    <strong>Support</strong><br>
    Available in NuGet 1.4+.
</div>

In the previous section, we looked at managing packages for a single project. In NuGet 1.4 and above, the 
**Manage NuGet Packages** UI can also be launched at the solution level in order to install/uninstall 
packages in multiple projects at the same time.

Just right click on the Solution and select **Manage NuGet Packages**. You can also launch this dialog from 

![Manage NuGet Packages Solution level menu](/images/docs/ManagePackagesSolutionUICommand.png)

The key difference is that each operation lets you select which projects it applies to.

![Manage NuGet Packages Solution level menu](/images/docs/SolutionPackagesUI.png)

## Package Sources
NuGet can display packages from multiple package sources. To add a package source, click on the **Settings** 
button in the UI or acess it from Tools->Options->NuGet Package Manager to launch the Options dialog. Make sure the **Package Sources** node is selected in the dialog.

![Package Sources Dialog](/images/docs/options.png)

Type in the name of a source as well as its URL or folder path (a folder containing NuGet package 
files is a valid package source) and then click the **Add** button.

The package source will be listed under the **All** node. Click on the package source to view 
packages from that source. The **All** node displays an aggregated view of packages from all 
package sources.

If you want to temporarily disable a package source, just uncheck the package source in the 
dialog. This is useful if a package source is temporarily down for some reason and you need to 
keep it from being included in the aggregate feed.


## Package Manager User Interface Options

This topic describes the options available to install and uninstall NuGet packages using the **Manage NuGet Packages** 
dialog box. To begin using this dialog box, you must have a solution open in Visual Studio.
For more information, see [Managing NuGet Packages Using the Dialog](Package-Manager-Dialog).

### Installation Options
<a name="install-options"></a>

Whether you are attempting to install, update, or downgrade a package you will have the same collection of options available to you:

![Installation Options](/images/consume/installation-options.png)

There are currently three options available:

1. **Show Preview Window** - if enabled, a modal window will be displayed that details which dependent packages will be loaded with the package you have chosen.

	![Example Preview Dialog](/images/consume/install-preview-dialog.png)

2. **Dependency Behavior** - this allows you to configure how NuGet will decide which versions of dependent packages will be installed.  There are five options:

	a. **Ignore** - This is usually a bad idea, as a package has dictated that it dependends on other packages and will require their contents to operate.  You may choose to skip installing those packages.

    b. **Lowest** - Install the package version with the minimal version number that meets the requirements of my chosen package.

    c. **Highest Patch** - Install the version with the same first and second version numbers (typically called major and minor version numbers), but the highest patch number.  For example, if version 1.2.2 is specified then the highest version that starts with 1.2 will be installed

    d. **Highest Minor** - Install the version with the same first (or major) version number, but the highest minor number and patch number.  If version 1.2.2 is specified, then the highest version that starts with 1 will be installed

    e. **Highest** - Install the highest version of the package available

3. **File Conflict Options** - If the package or any of its dependent packages being installed match a file already on disk, how should NuGet handle it?

    a. **Prompt** - NuGet will ask if you would like to keep or override the files
    
  	b. **Ignore All** - NuGet will skip writing any files into your project with matching file names

	c. **Overwrite All** - NuGet will overwrite any matching files in your project with those from the package 

### Uninstall Options
<a name="uninstall-options"></a>

The following options are available when you are attempting to uninstall and remove a package from your project:

![Uninstallation Options](/images/consume/uninstall-options.png)

These three options are available:

1. **Show Preview Window** - if enabled, a modal window will be displayed that details what packages and dependent packages will be removed from your project.
2. **Remove Dependencies** - Remove any dependent packages if they are not referenced by any other package in the project
3. **Force Uninstall** - Typically used in combination with **Remove Dependencies** to remove a package and its dependencies whether there are other packages that depend on the current package or its dependencies.  This may lead to a breaking reference scenario in your project.
