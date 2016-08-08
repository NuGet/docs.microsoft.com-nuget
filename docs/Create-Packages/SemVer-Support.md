# SemVer Support

NuGet supports the creation of pre-release packages by specifying a pre-release string in 
the version number according to the [Semantic Versioning (SemVer) specification v1.0.0](http://semver.org/spec/v1.0.0.html).

## Overview of SemVer
The [SemVer spec](http://semver.org/spec/v1.0.0.html) gives a detailed understanding of Semantic Versioning, and how to utilize version numbers to convey they meaning of the underlying code.

SemVer is a convention for versioning your public APIs in which the version number has meaning attached to it. 
Each version has three parts: `Major.Minor.Patch`.

Each part corresponds to:

* `Major`: Breaking changes.
* `Minor`: New features, but backwards compatible.
* `Patch`: Backwards compatible bug fixes only.

Additionally, pre-release versions of your API can be denoted by appending an arbitrary string to the 
_Patch_ number separated by a dash. For example:

<code class="bash hljs">
1.0.1-alpha <br>
1.0.1-beta <br>
1.0.1-rc <br>
</code>

<div class="block-callout-info">
    <strong>Note:</strong><br>
    The actual string applied doesn't matter. If there's a string there, it's a pre-release version.
</div>

## Pre-release Versions
When you’re ready to release, just remove the dash and the string and that version is considered “higher” 
than all the pre-release versions. For example, the stable version `1.0.1` is larger than `1.0.1-rc`

The pre-release versions are given precedence in alphabetical order (well technically lexicographic 
ASCII sort order).Therefore, the following is an example from lowest to highest versions of a package.

<code class="bash hljs">
1.0.1-alpha <br>
1.0.1-alpha2 <br>
1.0.1-beta <br>
1.0.1-rc <br>
1.0.1-zzz <br>
1.0.1 <br>
</code>


## CI Builds
SemVer also introduces the concept of a build number for those creating daily or continuous builds. **This is not 
supported** in the public NuGet.org gallery.

While this is allowed:
<code class="bash hljs">
1.0.1-build23
</code>

Including the SemVer-compatible build number with dot notation is not allowed:
<code class="bash hljs">
1.0.1-build.23
</code>
