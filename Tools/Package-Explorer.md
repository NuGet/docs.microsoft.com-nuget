# NuGet Package Explorer

The NuGet Package Explorer click-once application makes creating packages easy. It's also a great way 
to examine packages and learn how packages are structured.

<div class="block-callout-info">
    <strong>Note:</strong><br>
    This is an open source community project. While we love it, it is not officially supported by the NuGet Team.
</div>

If you are integrating building packages into a build system, then [using NuGet.exe to create and publish packages](../create-packages/create-a-package) is a better choice.

## Installation

This is a open source project hosted in [npe.codeplex.com](https://npe.codeplex.com) where you can find a    
[ClickOnce installer](https://npe.codeplex.com/releases/view/624769)

## Creating a Package

**Step 1**

To create a package, launch Package Explorer and select **File** > **New** menu option s(or hit CTRL + N). Alternatively, when you start the tool, you can create a new package from there as well.

![Creating a new package](/images/docs/PackageExplorerStart.png)

**Step 2**

Then select the **Edit** > **Edit Package Metadata** menu option (or CTRL + K) to edit the package metadata.

![Editing Metadata with the Package Explorer](/images/docs/PackageExplorerEditMetadata.png)

The metadata editor provides a GUI editor for editing the underlying nuspec file. For more details about these fields, read the [NuSpec reference]().

**Step 3**

The final step is to drag in the contents of your package into the **Package contents** pane. Package Explorer will attempt to infer where the content belongs and prompt you to place it in the correct directory within the package.

For example, if you drag an assembly into the Package contents window, it will prompt you to place the assembly in the **lib** folder.

![Package Explorer Infers content location](/images/docs/PackageExplorerAddFiles.png)

And clicking **OK** places the file in the appropriate folder.

![Package Explorer Lib Folder](/images/docs/PackageExplorerFileAdded.png)

You can also explicitly add the special folders via the **Content** menu.


**Step 4**

Don't forget to save your package via the **File** > **Save** menu option (or CTRL + S).

## Publishing a Package

Once you've created and saved your package, navigate to the **File** > **Publish** menu option (CTRL + P) to publish your package.

This brings up the **Publish Package** dialog.

![Publish Package Dialog](/images/docs/PackageExplorerPublish.png)

Enter your API Key and click **Publish** to publish your package to the NuGet package feed. If you don't have an account yet, visit [the NuGet Gallery](http://nuget.org/) and register for an account.

After you register and login, click on the **[My Account](http://nuget.org/Contribute/MyAccount)** link to see your API access key.
