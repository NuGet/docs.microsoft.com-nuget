The package identifier and the version number uniquely identify the exact code that's contained in the package.

Follow these best practices to create the package identifier:

- The identifier must be *unique* across nuget.org and all other locations that host the package. To avoid conflicts, a good pattern is to use your company name as the first part of the identifier.
- Follow a *.NET namespace-like naming convention*, using dot notation. For example, use `Contoso.Utility.UsefulStuff` rather than `Contoso-Utility-UsefulStuff` or `Contoso_Utility_UsefulStuff`. It's also helpful for consumers if you match the package identifier to the namespace the code uses.
- If you produce a package of *sample code* that demonstrates how to use another package, append `.Sample` to the identifier, as in `Contoso.Utility.UsefulStuff.Sample`.

  The sample package has a dependency on the original package. When you create the sample package, add `<IncludeAssets>` with the `contentFiles` value. In the *content* folder, arrange the sample code in a folder called *\\Samples\\\<identifier>*, such as *\\Samples\\Contoso.Utility.UsefulStuff.Sample*.

Follow these best practices to set the package version:

- In general, set the package version to *match the project or assembly version*, although this isn't strictly required. Matching the version is simple when you limit a package to a single assembly. NuGet itself deals with package versions when resolving dependencies, not assembly versions.

- If you use a non-standard version scheme, be sure to consider the NuGet versioning rules as explained in [Package versioning](../../concepts/package-versioning.md). NuGet is mostly [Semantic Versioning 2.0.0](../../concepts/package-versioning.md#semantic-versioning-200)-compliant.

>[!NOTE]
> For more information about dependency resolution, see [Dependency resolution with PackageReference](../../concepts/dependency-resolution.md#dependency-resolution-with-packagereference). For older information that might help you understand versioning, see this series of blog posts:
>
> - [Part 1: Taking on DLL Hell](https://blog.davidebbo.com/2011/01/nuget-versioning-part-1-taking-on-dll.html)
> - [Part 2: The core algorithm](https://blog.davidebbo.com/2011/01/nuget-versioning-part-2-core-algorithm.html)
> - [Part 3: Unification via binding redirects](https://blog.davidebbo.com/2011/01/nuget-versioning-part-3-unification-via.html)
