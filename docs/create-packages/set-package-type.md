---
title: Set a NuGet package type
description: Describes packages types to indicate intended use of a package.
author: karann-msft
ms.author: karann
ms.date: 07/09/2019
ms.topic: conceptual
---

# Set a NuGet package type

With NuGet 3.5+, packages can be marked with a specific *package type* to indicate its intended use. Packages not marked with a type, including all packages created with earlier versions of NuGet, default to the `Dependency` type.

- `Dependency` type packages add build- or run-time assets to libraries and applications, and can be installed in any project type (assuming they are compatible).

- `DotnetCliTool` type packages are extensions to the [dotnet CLI](/dotnet/articles/core/tools/index) and are invoked from the command line. Such packages can be installed only in .NET Core projects and have no effect on restore operations. More details about these per-project extensions are available in the  [.NET Core extensibility](/dotnet/articles/core/tools/extensibility#per-project-based-extensibility) documentation.

- Custom type packages use an arbitrary type identifier that conforms to the same format rules as package IDs. Any type other than `Dependency` and `DotnetCliTool`, however, are not recognized by the NuGet Package Manager in Visual Studio.

Package types are set in the `.nuspec` file. It's best for backwards compatibility to *not* explicitly set the `Dependency` type and to instead rely on NuGet assuming this type when no type is specified.

- `.nuspec`: Indicate the package type within a `packageTypes\packageType` node under the `<metadata>` element:

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <package xmlns="http://schemas.microsoft.com/packaging/2012/06/nuspec.xsd">
        <metadata>
        <!-- ... -->
        <packageTypes>
            <packageType name="DotnetCliTool" />
        </packageTypes>
        </metadata>
    </package>
    ```
