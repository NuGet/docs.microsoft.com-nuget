No contribution is too big or too small.

1. Visit the page to edit in the [NuGet documentation](https://learn.microsoft.com/nuget/), then click the **Edit** button on the top right. This brings you to the appropriate markdown page in the repo.
1. Edit the markdown:
    1. If you're including images (use PNGs, generally), place them in the media folder that's in the topic's folder. Links are then `media/<image_name>.png`.
    1. Relative links to other pages in this docset should be in the form `../<folder>/<topic-file>.md` including the training `.md`. If you're linking to another topic in the same folder, then `../<folder>/` can be omitted. When using anchors, always remember to include the `.md` before the `#`.
    1. When using external links, especially to Microsoft Docs (or msdn.microsoft.com for any older content), omit any language tag like "en-us" so that a reader in another language lands on a target page in that same language if it's available.
1. When you're done, enter a commit message below, and click **Propose file change**.
1. Send a pull request for your change. We review PRs on a regular basis.
1. Thank you!

If you're creating a new topic, keep the following in mind as well:

1. Always place the new topic in an appropriate subfolder, and follow the conventions for filenames as you see them used here.
1. You must include a metadata block as you see on other topics. Typical defaults (such as for ms.workload and ms.reviewer) are set within docs/docjx.json, so you need only change the following:

  - title: The title that appears in search results. For SEO, this ideally isn't the same as the top-level # (H1) of the article.
  - description: The abstract of the article that appears in search results.
  - author: the author's GitHub ID, to which issues files for this article are assigned.
  - ms.author: if the author is a Microsoft employee, this is the Microsoft alias. Used for reporting and forwarding feedback from other channels.
  - manager: Microsoft alias of the author's manager, if applicable.
  - ms.date: the date of the last revision or review of the article in mm/dd/yyyy format (use leading zeros). This is a communication to the reader about freshness, so it's not updated for minor changes, only for more significant revisions OR when the article has reverified even if there are no changes.
  - ms.topic: used to categorize the article in reports. See table below. Most articles are "conceptual". 
1. In addition to adding your page, edit docs/TOC.md to add a link to that page.
1. If you're adding a top-level node to the TOC, also make an entry for it in docs/index.md.

| ms.topic category | Description |
| --- | --- |
| conceptual | Use for any content that doesn't fall into another. This is set as the default in docfx.json. |
| overview | Use for any overview or user-guide articles, typically only those that live under an "Overview" node in the TOC. |
| quickstart | Anything under the "Quickstart" node in the TOC that's authored according to Quickstart guidelines. |
| tutorial | Anything under the "Tutorial" node in the TOC that's authored according to Tutorial guidelines. |
| reference | Any reference-type article that isn't auto-generated. |
| article | Use for community-contributed content (that is, anything from outside the engineering team or the docs team at Microsoft. |
