# NuGet 3.4.3 Release Notes

NuGet 3.4.3 was released on April 22, 2016 to address several issues that were identified in the 3.4 and subsequent releases.

You can download both the VSIX and NuGet.exe [here](https://dist.nuget.org/index.html).

## Updates and Improvements

* Improved Visual Studio reliability. We have fixed some issues in NuGet that caused crashes in Visual Studio.

##Fixes

* Fixed some authorization issues with password protected private nuget feeds.
* Fixed an issue around being unable to restore PCL's from project.json with runtimes specified.
* Some customers were running into intermittent failures when installing packages. This has now been fixed in this release.
* Fixed an issue that caused restore failures in C++/CLI projects with project.json.
* Some packages (E.g ModernHttpClient) where not being unzipped correctly when you use nuget in mono. This has now been fixed in this release.

For the complete list of fixes and improvements in this release, check out the list of issues [here](https://github.com/NuGet/Home/issues?q=is%3Aissue+milestone%3A3.4.3+is%3Aclosed).
