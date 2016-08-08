# Creating Localized Packages

## Localization Options

There are two options for providing a localized experience for your library package:

1. Include your localized satellite assemblies in the same NuGet package as your runtime assemblies.  This has always been supported. Note: Currently, NuGet pack doesn't support packing satellite assemblies automatically. You need to specify satellite assemblies manually in Nuspec.
1. Create separate localized satellite packages that follow a very strict convention.  This approach has been supported since [NuGet 1.8](../Release-Notes/NuGet-1.8#Satellite-Packages-for-Localized-Resources).

Different libraries have different localization requirements, so it's important to consider the differences between these two approaches.

## Single Package Approach

Usually the simplest approach for localization is to include all of the localized satellite assemblies and XML IntelliSense in the same package as your runtime.  Here's an example package layout that accomplishes this.

**_SuperAwesomeness.1.0.0.nupkg_**

* Id: SuperAwesomeness
* Version: 1.0.0
* Title: Super Awesomeness
* Summary: Super Awesome features for your application
* Description: Super Awesomeness provides lots of super awesome features that you can use in your application
* Language: en-us

    <pre>
    lib
    └───net40
        │   SuperAwesomeness.dll
        │   SuperAwesomeness.xml
        │
        ├───de
        │       SuperAwesomeness.resources.dll
        │       SuperAwesomeness.xml
        │
        ├───it
        │       SuperAwesomeness.resources.dll
        │       SuperAwesomeness.xml
        │
        ├───ja
        │       SuperAwesomeness.resources.dll
        │       SuperAwesomeness.xml
        │
        ├───ru
        │       SuperAwesomeness.resources.dll
        │       SuperAwesomeness.xml
        │
        ├───zh-Hans
        │       SuperAwesomeness.resources.dll
        │       SuperAwesomeness.xml
        │
        └───zh-Hant
                SuperAwesomeness.resources.dll
                SuperAwesomeness.xml
    </pre>

This package contains a single class library (SuperAwesomeness.dll) that contains the English strings as part of the runtime assembly.  The package also contains localized satellite assemblies and XML IntelliSense files for German, Italian, Japanese, Russian, Chinese (Simplified) and Chinese (Traditional).

Installing this package into your project will yield a fully localized experience in any of the languages it supports.

### Disadvantages to the Single Package Approach

There are a few disadvantages to bundling your localized files into your runtime package.

1. **Package Size**.  Whether or not this is a problem will depend on how many localized strings your library has, but it's something to be mindful of.
1. **Package Metadata**.  This is typically the more concerning issue.  A NuGet package can only contain a single nuspec file that can only contain a single language's metadata.  There is no support for localized package metadata within a single NuGet package.
1. **Simultaneous Shipping**.  Bundling your localized files into your runtime package requires that you simultaneously ship all assets in the single package.  There's no option to ship the default language first and later ship the localized experience.  Any update to your localized experience requires a new version of the runtime package even if the runtime assemblies didn't change.

### Example of the Single Package Approach

As of version 5.4.0 The [Microsoft.Data.OData package](http://nuget.org/packages/Microsoft.Data.OData/5.4.0) utilizes the single package approach.

## Satellite Package Approach

The satellite package approach allows you to separate your localized resources from your runtime assemblies, much like the .NET Framework supports satellite assemblies.  Here is a sample package structure that includes all of the packages involved.

**_SuperAwesomeness.1.0.0.nupkg_**

* Id: SuperAwesomeness
* Version: 1.0.0
* Title: Super Awesomeness
* Summary: Super Awesome features for your application
* Description: Super Awesomeness provides lots of super awesome features that you can use in your application
* Language: en-us

    <pre>
    lib
    └───net40
            SuperAwesomeness.dll
            SuperAwesomeness.xml
    </pre>

**_SuperAwesomeness.de.1.0.0.nupkg_**

* Id: SuperAwesomeness.de
* Version: 1.0.0
* Title: The German translation of "Super Awesomeness German Resources"
* Summary: The German translation of "German resources for the SuperAwesomeness package"
* Description: The German translation of "This package contains the German satellite assemblies for Super Awesomeness"
* Language: de
* Dependency: SuperAwesomeness \[1.0.0\]

    <pre>
    lib
    └───net40
        └───de
                SuperAwesomeness.resources.dll
                SuperAwesomeness.xml
    </pre>

**_SuperAwesomeness.it.1.0.0.nupkg_**

* Id: SuperAwesomeness.it
* Version: 1.0.0
* Title: The Italian translation of "Super Awesomeness Italian Resources"
* Summary: The Italian translation of "Italian resources for the SuperAwesomeness package"
* Description: The Italian translation of "This package contains the Italian satellite assemblies for Super Awesomeness"
* Language: it
* Dependency: SuperAwesomeness \[1.0.0\]

    <pre>
    lib
    └───net40
        └───it
                SuperAwesomeness.resources.dll
                SuperAwesomeness.xml
    </pre>

**_SuperAwesomeness.ja.1.0.0.nupkg_**

* Id: SuperAwesomeness.ja
* Version: 1.0.0
* Title: The Japanese translation of "Super Awesomeness Japanese Resources"
* Summary: The Japanese translation of "Japanese resources for the SuperAwesomeness package"
* Description: The Japanese translation of "This package contains the Japanese satellite assemblies for Super Awesomeness"
* Language: ja
* Dependency: SuperAwesomeness \[1.0.0\]

    <pre>
    lib
    └───net40
        └───ja
                SuperAwesomeness.resources.dll
                SuperAwesomeness.xml
    </pre>

**_SuperAwesomeness.ru.1.0.0.nupkg_**

* Id: SuperAwesomeness.ru
* Version: 1.0.0
* Title: The Russian translation of "Super Awesomeness Russian Resources"
* Summary: The Russian translation of "Russian resources for the SuperAwesomeness package"
* Description: The Russian translation of "This package contains the Russian satellite assemblies for Super Awesomeness"
* Language: ru
* Dependency: SuperAwesomeness \[1.0.0\]

    <pre>
    lib
    └───net40
        └───ru
                SuperAwesomeness.resources.dll
                SuperAwesomeness.xml
    </pre>

**_SuperAwesomeness.zh-Hans.1.0.0.nupkg_**

* Id: SuperAwesomeness.zh-Hans
* Version: 1.0.0
* Title: The Chinese (Simplified) translation of "Super Awesomeness Chinese (Simplified) Resources"
* Summary: The Chinese (Simplified) translation of "Chinese (Simplified) resources for the SuperAwesomeness package"
* Description: The Chinese (Simplified) translation of "This package contains the Chinese (Simplified) satellite assemblies for Super Awesomeness"
* Language: zh-Hans
* Dependency: SuperAwesomeness \[1.0.0\]

    <pre>
    lib
    └───net40
        └───zh-Hans
                SuperAwesomeness.resources.dll
                SuperAwesomeness.xml
    </pre>

**_SuperAwesomeness.zh-Hant.1.0.0.nupkg_**

* Id: SuperAwesomeness.zh-Hant
* Version: 1.0.0
* Title: The Chinese (Traditional) translation of "Super Awesomeness Chinese (Traditional) Resources"
* Summary: The Chinese (Traditional) translation of "Chinese (Traditional) resources for the SuperAwesomeness package"
* Description: The Chinese (Traditional) translation of "This package contains the Chinese (Traditional) satellite assemblies for Super Awesomeness"
* Language: zh-Hant
* Dependency: SuperAwesomeness \[1.0.0\]

    <pre>
    lib
    └───net40
        └───zh-Hant
                SuperAwesomeness.resources.dll
                SuperAwesomeness.xml
    </pre>

If a developer installs this full set of packages, the same fully localized experience will be accomplished as the Single Package Approach.  The satellite assemblies and localized IntelliSense will be utilized.

### Benefits and Disadvantages to the Satellite Package Approach

The satellite package approach offers the following benefits:

1. **Package Size**.  The runtime package is kept trim, and package consumers can opt into pulling down the additional localized bits, language by language.
1. **Package Metadata**.  Because there are now packages that have localized metadata, some users might be able to find your package more easily if you include a fully localized title, summary, and description for your package.
1. **Deferred Localization**.  You have the opportunity to ship the satellite packages separately from your runtime, deferring localization until after you publish the runtime package.

However, satellite packages have their own set of disadvantages:

1. **Clutter**.  Instead of a single package, you end up having a package for each language you support.  This can lead to cluttered search results.  Satellite packages are not differentiated on the nuget.org gallery or in search results, so they show up as peers of the runtime packages.
1. **Strict Conventions**.  There are very strict conventions for satellite packages in order for them to be recognized as satellite packages.  Those details are below and they must be followed precisely or the localized experience will not be achieved.
1. **Versioning**.  One of the conventions is that satellite packages must have an exact version dependency on the associated runtime package.  That means that any time a new runtime package version is published, you must publish updated satellite packages as well, even if the localized resources didn't change.  There are more details below.

### Satellite Package Conventions

Satellite packages work by way of a strict set of conventions.  Aside from these conventions, satellite packages are no different from any other NuGet package.  In order for the package to be treated as a satellite package though, all of the conventions must be followed.

1. The Id of the package must match that of the runtime package, followed by a dot and then the target language as a suffix.  For example, "SuperAwesomeness.ja" is the Japanese satellite package for the "SuperAwesomeness" package.
1. The Language element in the nuspec must be set to the target language.  Unless specific subcultures are being provided, the higher level culture is recommended.  For example, use "ja" instead of "ja-JP" for Japanese packages.  The language element's value must exactly match the language suffix on the package Id.
1. The satellite package must have a dependency on the runtime package.  For example, the "SuperAwesomeness.ja" package must have a dependency on the "SuperAwesomeness" package.
1. The dependency on the runtime package must be for an exact version and not a range.  This is specified using the square brackets in the [dependency version](Versioning).  For instance, the "SuperAwesomeness.ja" package's dependency on "SuperAwesomeness" uses the version range of \[1.0.0\].  Note that the version of the satellite package doesn't need to match the version of the runtime package.  However, a satellite package can only target a single version of the runtime package.  Therefore, it's possible to have SuperAwesomeness.ja version 1.0.1 target (and therefore depend on) SuperAwesomeness version 1.0.0.
1. Only culture-specific files within the lib folder will be recognized.  Referring to the SuperAwesomeness.ja.1.0.0.nupkg file above, the package illustrates having files under the \lib\ja\ folder.  These files will be recognized because they A) are under the \lib folder, and B) there's a subfolder named "ja" which matches the target language for the package.

By following these conventions, NuGet recognizes that the package is a satellite package at the time of installation.  When that is identified, the localized files in the lib folder are copied into the runtime package's lib folder at the time of package installation.  When the satellite package is uninstalled, the localized files are removed from the runtime package's lib folder.  Once the localized files are copied into the runtime package's lib folder, Visual Studio and MSBuild do the rest of the work.

### Example Satellite Packages

ASP.NET MVC package is one example that utilizes the satellite package approach.  Here are some of the related packages:

* [Microsoft.AspNet.Mvc](http://nuget.org/packages/Microsoft.AspNet.Mvc) - Runtime and English resources
* [Microsoft.AspNet.Mvc.de](http://nuget.org/packages/Microsoft.AspNet.Mvc.de) - German resources
* [Microsoft.AspNet.Mvc.ja](http://nuget.org/packages/Microsoft.AspNet.Mvc.ja) - Japanese resources
* [Microsoft.AspNet.Mvc.zh-Hans](http://nuget.org/packages/Microsoft.AspNet.Mvc.zh-Hans) - Chinese (Simplified) resources
* [Microsoft.AspNet.Mvc.zh-Hant](http://nuget.org/packages/Microsoft.AspNet.Mvc.zh-Hant) - Chinese (Traditional) resources

