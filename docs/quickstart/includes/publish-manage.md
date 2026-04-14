When your package is successfully published, you receive a confirmation email. To see the published package, go to [nuget.org](https://www.nuget.org/), select your user name in the upper-right corner, and then select **Manage Packages**.

> [!NOTE]
> It might take a while for your package to be indexed and to appear in search results where others can find it. During that time, your package appears under **Unlisted Packages**, and the package page shows the following message:
>
> :::image type="content" source="../media/qs-create-not-indexed.png" alt-text="Screenshot of a nuget.org warning message about the package not being published yet. The text states that validation and indexing can take an hour.":::

Now that your NuGet package is published at nuget.org, other developers can use it in their projects.

If you create a package that isn't useful (such as this sample package from an empty class library), or if you don't want the package to be visible, you can *unlist* the package to hide it from search results:

1. After the package appears under **Published Packages** on the **Manage Packages** page, select the pencil icon next to the package listing.

   :::image type="content" source="../media/qs-create-vs-edit-package.png" alt-text="Screenshot of the nuget.org Packages page. The Published Packages section lists one package. Its edit icon is highlighted.":::

1. On the next page, select **Listing**, clear the **List in search results** checkbox, and then select **Save**.

   :::image type="content" source="../media/qs-create-vs-unlist-package.png" alt-text="Screenshot of a nuget.org page. In the Listing section, the option for listing the package in search results is highlighted.":::

The package now appears under **Unlisted Packages** in **Manage Packages** and no longer appears in search results.

> [!NOTE]
> To avoid your test package being live on nuget.org, you can push to the nuget.org test site at [https://int.nugettest.org](https://int.nugettest.org). Note that packages uploaded to int.nugettest.org might not be preserved.
