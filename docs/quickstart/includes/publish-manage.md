From your profile on nuget.org, select **Manage Packages** to see the one you just published. You also receive a confirmation email. Note that it might take a while for your package to be indexed and appear in search results where others can find it. During that time your package page shows the message below:

![This package has not been indexed yet. It will appear in search results and will be available for install/restore after indexing is complete.](../media/QS_Create-03-NotIndexed.png)

And that's it! You've just published your first NuGet package to nuget.org that other developers can use in their own projects.

If in this walkthrough you created a package that isn't actually useful (such as a package created with an empty class library), you should *unlist* the package to hide it from search results:

1. On nuget.org, select your user name (upper right of the page), then select **Manage Packages**.

1. Locate the package you want to unlist under **Published** and select the trash can icon on the right:

    ![Trash can icon shown for a package listing on nuget.org](../media/qs_create-vs-03-trash-can.png)

1. On the subsequent page, clear the box labeled **List (package-name) in search results** and select **Save**:

    ![Clearing the List checkbox for a package on nuget.org](../media/qs_create-vs-04-unlist.png)