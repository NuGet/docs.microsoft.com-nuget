#5.1 Release Notes

[Full Changelog]()

[Issues List](https://github.com/nuget/home/issues?q=is%3Aissue+is%3Aclosed+milestone%3A%225.1")

**Bug:**

* Plugins:  exception details lost during plugin creation - [#8057](https://github.com/NuGet/Home/issues/8057)

* PackageReference range with exclusive lower bound does not work if the lower bound is present on one of the sources. - [#8054](https://github.com/NuGet/Home/issues/8054)

* Improve IsPackableFalseError message - [#8021](https://github.com/NuGet/Home/issues/8021)

* Packages Lock File - regenerate lock file when project graph changes - [#8019](https://github.com/NuGet/Home/issues/8019)

* ProjectSystem bug: Nuget Packages getting auto removed - [#8017](https://github.com/NuGet/Home/issues/8017)

* Add a target for returning the FrameworkReference similar to CollectPackageDownloads and CollectPackageReferences - [#8005](https://github.com/NuGet/Home/issues/8005)

* HTTP cache:  RepositoryResources resource is not cached in a versioned way - [#7997](https://github.com/NuGet/Home/issues/7997)

* Logging:  exception callstacks are not reported with detailed verbosity - [#7955](https://github.com/NuGet/Home/issues/7955)

* Change all NuGet Docs URLs to use HTTPS - [#7950](https://github.com/NuGet/Home/issues/7950)

* Improve NU3024 warning message - [#7933](https://github.com/NuGet/Home/issues/7933)

* lock file not updating when packagereference removed - [#7930](https://github.com/NuGet/Home/issues/7930)

* Improve the error case handling when validating licenseurl and license element in nuspec - [#7915](https://github.com/NuGet/Home/issues/7915)

* PM UI - right click on tab header and clicking "Open file location" results in error - [#7913](https://github.com/NuGet/Home/issues/7913)

* Plugins:  log when plugin process exits - [#7907](https://github.com/NuGet/Home/issues/7907)

* Plugins:  high collision rate in logging datetime values - [#7899](https://github.com/NuGet/Home/issues/7899)

* Manifest.ReadFrom fails on any nuspec with LicenseExpression - [#7894](https://github.com/NuGet/Home/issues/7894)

* RestoreLockedMode: Unexpected NU1004 when ProjectReference refers to a project with custom AssemblyName - [#7889](https://github.com/NuGet/Home/issues/7889)

* Better error message when the plugin startup fails with an exception - [#7857](https://github.com/NuGet/Home/issues/7857)

* When doing a NoOp restore, avoid *.dgspec.json write in obj directory - [#7854](https://github.com/NuGet/Home/issues/7854)

* GeneratePathProperty=true fails to generate property on case mismatch - [#7843](https://github.com/NuGet/Home/issues/7843)

* Settings:  illegal character in package source path can crash VS - [#7820](https://github.com/NuGet/Home/issues/7820)

* If lock file is deleted, restore does not generate lock file on NoOp  - [#7807](https://github.com/NuGet/Home/issues/7807)

* License URL and license causes read error with Metadata - [#7547](https://github.com/NuGet/Home/issues/7547)

* Unhandled exceptions in V2FeedParser - [#7523](https://github.com/NuGet/Home/issues/7523)

* nuget.exe returns exit code zero for invalid arguments - [#7178](https://github.com/NuGet/Home/issues/7178)

* Update Errors and warning docs to reflect signing related scenarios - [#6498](https://github.com/NuGet/Home/issues/6498)

* Assets file should use relative paths to enable moving projects more easily - [#4582](https://github.com/NuGet/Home/issues/4582)


**DCR:**

* Plugins:  enable diagnostic logging - [#7859](https://github.com/NuGet/Home/issues/7859)

* Make Tizen 6 map to NetStandard 2.1 - [#7773](https://github.com/NuGet/Home/issues/7773)

**Feature:**

* Represent FrameworkReferences in NuGet - pack & restore support - [#7342](https://github.com/NuGet/Home/issues/7342)

* Support "download only" package scenario with PackageDownload - [#7339](https://github.com/NuGet/Home/issues/7339)

* Add PackageType for runtime and targeting packs to exclude them from search results & restore graph. - [#7337](https://github.com/NuGet/Home/issues/7337)

* Link VS Package Entries to Gallery Package Pages - [#5299](https://github.com/NuGet/Home/issues/5299)

* Skip Duplicate switch added to nuget.exe push command - [#1630](https://github.com/NuGet/Home/issues/1630)

