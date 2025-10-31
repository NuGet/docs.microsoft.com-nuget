---
title: How to Create a Localized NuGet Package
description: Details on the two ways to create localized NuGet packages, either by including all assemblies in a single package or publishing separate assemblies.
author: JonDouglas
ms.author: jodou
ms.date: 01/18/2018
ms.topic: how-to
---

# Creating localized NuGet packages

There are two ways to create localized versions of a library:

1. Include all localized resources assemblies in a single package.
1. Create separate localized satellite packages by following a strict set of conventions.

Both methods have their advantages and disadvantages, as described in the following sections.

## Localized resource assemblies in a single package

Including localized resource assemblies in a single package is typically the simplest approach. To do this, create folders within `lib` for supported language other than the package default (assumed to be en-us). In these folders you can place resource assemblies and localized IntelliSense XML files.

For example, the following folder structure supports, German (de), Italian (it), Japanese (ja), Russian (ru), Chinese (Simplified) (zh-Hans), and Chinese (Traditional) (zh-Hant):

```
lib
└───net40
    │   Contoso.Utilities.dll
    │   Contoso.Utilities.xml
    │
    ├───de
    │       Contoso.Utilities.resources.dll
    │       Contoso.Utilities.xml
    │
    ├───it
    │       Contoso.Utilities.resources.dll
    │       Contoso.Utilities.xml
    │
    ├───ja
    │       Contoso.Utilities.resources.dll
    │       Contoso.Utilities.xml
    │
    ├───ru
    │       Contoso.Utilities.resources.dll
    │       Contoso.Utilities.xml
    │
    ├───zh-Hans
    │       Contoso.Utilities.resources.dll
    │       Contoso.Utilities.xml
    │
    └───zh-Hant
            Contoso.Utilities.resources.dll
            Contoso.Utilities.xml
```

You can see that the languages are all listed underneath the `net40` target framework folder. If you're [supporting multiple frameworks](../create-packages/supporting-multiple-target-frameworks.md), then you have a folder under `lib` for each variant.

With these folders in place, you then reference all the files in your `.nuspec`:

```xml
<?xml version="1.0"?>
<package>
    <metadata>...
    </metadata>
    <files>
    <file src="lib\**" target="lib" />
    </files>
</package>
```

One example package that uses this approach is [Microsoft.Data.OData 5.4.0](https://nuget.org/packages/Microsoft.Data.OData/5.4.0).

### Advantages and disadvantages (localized resource assemblies)

Bundling all languages in a single package has a few disadvantages:

1. **Shared metadata**: Because a NuGet package can only contain a single `.nuspec` file, you can provide metadata for only a single language. That is, NuGet does not present support localized metadata.
1. **Package size**: Depending on the number of languages you support, the library can become considerably large, which slows installing and restoring the package.
1. **Simultaneous releases**: Bundling localized files into a single package requires that you release all assets in that package simultaneously, rather than being able to release each localization separately. Furthermore, any update to any one localization requires a new version of the entire package.

However, it also has a few benefits:

1. **Simplicity**: Consumers of the package get all supported languages in a single install, rather than having to install each language separately. A single package is also easier to find on nuget.org.
1. **Coupled versions**: Because all of the resource assemblies are in the same package as the primary assembly, they all share the same version number and don't run a risk of getting erroneously decoupled.

## Localized satellite packages

Similar to how .NET Framework supports satellite assemblies, this method separates localized resources and IntelliSense XML files into satellite packages.

Do to this, your primary package uses the naming convention `{identifier}.{version}.nupkg` and contains the assembly for the default language (such as en-US). For example, `ContosoUtilities.1.0.0.nupkg` would contain the following structure:

```
lib
└───net40
        ContosoUtilities.dll
        ContosoUtilities.xml
```

A satellite assembly then uses the naming convention `{identifier}.{language}.{version}.nupkg`, such as `ContosoUtilities.de.1.0.0.nupkg`. The identifier **must** exactly match that of the primary package.

Because this is a separate package, it has its own `.nuspec` file that contains localized metadata. Be mindful that the language in the `.nuspec` **must** match the one used in the filename.

The satellite assembly **must** also declare an exact version of the primary package as a dependency, using the [] version notation (see [Package versioning](../concepts/package-versioning.md)). For example, `ContosoUtilities.de.1.0.0.nupkg` must declare a dependency on `ContosoUtilities.1.0.0.nupkg` using the `[1.0.0]` notation. The satellite package can, of course, have a different version number than the primary package.

The satellite package's structure must then include the resource assembly and XML IntelliSense file in a subfolder that matches `{language}` in the package filename:

```
lib
└───net40
    └───de
            ContosoUtilities.resources.dll
            ContosoUtilities.xml
```

**Note**: unless specific subcultures such as `ja-JP` are necessary, always use the higher level language identifier, like `ja`.

In a satellite assembly, NuGet will recognize **only** those files in the folder that matches the `{language}` in the filename. All others are ignored.

When all of these conventions are met, NuGet will recognize the package as a satellite package and install the localized files into the primary package's `lib` folder, as if they had been originally bundled. Uninstalling the satellite package will remove its files from that same folder.

You would create additional satellite assemblies in the same way for each supported language. For an example, examine the set of ASP.NET MVC packages:

- [Microsoft.AspNet.Mvc](https://nuget.org/packages/Microsoft.AspNet.Mvc) (English primary)
- [Microsoft.AspNet.Mvc.de](https://nuget.org/packages/Microsoft.AspNet.Mvc.de) (German)
- [Microsoft.AspNet.Mvc.ja](https://nuget.org/packages/Microsoft.AspNet.Mvc.ja) (Japanese)
- [Microsoft.AspNet.Mvc.zh-Hans](https://nuget.org/packages/Microsoft.AspNet.Mvc.zh-Hans) (Chinese (Simplified))
- [Microsoft.AspNet.Mvc.zh-Hant](https://nuget.org/packages/Microsoft.AspNet.Mvc.zh-Hant) (Chinese (Traditional))

### Summary of required conventions

- Primary package must be named `{identifier}.{version}.nupkg`
- A satellite package must be named `{identifier}.{language}.{version}.nupkg`
- A satellite package's `.nuspec` must specify its language to match the filename.
- A satellite package must declare a dependency on an exact version of the primary using the [] notation in its `.nuspec` file. Ranges are not supported.
- A satellite package must place files in the `lib\[{framework}\]{language}` folder that exactly matches `{language}` in the filename.

### Advantages and disadvantages (satellite packages)

Using satellite packages has a few benefits:

1. **Package size**: The overall footprint of the primary package is minimized, and consumers only incur the costs of each language they want to use.
1. **Separate metadata**: Each satellite package has its own `.nuspec` file and thus its own localized metadata. This can allow some consumers to find packages more easily by searching nuget.org with localized terms.
1. **Decoupled releases**: Satellite assemblies can be released over time, rather than all at once, allowing you to spread out your localization efforts.

However, satellite packages have their own set of disadvantages:

1. **Clutter**: Instead of a single package, you have many packages that can lead to cluttered search results on nuget.org and a long list of references in a Visual Studio project.
1. **Strict conventions**. Satellite packages must follow the conventions exactly or the localized versions won't be picked up properly.
1. **Versioning**: Each satellite package must have an exact version dependency on the primary package. This means that updating the primary package may require updating all satellite packages as well, even if the resources didn't change.
