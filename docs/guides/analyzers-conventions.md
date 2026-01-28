---
title: .NET Compiler Platform Analyzer Formats for NuGet
description: Conventions for .NET analyzers that are packaged and distributed with NuGet packages that implement an API or library.
author: JonDouglas
ms.author: jodou
ms.date: 01/09/2017
ms.topic: concept-article
---

# Analyzer NuGet formats

The .NET Compiler Platform (also known as "Roslyn") allows developers to create [analyzers](https://github.com/dotnet/roslyn/blob/main/docs/wiki/How-To-Write-a-C%23-Analyzer-and-Code-Fix.md) that examine the syntax tree and semantics of code as it's being written. This provides developers with a way to create domain-specific analysis tools, such as those that would help guide the use of a particular API or library. You can find more information on the [.NET/Roslyn](https://github.com/dotnet/roslyn/wiki) GitHub wiki. Also see the article, [Use Roslyn to Write a Live Code Analyzer for your API](/archive/msdn-magazine/2014/special-issue/csharp-and-visual-basic-use-roslyn-to-write-a-live-code-analyzer-for-your-api) in MSDN Magazine.

Analyzers themselves are typically packaged and distributed as part of the NuGet packages that implement the API or library in question.

For a good example, see the [System.Runtime.Analyzers](https://www.nuget.org/packages/System.Runtime.Analyzers) package, which has the following contents:

- analyzers\dotnet\System.Runtime.Analyzers.dll
- analyzers\dotnet\cs\System.Runtime.CSharp.Analyzers.dll
- analyzers\dotnet\vb\System.Runtime.VisualBasic.Analyzers.dll
- build\System.Runtime.Analyzers.Common.props
- build\System.Runtime.Analyzers.props
- build\System.Runtime.CSharp.Analyzers.props
- build\System.Runtime.VisualBasic.Analyzers.props
- tools\install.ps1
- tools\uninstall.ps1

As you can see, you place the analyzer DLLs into an `analyzers` folder in the package.

Props files, which are included to disable legacy FxCop rules in favor of the analyzer implementation, are placed in the `build` folder.

Install and uninstall scripts that support projects using `packages.config` are placed in `tools`.

Also note that because this package has no platform-specific requirements, the `platform` folder is omitted.


## Analyzers path format

The use of the `analyzers` folder is similar to that used for [target frameworks](../create-packages/supporting-multiple-target-frameworks.md), except the specifiers in the path describe development host dependencies instead of build-time. The general format is as follows:

```
$/analyzers/{framework_name}{version}/{supported_architecture}/{supported_language}/{analyzer_name}.dll
```

- **framework_name** and **version**: the *optional* API surface area of the .NET Framework that the contained DLLs need to run. `dotnet` is presently the only valid value because Roslyn is the only host that can run analyzers. If no target is specified, DLLs are assumed to apply to *all* targets.
- **supported_language**: the language for which the DLL applies, one of `cs` (C#) and `vb` (Visual Basic), and `fs` (F#). The language indicates that the analyzer should be loaded only for a project using that language. If no language is specified then the DLL is assumed to apply to *all* languages that support analyzers.
- **analyzer_name**: specifies the DLLs of the analyzer. If you need additional files beyond DLLs, they must be included through a targets or properties files.


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
