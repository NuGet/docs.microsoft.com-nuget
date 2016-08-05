# Search Syntax

NuGet package search works the same whether you use NuGet Gallery, or NuGet Package Manager extension for Visual Studio. In other words: NuGet Gallery, NuGet command line, Package Manager Console and Dialogs all share the same search formats.

## Keywords

    modern UI javascript

Search will do its best effort to find relevant documents containing all 3 keywords, and return matching documents.

## Phrases and keywords

    "modern UI" package

Entering a phrase between quotation marks ("") change the search to look for the particular phrase instead of separate keywords. 
Matching documents should usually contain the exact phrase "modern UI", including variations on capitalization e.g. 
"Modern ui", and also usually contain the word 'package'.

## Filtering

You can search for a specific package ID (or 'Id' or 'id'), or certain other fields by prefixing search terms with the field name.

    Searchable fields are 'Id', 'Version', 'Title', 'Tags', 'Author', 'Description', 'Summary' and 'Owner'.

What's the difference between ID and Title? ID is the name you use in package management console. Title is what is shown at the top of the package page in search results.

### Examples

    ID:jQuery
    id:NuGet.Core

finds packages with "jQuery" or "NuGet.Core" in their ID field respectively.

    Id:"Nuget.Core"

is another way to find packages with "NuGet.Core" in their ID field.

The 'Id' filter is a substring match, so if you search for the following:

    Id:jquery
    
You'll get results like 'jQuery.UI.Combined' and 'jQuery.Validation'. If you want to perform an exact search, use the 'packageid' field. This will filter to show **only** the one package with the ID you specified (or nothing if there is no package with that ID). For example:

    PackageId:jquery

You can also search for multiple keywords in a single field. Or mix and match fields.

    id:jquery tags:validation

To search for packages containing jquery in their id and validation in their tags.

    id:jquery id:ui

To search for packages containing jquery or ui in their id.

And you can perform phrase searches:

    id:"jquery.ui"
    
Finally, if you use a field we don't support, such as 'invalid', we'll just ignore it and search all the fields. So the following query

    invalid:jquery ui
    
Is interpreted exactly the same as this query:

    jquery ui
