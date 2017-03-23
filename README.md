# docs.microsoft.com-nuget

The NuGet documentation contained in this repository is hosted on docs.microsoft.com/nuget. This repository was migrated from the former NuGetDocs repository, https://github.com/NuGet/NuGetDocs, which is no longer in active use.

Contributions to this docset are welcome. Please submit PRs to the *live* branch, which is what's published to the live docs site. (The master branch is used for staging larger series of changes.)

## Respository structure

- All markdown files are in the docs folder and various subfolders.
- The docs/index.md file defines the landing (hub) page are it appears on docs.microsoft.com/nuget.
- The docs/TOC.md file defines the left-hand navigation panel that appears when you navigate to any page other than the hub page.
- Images are contained within media folders within each subfolder.

## Contribution workflow

No contribution is too big or too small--

1. Visit the page to edit on [docs.microsoft.com/nuget](docs.microsoft.com/nuget), then click the **Edit** button on the top right. This brings you to the appropriate markdown page in the repo.    
1. Edit the markdown:
    1. If you're including images (use PNGs, generally), place them in the media folder that's in the topic's folder. Links are then `media/<image_name>.png`.
    1. Relative links to other pages in this docset should be in the form `../<folder>/<topic-file>.md` including the training `.md`. If you're linking to another topic in the same folder, then `../<folder>/` can be omitted. When using anchors, always remember to include the `.md` before the `#`.
    1. When using external links, especially to docs.microsoft.com or msdn.microsoft.com, omit any language tag like "en-us" so that a reader in another language lands on a target page in that same language if it's available.
1. When you're done, enter a commit message below, and click **Propose file change**.
1. Send a pull request for your change. We review PRs on a regular basis.'
1. Thank you!

If you're creating a new topic, keep the following in mind as well:

1. Always place the new topic in an appropriate subfolder, and follow the conventions for filenames as you see them used here.
1. You must include a metadata block as you'll see on other topics. The fields you should change are the following: title, ms.date, ms.assetid, description, and keywords.
    1. For ms.assetid, always create a new GUID for a new topic.
    1. Leave the ms.author and manager fields set as they are, because these are used to manage ownership internally.
    1. Ideally for SEO, the title field and the top-level # heading of the topic are different; see various topics for examples.
1. In addition to adding your page, edit docs/TOC.md to add a link to that page.
1. If you're adding a top-level node to the TOC, also make an entry for it in docs/index.md.

## Conventions

In general, if you don't see something described here, look in editing markdown files for examples, with the exception of files in the Release Notes which are legacy content and haven't been as thoroughly edited.

## Language level and terms

Because our docs can be localized into many languages other than English, topics should be written at what's called the "fifth-grade" reading level, or what a typical 11-12 year old child would understand. In other words, avoid using college-level words if possible.

To keep the tone more casual, use contractions like "you'll" and "don't".

Also avoid any cultural references or idioms that do not translate easily. 

When describing UI actions, use terms like "select" instead of "click" or "check" because "select" translates better. Use "clear" instead of "uncheck", and prefer "run" over "execute".

Don't worry too much, though. We'll thoroughly review your contributions and edit for these things.

## Naming

Follow these naming conventions and capitalizations when referring to NuGet and related components.

- NuGet: refers to the technology.
- NuGet Package Manager UI, NuGet Package Manager Console, etc.: refers to other components build on NuGet.
- nuget.exe: refers to the command-line executable; you can use "nuget" by itself when followed by other arguments. When referring to the command by itself, as in "when you run nuget", include the .exe as in "when you run nuget.exe" so your meaning is clear.
- packages.config, project.json: refer to NuGet files in a project.
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

Use standard markdown tables, starting with "| heading | heading | heading |", followed by "| --- | --- | --- |", followed by your rows. The row with "---" is necessary for docs.microsoft.com to read the markdown as a table.

Items in the first column are bolded by default, so you don't need to do that explicitly.

### Screenshots and images

Make all images purposeful and easy to consume; avoid graphics for the sake of graphics. When using a screenshot, include a red rounded-rectangle outline of where the reader's eyes should go. That is, do the work to help the reader look at what you want them to look at, rather than burdening them with having to figure that our for themselves.

If an image has white bleed areas on the edges, draw a 1-pixel gray outline around the entire graphic.

Always include a meaningful description of the image in the markdown alt-text between the []'s. These descriptions are also a great place to put SEO terms that you don't otherwise want to appear in the text.

### Inline code

Delineate inline code with grave accents (backticks), as in `nuget pack`. This inline formatting is used for the following:

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

Code blocks on docs.microsoft.com are delineated by with three grave accents (backticks), ```, at the beginning and the end. You do not need to indent code blocks unless they are contained within a list.

The opening ``` should be followed by a language code for proper syntax coloring, such as "xml", "json", "csharp", etc. Use "bash" for command-line examples and "output" for command-line results.

The only case when you should use ``` without a language tag is when creating a block of fixed-point text that isn't related to any kind of code. In these cases you can also just indent the code block, which can be preferable because it visually separates the code in an editor. See [docs/create-packages/project-json-and-uwp.md](./docs/Create-Packages/project-json-and-UWP.md) for an example.

### Callouts

docs.microsoft.com uses blockquotes for callouts, that is, lines starting with ">".

Callout sections with ">" only will appear with a solid gray line to the left. See [Creating NuGet packages](https://docs.microsoft.com/en-us/nuget/create-packages/creating-a-package) for examples.

You can also use one of the following callout tags on the first line that will create a shaded callout in the indicated color:

| Tag | Shading color | Topic with examples | 
| --- | --- | --- |
| `> [!Note]` | Light blue, use for callouts without any special emphasis. | [Creating NuGet packages](https://docs.microsoft.com/nuget/create-packages/creating-a-package) |
| `> [!Tip]` | Green, use for callouts that share special tips and tricks or other helpful knowledge. | [Package consumption overview](https://docs.microsoft.com/nuget/consume-packages/overview-and-workflow) | 
| `> [!Important]` | Yellow, use for callouts that describe cautions. | [NuGet.Server](https://docs.microsoft.com/nuget/hosting-packages/nuget-server) |
| `> [!Warning]` | Red, use for callouts that warn readers about situations that could cause data loss or unexpected consequences. | [Dependency resolution](https://docs.microsoft.com/nuget/consume-packages/dependency-resolution) |

### Links

- In general, always use the title of the target page as the link text rather than words like "see here" or "this documentation".
- Relative links to other pages in this docset should be in the form `../<folder>/<topic-file>.md` including the trailing `.md`.
- If you're linking to another topic in the same folder, then `../<folder>/` can be omitted.
- When using anchors, always remember to include the `.md` before the `#`.
- When using external links, especially to https://docs.microsoft.com or msdn.microsoft.com, omit any language tag like "en-us" so that a reader in another language lands on a target page in that same language if it's available.
- Bare URLs are not automatically converted into links.

### Inline HTML

If you need to do something that markdown can't handle, use inline HTML. An example is creating a bullet list inside a table.

Use `&lt;` and `&gt;` for < and > characters outside a code block or inline code (delimited by backticks `).

Block-level HTML elements have a few restrictions:

* They must be separated from surrounding text by blank lines.
* The begin and end tags of the outermost block element must not be indented.
* Markdown can't be used within HTML blocks.