# NuGet Docs

The NuGet documentation contained in this repository is hosted in [NuGet documentation](https://learn.microsoft.com/nuget/). This repository was migrated from the former NuGetDocs repository, https://github.com/NuGet/NuGetDocs, which is no longer in active use.

Contributions to this docset are welcome. Please submit PRs to the *main* branch. The main branch is used for staging changes which is periodically merged into the *live* branch which is what's published to the live Microsoft Learn site.

NuGet follows the [.NET Foundation Contributors Code of Conduct](https://github.com/dotnet/home/blob/master/guidance/be-nice.md). Please take a few minutes to review it.

## Repository structure

- All markdown files are in the `docs` folder and various subfolders.
- The `docs/index.md` file defines the landing (hub) page as it appears in the [NuGet documentation](https://learn.microsoft.com/nuget).
- The `docs/TOC.md` file defines the left-hand navigation panel that appears when you navigate to any page other than the hub page.
- Images are contained within media folders within each subfolder.
- The `docs/docfx.json` file contains various defaults, especially for metadata.
- The `docs/.openpublishing.redirection.json` file contains redirects for old filenames; if you rename a file, create an entry here that maps the old to the new.
- The `docs/_breadcrumb/toc.yml` file defines the breadcrumbs that appear on the site and their target pages. Be mindful of this if you make changes to filenames or placement of articles.

## Contribution workflow

No contribution is too big or too small.

1. Visit the page to edit in [NuGet documentation](https://learn.microsoft.com/nuget/), then click the **Edit** button on the top right. This brings you to the appropriate markdown page in the repo.
1. Edit the markdown:
    1. If you're including images (use PNGs, generally), place them in the media folder that's in the topic's folder. Links are then `media/<image_name>.png`.
    1. Relative links to other pages in this docset should be in the form `../<folder>/<topic-file>.md` including the training `.md`. If you're linking to another topic in the same folder, then `../<folder>/` can be omitted. When using anchors, always remember to include the `.md` before the `#`.
    1. When using external links, especially to Microsoft Learn, omit any language tag like "en-us" so that a reader in another language lands on a target page in that same language if it's available.
1. When you're done, enter a commit message below, and click **Propose file change**.
1. Send a pull request for your change. We review PRs on a regular basis.
1. Thank you!

> **If your content is not live yet, there is a manual `main` -> `live` pull request that is needed to pick-up the changes. Please create a PR or ping a content owner to do so on your behalf.**

If you're creating a new topic, keep the following in mind as well:

1. Always place the new topic in an appropriate subfolder, and follow the conventions for filenames as you see them used here.

1. You must include a metadata block as you see on other topics. Typical defaults (such as for `ms.workload` and `ms.reviewer`) are set within `docs/docjx.json`, so you need only change the following:

   - title: The title that appears in search results. For SEO, this ideally isn't the same as the top-level # (H1) of the article.
   - description: The abstract of the article that appears in search results.
   - author: the author's GitHub ID, to which issues files for this article are assigned.
   - ms.author: if the author is a Microsoft employee, this is the Microsoft alias. Used for reporting and forwarding feedback from other channels.
   - manager: Microsoft alias of the author's manager, if applicable.
   - ms.date: the date of the last revision or review of the article in mm/dd/yyyy format (use leading zeros). This is a communication to the reader about freshness, so it's not updated for minor changes, only for more significant revisions OR when the article has reverified even if there are no changes.
   - ms.topic: used to categorize the article in reports. See table below. Most articles are "conceptual". 

1. In addition to adding your page, edit `docs/TOC.md` to add a link to that page.

1. If you're adding a top-level node to the TOC, also make an entry for it in `docs/index.md`.

| ms.topic category | Description |
| --- | --- |
| conceptual | Use for any content that doesn't fall into another. This is set as the default in docfx.json. |
| overview | Use for any overview or user-guide articles, typically only those that live under an "Overview" node in the TOC. |
| quickstart | Anything under the "Quickstart" node in the TOC that's authored according to Quickstart guidelines. |
| tutorial | Anything under the "Tutorial" node in the TOC that's authored according to Tutorial guidelines. |
| reference | Any reference-type article that isn't auto-generated. |
| article | Use for community-contributed content (that is, anything from outside the engineering team or the content team at Microsoft. |

## Conventions

In general, if you don't see something described here, look in editing markdown files for examples, with the exception of files in the Release Notes which are legacy content and haven't been as thoroughly edited.

## Language level and terms

Because content can be localized into many languages other than English, topics should be written at what's called the "fifth-grade" reading level, or what a typical 11-12-year-old child would understand. In other words, avoid using college-level words if possible.

To keep the tone more casual, use contractions like "you'll" and "don't".

Also, avoid any cultural references or idioms that do not translate easily. 

When describing UI actions, use terms like "select" instead of "click" or "check" because "select" translates better. Use "clear" instead of "uncheck", and prefer "run" over "execute".

Don't worry too much, though. We'll thoroughly review your contributions and edit for these things.

## Naming

Follow these naming conventions and capitalizations when referring to NuGet and related components.

- NuGet: refers to the technology.
- NuGet Package Manager UI, NuGet Package Manager Console, etc.: refers to other components build on NuGet.
- nuget.exe: refers to the command-line executable; you can use "nuget" by itself when followed by other arguments. When referring to the command by itself, as in "when you run nuget", include the .exe as in "when you run nuget.exe" so your meaning is clear.
- packages.config (or the deprecated project.json): refer to NuGet files in a project.
- NuGet.Config and NuGetDefaults.Config: these files appear with this capitalization so be sure to follow them.
- .nuspec: refers to a NuGet specification for creating a package; generally, we speak of a .nuspec file with the period, because it's always used as a file extension.

For items not listed here, search in the repository for the term and see how it's used.

### Heading capitalizations

All headings need only capitalize the first word and proper names. Refer to most topics for examples.

### UI terms, boldface, and italics

Use **boldface** for any string that a reader will see in UI, such as "Select the **File** menu," except when referring to common window titles like Solution Explorer.

Use ">" to indicate UI hierarchies, as in the **File > New Project...** menu, or **Tools > Options > NuGet > NuGet Package Manager > Package Sources**. Notice how in this example you can use ">" to traverse a hierarchy from a menu, into a dialog box, into tabs, sub-tabs, and fields. In other words, readers can work with the chain of items to follow and don't need every click spelled out for them. In general, think of how to get the reader to where they need to go in the most efficient way.

With boldface used for UI elements, use *italics* for emphasis in the text.

### Tables

Use standard markdown tables, starting with "| heading | heading | heading |", followed by "| --- | --- | --- |", followed by your rows. The row with "---" is necessary for Microsoft Learn to read the markdown as a table.

Items in the first column are bolded by default, so you don't need to do that explicitly.

### Screenshots and images

Make all images purposeful and easy to consume; avoid graphics for the sake of graphics. When using a screenshot, include a red rounded-rectangle outline of where the reader's eyes should go. That is, do the work to help the reader look at what you want them to look at, rather than burdening them with having to figure that out for themselves.

If an image has white bleed areas on the edges, draw a 1-pixel gray outline around the entire graphic.

Always include a meaningful description of the image in the markdown alt-text between the []'s. These descriptions are also a great place to put SEO terms that you don't otherwise want to appear in the text.

### Inline code

Delineate inline code with grave accents (backticks), as in \`nuget pack\`. This inline formatting is used for the following:

- Code
- Identifiers
- File names, folder names, and extensions
- Command line strings and arguments

Except for the following cases:

- When part of a surrounding code block.
- When appearing in the left-hand column of a table, because these are automatically bolded.
- When appearing in quoted strings
- When appearing in links (although when the term appears by itself, it's OK)

Markdown and HTML are ignored within inline code.

### Code blocks

Code blocks on Microsoft Learn are delineated by with three grave accents (backticks), ```, at the beginning and the end. You do not need to indent code blocks unless they are contained within a list.

The opening ``` should be followed by a language code for proper syntax coloring, such as "xml", "json", "csharp", etc. Use "cli" for command-line examples and "output" for command-line results.

The only case when you should use ``` without a language tag is when creating a block of fixed-point text that isn't related to any kind of code. In these cases you can also just indent the code block, which can be preferable because it visually separates the code in an editor.

### Callouts

Microsoft Learn uses blockquotes for callouts, that is, lines starting with ">".

Callout sections with ">" only will appear with a solid gray line to the left. See [Creating NuGet packages](https://learn.microsoft.com/nuget/create-packages/creating-a-package) for examples.

You can also use one of the following callout tags on the first line that will create a shaded callout in the indicated color:

| Tag | Callout use | Topic with examples |
| --- | --- | --- |
| `> [!Note]` | Callouts without any special emphasis. | [Creating NuGet packages](https://learn.microsoft.com/nuget/create-packages/creating-a-package) |
| `> [!Tip]` | Callouts that share special tips and tricks or other helpful knowledge. | [Package consumption overview](https://learn.microsoft.com/nuget/consume-packages/overview-and-workflow) | 
| `> [!Important]` | Callouts that describe cautions. | [NuGet.Server](https://learn.microsoft.com/nuget/hosting-packages/nuget-server) |
| `> [!Warning]` | Callouts that warn readers about situations that could cause data loss or unexpected consequences. | [Dependency resolution](https://learn.microsoft.com/nuget/consume-packages/dependency-resolution) |

### Links

- In general, always use the title of the target page as the link text rather than words like "see here" or "this documentation".
- Relative links to other pages in this docset should be in the form `../<folder>/<topic-file>.md` including the trailing `.md`.
- Links to other markdown files on Microsoft Learn are case-insensitive (unlike links to files in GitHub, which are).
- If you're linking to another topic in the same folder, then `../<folder>/` can be omitted.
- When using anchors, always remember to include the `.md` before the `#`.
- When using external links, especially to Microsoft Learn, omit any language tag like "en-us" so that a reader in another language lands on a target page in that same language if it's available.
- Bare URLs are not automatically converted into links.

### Inline HTML

If you need to do something that Markdown can't handle, use inline HTML. An example is creating a bullet list inside a table.

Use `&lt;` and `&gt;` for < and > characters outside a code block or inline code (delimited by backticks `).

Block-level HTML elements have a few restrictions:

- They must be separated from surrounding text by blank lines.
- The begin and end tags of the outermost block element must not be indented.
- Markdown can't be used within HTML blocks.
