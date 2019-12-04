#5.4 Release Notes

[Full Changelog]()

[Issues List](https://github.com/nuget/home/issues?q=is%3Aissue+is%3Aclosed+milestone%3A%225.4")

**Bug:**

* Plugin: Logging time accuracy is off on linux/Mac - [#8747](https://github.com/NuGet/Home/issues/8747)

* Disposing of a plugin can sometimes throw and fail the whole operation. - [#8732](https://github.com/NuGet/Home/issues/8732)

* Stop displaying version duplicates in allowed and blocked versions list in PMUI - [#8679](https://github.com/NuGet/Home/issues/8679)

* Lock File not properly generated - framework ordering should not impact the restore with lockedmode - [#8645](https://github.com/NuGet/Home/issues/8645)

* LockFile validation fails for projects with <RuntimeIdentifiers> set in SDK 3.0.100 - [#8639](https://github.com/NuGet/Home/issues/8639)

* Signing Validation will now properly reject signatures with timestamps which have 2 values under the same OID - [#8629](https://github.com/NuGet/Home/issues/8629)

* Update the license list - [#8544](https://github.com/NuGet/Home/issues/8544)

**Feature:**

* Improve network diagnostics to understand real world http download perf in VS - [#8592](https://github.com/NuGet/Home/issues/8592)

* New helper function - get the top level packages given a package graph. - [#8316](https://github.com/NuGet/Home/issues/8316)

* partial-ngen nuget DLLs which ship in VS to reduce JIT cost in core scenarios - [#6007](https://github.com/NuGet/Home/issues/6007)

**DCR:**

* Onboarding diagnostic files to IFeedbackDiagnosticFileProvider - [#8535](https://github.com/NuGet/Home/issues/8535)

