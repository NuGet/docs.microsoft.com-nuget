When your package successfully publishes, you receive a confirmation email. On nuget.org, select your user name at upper right, and then select **Manage Packages** to see the package you just published.

> [!NOTE]
> It might take awhile for your package to be indexed and appear in search results where others can find it. During that time your package page shows the following message:
> 
> ![This package has not been indexed yet. It will appear in search results and will be available for install/restore after indexing is complete.](../media/QS_Create-03-NotIndexed.png)

You've just published a NuGet package to nuget.org so other developers can use it in their own projects. This quickstart created a package with an empty class library that isn't actually useful. To *unlist* the package and hide it from search results:

1. From **Manage Packages**, under **Published Packages**, select pencil icon next to the package you want to unlist.

   ![Screenshot that shows the Edit icon for a package listing on nuget.org.](../media/qs_create-vs-03-trash-can.png)

1. On the next page, under **Listing**, deselect the **List in search results** checkbox, and then select **Save**.

   ![Screenshot that shows clearing the List checkbox for a package on nuget.org.](../media/qs_create-vs-04-unlist.png)