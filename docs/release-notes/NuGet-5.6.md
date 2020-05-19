# 5.6 Release Notes

[Full Changelog]("")

[Issues List](https://app.zenhub.com/workspaces/nuget-client-team-55aec9a240305cf007585881/reports/release?release=5e3b2080c4b30708e48bf9f3)

**DCR:**

* Support pre-release packages with floating versions. Version="*-*" and similar. - [#912](https://github.com/NuGet/Home/issues/912)

* add support to `NuGet.exe update` for -DependencyVersion parameter, like install command - [#7694](https://github.com/NuGet/Home/issues/7694)

* Default verbosity should not report each project's noop restore. - [#8792](https://github.com/NuGet/Home/issues/8792)

* Sort packages by ID in the Updates tab of the Package Manager UI - [#9278](https://github.com/NuGet/Home/issues/9278)

* net5.0 tfm - initial support - [#9584](https://github.com/NuGet/Home/issues/9584)

**Bug:**

* dotnet nuget push - Missing value for option - [#4864](https://github.com/NuGet/Home/issues/4864)

* `nuget push *.nupkg` fails with snupkg does not exist - [#8148](https://github.com/NuGet/Home/issues/8148)

* Pack, and several other code paths, fails dependant on locale. Use RegexOptions.CultureInvariant - [#8246](https://github.com/NuGet/Home/issues/8246)

* Perf: DG Spec for unloaded project scenarios should not be written in preview restores - [#8793](https://github.com/NuGet/Home/issues/8793)

* Restore:  improve performance by caching solution dependency graph spec - [#9201](https://github.com/NuGet/Home/issues/9201)

* NuGet UI doesn't work for sdk style projects after installing a  package with PMConsole - [#9203](https://github.com/NuGet/Home/issues/9203)

* [Test Failure] Embedded icon can’t be shown in PM UI with local package feed - depending on / vs \ - [#9225](https://github.com/NuGet/Home/issues/9225)

* NuGetVersion.TryParseStrict() should return false if it fails to parse - [#9255](https://github.com/NuGet/Home/issues/9255)

* `nuget.exe push` help for `-source`, should be source name, not source uri - [#9265](https://github.com/NuGet/Home/issues/9265)

* `dotnet nuget add package SourceUri`  creates bad default package source name - [#9277](https://github.com/NuGet/Home/issues/9277)

* Screen reader doesn't announces "Searching..." message when switching tabs - [#9307](https://github.com/NuGet/Home/issues/9307)

* [Test Failure] Focus-rect color in themes for PM UI tabs - [#9336](https://github.com/NuGet/Home/issues/9336)

* Investigate Accessibility bugs reported by the Community - [#9393](https://github.com/NuGet/Home/issues/9393)

* nuget.exe 5.5 fails to restore with MSBuild 14 or below - [#9458](https://github.com/NuGet/Home/issues/9458)

* Don't bother logging small millisecond times in restore messages - [#8977](https://github.com/NuGet/Home/issues/8977)

* Make IOutputConsole async - [#9268](https://github.com/NuGet/Home/issues/9268)

* MsBuild version picking works badly on some non-english cultures - [#9322](https://github.com/NuGet/Home/issues/9322)

* Missing format default on sources list - [#9337](https://github.com/NuGet/Home/issues/9337)

* Highest pre-release dependencies not installed - [#240](https://github.com/NuGet/Home/issues/240)

* Perf: RestoreOperationLogger has unnecessary thread affinity - [#9288](https://github.com/NuGet/Home/issues/9288)

* Automated creation of docs for `dotnet nuget` commands. - [#9146](https://github.com/NuGet/Home/issues/9146)




