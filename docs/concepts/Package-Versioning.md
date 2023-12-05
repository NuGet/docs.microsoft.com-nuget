---
title: NuGet Package Version Reference
description: Exact details on specifying version numbers and ranges for other packages upon which a NuGet package depends, and how dependencies are installed.
author: JonDouglas
ms.author: jodou
ms.date: 03/23/2018
ms.topic: reference
ms.reviewer: anangaur
---

# Package versioning

A specific package is always referred to using its package identifier and an exact version number. For example, [Entity Framework](https://www.nuget.org/packages/EntityFramework/) on nuget.org has several dozen specific packages available, ranging from version *4.1.10311* to version *6.1.3* (the latest stable release) and a variety of pre-release versions like *6.2.0-beta1*.

When creating a package, you assign a specific version number with an optional pre-release text suffix. When consuming packages, on the other hand, you can specify either an exact version number or a range of acceptable versions.

The following document follows the [Semantic Versioning 2.0.0](https://semver.org/spec/v2.0.0.html) standard, supported by NuGet 4.3.0+ and Visual Studio 2017 version 15.3+.
Certain [semantics of SemVer v2.0.0](#semantic-versioning-200) are not supported in older clients.

In this topic:

- [Version basics](#version-basics) including pre-release suffixes.
- [Version ranges](#version-ranges)
- [Normalized version numbers](#normalized-version-numbers)
- [Semantic Versioning 2.0.0](#semantic-versioning-200)

## Version basics

A specific version number is in the form *Major.Minor.Patch[-Suffix]*, where the components have the following meanings:

- *Major*: Breaking changes
- *Minor*: New features, but backwards compatible
- *Patch*: Backwards compatible bug fixes only
- *-Suffix* (optional): a hyphen followed by a string denoting a pre-release version (following the [Semantic Versioning or SemVer](https://semver.org/) convention).

**Examples:**

```
1.0.1
6.11.1231
4.3.1-rc
2.2.44-beta.1
```

> [!Important]
> nuget.org rejects any package upload that lacks an exact version number. The version must be specified in the `.nuspec` or project file used to create the package.

### Pre-release versions

Technically speaking, package creators can use any string as a suffix to denote a pre-release version, as NuGet treats any such version as pre-release and makes no other interpretation. That is, NuGet displays the full version string in whatever UI is involved, leaving any interpretation of the suffix's meaning to the consumer.

That said, package developers generally follow recognized naming conventions:

- `-alpha`: Alpha release, typically used for work-in-progress and experimentation.
- `-beta`: Beta release, typically one that is feature complete for the next planned release, but may contain known bugs.
- `-rc`: Release candidate, typically a release that's potentially final (stable) unless significant bugs emerge.

When resolving package references and multiple package versions differ only by suffix, NuGet chooses a version without a suffix first, then applies precedence to pre-release versions in reverse alphabetical order and treats dot notation numbers with numerical order. For example, the following versions would be chosen in the exact order shown:

> [!Note]
> Prerelease numbers with dot notation, as in *1.0.1-build.23*, are considered are part of the [SemVer 2.0.0](https://semver.org/spec/v2.0.0.html) standard, and as such are [only supported with NuGet 4.3.0+](#semantic-versioning-200).

### [SemVer 2.0 sorting](#tab/semver20sort)

```
1.0.1
1.0.1-zzz
1.0.1-rc.10
1.0.1-rc.2
1.0.1-open
1.0.1-beta
1.0.1-alpha2
1.0.1-alpha10
1.0.1-aaa
```

Note that 1.0.1-alpha10 is sorted strictly in reverse alphabetical order, whereas 1.0.1-rc.10 is greater precedence than 1.0.1-rc.2.

### [SemVer 1.0 sorting](#tab/semver10sort)

```
1.0.1
1.0.1-zzz
1.0.1-open
1.0.1-beta05
1.0.1-beta02
1.0.1-beta
1.0.1-alpha2
1.0.1-alpha10
1.0.1-aaa
```

Note that versions such as `1.0.1-rc.10` and `1.0.1-rc.2` are not parsable by older versions of the client, and such packages with those versions won't be available for download with those clients.

If you use numerical suffixes with pre-release tags that might use double-digit numbers (or more), use leading zeroes as in beta01 and beta05 to ensure that they sort correctly when the numbers get larger. This recommendation only applies this schema.

---

## Version ranges

When referring to package dependencies, NuGet supports using interval notation for specifying version ranges, summarized as follows:

| Notation | Applied rule | Description |
|----------|--------------|-------------|
| 1.0 | x ≥ 1.0 | Minimum version, inclusive |
| [1.0,) | x ≥ 1.0 | Minimum version, inclusive |
| (1.0,) | x > 1.0 | Minimum version, exclusive |
| [1.0] | x == 1.0 | Exact version match |
| (,1.0] | x ≤ 1.0 | Maximum version, inclusive |
| (,1.0) | x < 1.0 | Maximum version, exclusive |
| [1.0,2.0] | 1.0 ≤ x ≤ 2.0 | Exact range, inclusive |
| (1.0,2.0) | 1.0 < x < 2.0 | Exact range, exclusive |
| [1.0,2.0) | 1.0 ≤ x < 2.0 | Mixed inclusive minimum and exclusive maximum version |
| (1.0)    | invalid | invalid |

### Examples

Always specify a version or version range for package dependencies in project files, `packages.config` files, and `.nuspec` files. Without a version or version range, NuGet 2.8.x and earlier chooses the latest available package version when resolving a dependency, whereas NuGet 3.x and later chooses the lowest package version. Specifying a version or version range avoids this uncertainty.

#### References in project files (PackageReference)

```xml
<!-- Accepts any version 6.1 and above.
     Will resolve to the smallest acceptable stable version.-->
<PackageReference Include="ExamplePackage" Version="6.1" />

<!-- Accepts any 6.x.y version.
     Will resolve to the highest acceptable stable version.-->
<PackageReference Include="ExamplePackage" Version="6.*" />

<!-- Accepts any version above, but not including 4.1.3. Could be
     used to guarantee a dependency with a specific bug fix. 
     Will resolve to the smallest acceptable stable version.-->
<PackageReference Include="ExamplePackage" Version="(4.1.3,)" />

<!-- Accepts any version up below 5.x, which might be used to prevent pulling in a later
     version of a dependency that changed its interface. However, this form is not
     recommended because it can be difficult to determine the lowest version. 
     Will resolve to the smallest acceptable stable version.
     -->
<PackageReference Include="ExamplePackage" Version="(,5.0)" />

<!-- Accepts any 1.x or 2.x version, but not 0.x or 3.x and higher.
     Will resolve to the smallest acceptable stable version.-->
<PackageReference Include="ExamplePackage" Version="[1,3)" />

<!-- Accepts 1.3.2 up to 1.4.x, but not 1.5 and higher.
     Will resolve to the smallest acceptable stable version. -->
<PackageReference Include="ExamplePackage" Version="[1.3.2,1.5)" />
```

**References in `packages.config`:**

In `packages.config`, every dependency is listed with an exact `version` attribute that's used when restoring packages. The `allowedVersions` attribute is used only during update operations to constrain the versions to which the package might be updated.

```xml
<!-- Install/restore version 6.1.0, accept any version 6.1.0 and above on update. -->
<package id="ExamplePackage" version="6.1.0" allowedVersions="6.1.0" />

<!-- Install/restore version 6.1.0, and do not change during update. -->
<package id="ExamplePackage" version="6.1.0" allowedVersions="[6.1.0]" />

<!-- Install/restore version 6.1.0, accept any 6.x version during update. -->
<package id="ExamplePackage" version="6.1.0" allowedVersions="[6,7)" />

<!-- Install/restore version 4.1.4, accept any version above, but not including, 4.1.3.
     Could be used to guarantee a dependency with a specific bug fix. -->
<package id="ExamplePackage" version="4.1.4" allowedVersions="(4.1.3,)" />

<!-- Install/restore version 3.1.2, accept any version up below 5.x on update, which might be
     used to prevent pulling in a later version of a dependency that changed its interface.
     However, this form is not recommended because it can be difficult to determine the lowest version. -->
<package id="ExamplePackage" version="3.1.2" allowedVersions="(,5.0)" />

<!-- Install/restore version 1.1.4, accept any 1.x or 2.x version on update, but not
     0.x or 3.x and higher. -->
<package id="ExamplePackage" version="1.1.4" allowedVersions="[1,3)" />

<!-- Install/restore version 1.3.5, accepts 1.3.2 up to 1.4.x on update, but not 1.5 and higher. -->
<package id="ExamplePackage" version="1.3.5" allowedVersions="[1.3.2,1.5)" />
```

**References in `.nuspec` files**

The `version` attribute in a `<dependency>` element describes the range versions that are acceptable for a dependency.

```xml
<!-- Accepts any version 6.1 and above. -->
<dependency id="ExamplePackage" version="6.1" />

<!-- Accepts any version above, but not including 4.1.3. Could be
     used to guarantee a dependency with a specific bug fix. -->
<dependency id="ExamplePackage" version="(4.1.3,)" />

<!-- Accepts any version up below 5.x, which might be used to prevent pulling in a later
     version of a dependency that changed its interface. However, this form is not
     recommended because it can be difficult to determine the lowest version. -->
<dependency id="ExamplePackage" version="(,5.0)" />

<!-- Accepts any 1.x or 2.x version, but not 0.x or 3.x and higher. -->
<dependency id="ExamplePackage" version="[1,3)" />

<!-- Accepts 1.3.2 up to 1.4.x, but not 1.5 and higher. -->
<dependency id="ExamplePackage" version="[1.3.2,1.5)" />
```

## Normalized version numbers

> [!Note]
> This is a breaking change for NuGet 3.4+.

When obtaining packages from a repository during install, reinstall, or restore operations, NuGet 3.4+ treats version numbers as follows:

- Leading zeroes are removed from version numbers:
  - 1.00 is treated as 1.0
  - 1.01.1 is treated as 1.1.1
  - 1.00.0.1 is treated as 1.0.0.1

- A zero in the fourth part of the version number will be omitted
  - 1.0.0.0 is treated as 1.0.0
  - 1.0.01.0 is treated as 1.0.1

- SemVer 2.0.0 build metadata is removed
  - 1.0.7+r3456 is treated as 1.0.7

`pack` and `restore` operations normalize versions whenever possible. For packages already built, this normalization does not affect the version numbers in the packages themselves; it affects only how NuGet matches versions when resolving dependencies.

However, NuGet package repositories must treat these values in the same way as NuGet to prevent package version duplication. Thus a repository that contains version *1.0* of a package should not also host version *1.0.0* as a separate and different package.

## Semantic Versioning 2.0.0

Certain semantics of SemVer v2.0.0 are not supported in older clients.
NuGet considers a package version to be SemVer v2.0.0 specific if either of the following statements is true:

- The pre-release label is dot-separated, for example, *1.0.0-alpha.1*
- The version has build-metadata, for example, *1.0.0+githash*

For nuget.org, a package is defined as a SemVer v2.0.0 package if either of the following statements is true:

- The package's own version is SemVer v2.0.0 compliant but not SemVer v1.0.0 compliant, as defined above.
- Any of the package's dependency version ranges has a minimum or maximum version that is SemVer v2.0.0 compliant but not SemVer v1.0.0 compliant, defined above; for example, *[1.0.0-alpha.1, )*.

If you upload a SemVer v2.0.0-specific package to nuget.org, the package is invisible to older clients and available to only the following NuGet clients:

- NuGet 4.3.0+
- Visual Studio 2017 version 15.3+
- Visual Studio 2015 with [NuGet VSIX v3.6.0](https://dist.nuget.org/visualstudio-2015-vsix/latest/NuGet.Tools.vsix)
- .NET SDK 2.0.0+

Third-party clients:

- JetBrains Rider
- Paket version 5.0+

<!-- For compatibility with previous dependency-versions page -->
<a name="version-ranges"></a>

## Where NuGetVersion diverges from Semantic Versioning

If you want to programatically use NuGet package versions, it is strongly recommended to use [the package NuGet.Versioning](https://www.nuget.org/packages/NuGet.Versioning). The static method `NuGetVersion.Parse(string)` can be used to parse the version strings, and `VersionComparer` can be used to sort `NuGetVersion` instances.

If you are implementing NuGet functionality in a language that does not run on .NET, here are the known list of differences between `NuGetVersion` and Semantic Versioning, and the reasons why an existing Semantic Versioning library might not work for packages already published on nuget.org.

1. `NuGetVersion` supports a 4th version segment, `Revision`, to be compatible with, or a superset of, [`System.Version`](/dotnet/api/system.version). Therefore, excluding prerelease and metadata labels, a version string is `Major.Minor.Patch.Revision`. As per version normalization described above, if `Revision` is zero, it is omitted from the normalized version string.
2. `NuGetVersion` only requires the major segment to be defined. All others are optional, and are equivalent to zero. This means that `1`, `1.0`, `1.0.0`, and `1.0.0.0` are all accepted and equal.
3. `NuGetVersion` uses case insensitive string comparisons for pre-release components. This means that `1.0.0-alpha` and `1.0.0-Alpha` are equal.
