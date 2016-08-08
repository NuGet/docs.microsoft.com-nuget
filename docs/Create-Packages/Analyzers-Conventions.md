# Analyzer Nuget Format For Project.json

The proposed format should be identical to the format described [here](https://docs.nuget.org/Create/Enforced-Package-Conventions), except the specifiers in the path describe development host dependencies instead of build-time.

$/analyzers/{**Framework Name**}{**Version**}/{**Supported Architecture**}/**{Supported Programming Language**}/{**Analyzer**}.dll

**Framework Name and Version** the *optional* API surface area of the .NET framework that the contained dlls need to run. If no target is specified, dlls are assumed to apply to *all* targets

example:

- dotnet
- net45
- wp10
- win10
- win8

**Supported Programming Language** the language specific dlls that should be loaded only when the project language and the dll language match. If no language is specified then dll is assumed to apply to *all* languages that support analyzers.  

valid values:

- cs -> represents C#
- vb -> represents Visual Basic
- fs -> represents F#

If it is ambiguous whether you are referring to a programming language of a framework, it will be assumed that you are referring to a programming language.

**Analyzer** The analyzer or analyzer dependency dll.  If the analyzer requires additional files beyond dlls their inclusion will need to be explained in a targets or properties files.

## Example Project.json Analyzer Packages
Because [System.Runtime.Analyzers](https://www.nuget.org/packages/System.Runtime.Analyzers) has no platform specific requirements the platform folder is omitted. Properties files are included to disable legacy FxCop rules in favor of the analyzer implementation. 

- analyzers\dotnet\System.Runtime.Analyzers.dll 
- analyzers\dotnet\cs\System.Runtime.CSharp.Analyzers.dll 
- analyzers\dotnet\vb\System.Runtime.VisualBasic.Analyzers.dll
- build\System.Runtime.Analyzers.Common.props
- build\System.Runtime.Analyzers.props
- build\System.Runtime.CSharp.Analyzers.props
- build\System.Runtime.VisualBasic.Analyzers.props

## Notes on Using Target Framework Other Than dotnet

At this time there is no host other than Roslyn compiler that can run analyzers.  Therefore, **Framework Name and Version** should always be specified as 'dotnet' until another host is implemented that has runtime restrictions.

## Analyzer Nuget Format For Packages.config
if the user's project is using package.config, the msbuild script that picks up the analyzer does not come into play, and you will want to add the following scripts under tools: **install.ps1** and **uninstall.ps1**.  

NOTE:  **install.ps1** and **uninstall.ps1** are only executed for packages.config scenarios.  In the case of Project.json these scripts are never executed.

**install.ps1 file contents**
```PowerShell

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

$project.Type gives the language name like (C# or VB.NET)
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


**uninstall.ps1 file contents**
```PowerShell

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

    $project.Type gives the language name like (C# or VB.NET)
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

## Example Analyzer Package for Both Project.json and Packages.config##


    [System.Runtime.Analyzers](https://www.nuget.org/packages/System.Runtime.Analyzers) ###
    - analyzers\dotnet\System.Runtime.Analyzers.dll 
    - analyzers\dotnet\cs\System.Runtime.CSharp.Analyzers.dll 
    - analyzers\dotnet\vb\System.Runtime.VisualBasic.Analyzers.dll
    - build\System.Runtime.Analyzers.Common.props
    - build\System.Runtime.Analyzers.props
    - build\System.Runtime.CSharp.Analyzers.props
    - build\System.Runtime.VisualBasic.Analyzers.props
    - tools\install.ps1
    - tools\uninstall.ps1
