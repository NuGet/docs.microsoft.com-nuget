---
title: .NET Compiler Platform Analyzer Formats for NuGet
description: Conventions for .NET analyzers that are packaged and distributed with NuGet packages that implement an API or library.
author: JonDouglas
ms.author: jodou
ms.date: 09/18/2026
ms.topic: concept-article
ai-usage: ai-assisted
---

# Analyzer NuGet formats

The .NET Compiler Platform (also known as "Roslyn") allows developers to create [analyzers](https://github.com/dotnet/roslyn/blob/main/docs/wiki/How-To-Write-a-C%23-Analyzer-and-Code-Fix.md) that examine the syntax tree and semantics of code as it's being written. This provides developers with a way to create domain-specific analysis tools, such as those that would help guide the use of a particular API or library. You can find more information on the [.NET/Roslyn](https://github.com/dotnet/roslyn/wiki) GitHub wiki. Also see the article, [Use Roslyn to Write a Live Code Analyzer for your API](/archive/msdn-magazine/2014/special-issue/csharp-and-visual-basic-use-roslyn-to-write-a-live-code-analyzer-for-your-api) in MSDN Magazine.

Analyzers themselves are typically packaged and distributed as part of the NuGet packages that implement the API or library in question.

For a good example, see the [CommunityToolkit.Mvvm 8.4.2](https://www.nuget.org/packages/CommunityToolkit.Mvvm/8.4.2) package, which includes the following analyzer and build assets:

- analyzers\dotnet\roslyn4.0\cs\CommunityToolkit.Mvvm.SourceGenerators.dll
- analyzers\dotnet\roslyn4.3\cs\CommunityToolkit.Mvvm.SourceGenerators.dll
- analyzers\dotnet\roslyn4.12\cs\CommunityToolkit.Mvvm.SourceGenerators.dll
- analyzers\dotnet\roslyn5.0\cs\CommunityToolkit.Mvvm.SourceGenerators.dll
- analyzers\dotnet\roslyn4.0\cs\CommunityToolkit.Mvvm.CodeFixers.dll
- analyzers\dotnet\roslyn4.3\cs\CommunityToolkit.Mvvm.CodeFixers.dll
- analyzers\dotnet\roslyn4.12\cs\CommunityToolkit.Mvvm.CodeFixers.dll
- analyzers\dotnet\roslyn5.0\cs\CommunityToolkit.Mvvm.CodeFixers.dll
- buildTransitive\CommunityToolkit.Mvvm.SourceGenerators.targets

The analyzer DLLs are placed in the `analyzers` folder, with separate variants for supported Roslyn compiler API versions.
The targets file in `buildTransitive` provides supporting build logic to the project and to projects that reference it.


## Analyzers path format

The use of the `analyzers` folder is similar to that used for [target frameworks](../create-packages/supporting-multiple-target-frameworks.md), except the specifiers in the path describe development host dependencies instead of build-time. Analyzer assemblies are `.dll` files anywhere under the `analyzers` folder, except satellite `.resources.dll` assemblies. The general format is as follows:

```
$/analyzers/{framework_name}/{compiler_api_version}/{supported_language}/{analyzer_name}.dll
```

The path segments are optional. The language and compiler API version segments can appear at any depth.

- **framework_name**: the development host that loads the analyzer. `dotnet` is presently the only valid value because Roslyn is the only supported host. If no host is specified, the DLL is assumed to apply to all hosts.
- **compiler_api_version**: the minimum supported compiler API version, in the form `roslynX.Y`, such as `roslyn4.0`. When a package contains multiple versioned analyzer variants, the .NET SDK selects the highest version that is not greater than the compiler version in use. If no compiler API version is specified, the analyzer applies to all compiler versions.
- **supported_language**: the language for which the DLL applies, one of `cs` (C#), `vb` (Visual Basic), and `fs` (F#). The analyzer is loaded only for projects using that language. If no language is specified, the DLL is assumed to apply to all languages that support analyzers.
- **analyzer_name**: specifies the analyzer DLL. If you need additional files beyond DLLs, they must be included through targets or properties files.

For example:

- `analyzers/dotnet/MyAnalyzer.dll`: analyzer for all supported languages and compiler versions.
- `analyzers/dotnet/cs/MyAnalyzer.dll`: C# analyzer for all compiler versions.
- `analyzers/dotnet/roslyn4.0/cs/MyAnalyzer.dll`: C# analyzer for Roslyn 4.0 and later.


## Install and uninstall scripts

If the user's project is using `packages.config`, the MSBuild script that picks up the analyzer does not come into play, so you should place `install.ps1` and `uninstall.ps1` in the `tools` folder with the contents that are described below.

**install.ps1 file contents**

```ps
param($installPath, $toolsPath, $package, $project)

$analyzersPaths = Join-Path (Join-Path (Split-Path -Path $toolsPath -Parent) "analyzers" ) * -Resolve

foreach($analyzersPath in $analyzersPaths)
{
    # Install the language agnostic analyzers.
    if (Test-Path $analyzersPath)
    {
        foreach ($analyzerFilePath in Get-ChildItem $analyzersPath -Filter *.dll)
        {
            if($project.Object.AnalyzerReferences)
            {
                $project.Object.AnalyzerReferences.Add($analyzerFilePath.FullName)
            }
        }
    }
}

$project.Type # gives the language name like (C# or VB.NET)
$languageFolder = ""
if($project.Type -eq "C#")
{
    $languageFolder = "cs"
}
if($project.Type -eq "VB.NET")
{
    $languageFolder = "vb"
}
if($languageFolder -eq "")
{
    return
}

foreach($analyzersPath in $analyzersPaths)
{
    # Install language specific analyzers.
    $languageAnalyzersPath = join-path $analyzersPath $languageFolder
    if (Test-Path $languageAnalyzersPath)
    {
        foreach ($analyzerFilePath in Get-ChildItem $languageAnalyzersPath -Filter *.dll)
        {
            if($project.Object.AnalyzerReferences)
            {
                $project.Object.AnalyzerReferences.Add($analyzerFilePath.FullName)
            }
        }
    }
}
```


**uninstall.ps1 file contents**

```ps
param($installPath, $toolsPath, $package, $project)

$analyzersPaths = Join-Path (Join-Path (Split-Path -Path $toolsPath -Parent) "analyzers" ) * -Resolve

foreach($analyzersPath in $analyzersPaths)
{
    # Uninstall the language agnostic analyzers.
    if (Test-Path $analyzersPath)
    {
        foreach ($analyzerFilePath in Get-ChildItem $analyzersPath -Filter *.dll)
        {
            if($project.Object.AnalyzerReferences)
            {
                $project.Object.AnalyzerReferences.Remove($analyzerFilePath.FullName)
            }
        }
    }
}

$project.Type # gives the language name like (C# or VB.NET)
$languageFolder = ""
if($project.Type -eq "C#")
{
    $languageFolder = "cs"
}
if($project.Type -eq "VB.NET")
{
    $languageFolder = "vb"
}
if($languageFolder -eq "")
{
    return
}

foreach($analyzersPath in $analyzersPaths)
{
    # Uninstall language specific analyzers.
    $languageAnalyzersPath = join-path $analyzersPath $languageFolder
    if (Test-Path $languageAnalyzersPath)
    {
        foreach ($analyzerFilePath in Get-ChildItem $languageAnalyzersPath -Filter *.dll)
        {
            if($project.Object.AnalyzerReferences)
            {
                try
                {
                    $project.Object.AnalyzerReferences.Remove($analyzerFilePath.FullName)
                }
                catch
                {

                }
            }
        }
    }
}
```
