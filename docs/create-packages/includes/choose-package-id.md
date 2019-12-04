The package identifier and the version number are the two most important values in the project because they uniquely identify the exact code that's contained in the package.

**Best practices for the package identifier:**

- **Uniqueness**: The identifier must be unique across nuget.org or whatever gallery hosts the package. Before deciding on an identifier, search the applicable gallery to check if the name is already in use. To avoid conflicts, a good pattern is to use your company name as the first part of the identifier, such as `Contoso.`.
- **Namespace-like names**: Follow a pattern similar to namespaces in .NET, using dot notation instead of hyphens. For example, use `Contoso.Utility.UsefulStuff` rather than `Contoso-Utility-UsefulStuff` or `Contoso_Utility_UsefulStuff`. Consumers also find it helpful when the package identifier matches the namespaces used in the code.
- **Sample Packages**: If you produce a package of sample code that demonstrates how to use another package, attach `.Sample` as a suffix to the identifier, as in `Contoso.Utility.UsefulStuff.Sample`. (The sample package would of course have a dependency on the other package.) When creating a sample package, use the `contentFiles` value in `<IncludeAssets>`. In the `content` folder, arrange the sample code in a folder called `\Samples\<identifier>` as in `\Samples\Contoso.Utility.UsefulStuff.Sample`.

**Best practices for the package version:**

- In general, set the version of the package to match the project (or assembly), though this is not strictly required. This is a simple matter when you limit a package to a single assembly. Overall, remember that NuGet itself deals with package versions when resolving dependencies, not assembly versions.
- When using a non-standard version scheme, be sure to consider the NuGet versioning rules as explained in [Package versioning](../../concepts/package-versioning.md). NuGet is mostly [semver 2 compliant](../../concepts/package-versioning.md#semantic-versioning-200).

> For information on dependency resolution, see [Dependency resolution with PackageReference](../../concepts/dependency-resolution.md#dependency-resolution-with-packagereference). For older information that may also be helpful to better understand versioning, see this series of blog posts.
>
> - [Part 1: Taking on DLL Hell](https://blog.davidebbo.com/2011/01/nuget-versioning-part-1-taking-on-dll.html)
> - [Part 2: The core algorithm](https://blog.davidebbo.com/2011/01/nuget-versioning-part-2-core-algorithm.html)
> - [Part 3: Unification via Binding Redirects](https://blog.davidebbo.com/2011/01/nuget-versioning-part-3-unification-via.html)
