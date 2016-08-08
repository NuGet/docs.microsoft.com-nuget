# NuGet support for Visual Studio project system

This document outlines the steps you should take in your own project system in Visual Studio so that NuGet packages can be installed in your projects using the NuGet client extension for Visual Studio.

## Support

NuGet v3 is the first version to support 3rd party project types, by supporting CPS-based projects. NuGet v3.2 adds support for non-CPS project systems as well.

In NuGet v3.0, the NuGet client will only integrate with project systems that support everything a NuGet package might do. Or in other words, a project system must advertise support for all the project capabilities
described below for NuGet functionality to "light up".

<div class="block-callout-info">
    <strong>Note:</strong><br>
    You should <strong>not</strong> declare capabilities that your project does not actually have for the sake of enabling packages to install in your project. Many features of Visual Studio and other extensions depend on project capabilities besides the NuGet client and falsely advertising capabilities of your project can lead these components to malfunction and your users' experience to degrade.   
</div>


## Advertise project capabilities

Your project system must advertise support for some key capabilities it has so that the NuGet client can determine which packages are compatible with your projects. Below is a table of the project capabilities the NuGet client
may look for when testing for compatibility.

<table class="reference">
<thead>
<tr>
<th>Project Capability</th><th>Description</th>
</tr>
<tr>
<td>AssemblyReferences</td><td>Indicates that the project supports assembly references (distinct from WinRTReferences)</td>
</tr>
<tr>
<td>DeclaredSourceItems</td><td>Indicates that the project is a typical MSBuild project (not DNX) in that it declares source items in the project itself (rather than a project.json file that assumes all files in the directory are part of a compilation)</td>
</tr>
<tr>
<td>UserSourceItems</td><td>Indicates that the user is allowed to add arbitrary files to their project.</td>
</tr>
</table>


[Learn more about project capabilities][1].

Note that for [CPS-based project systems][4], the implementation details for project capabilities described in the rest of this section have been done for you. Learn more about [declaring project capabilities in CPS projects][5].

## Responding to queries

Your project declares these capabilities by supporting the `IVsHierarchy::GetProperty` VSHPROPID_ProjectCapabilitiesChecker property. You should return an instance of the
`Microsoft.VisualStudio.Shell.Interop.IVsBooleanSymbolPresenceChecker`, which is defined in the `Microsoft.VisualStudio.Shell.Interop.14.0.DesignTime.dll` assembly. You can reference this assembly by installing the [NuGet package][2] for it.

### Example

You might add the following `case` statement to your `IVsHierarchy::GetProperty` method's `switch` statement:

    case __VSHPROPID8.VSHPROPID_ProjectCapabilitiesChecker:
        propVal = new VsProjectCapabilitiesPresenceChecker();
        return VSConstants.S_OK;

We will define the `VsProjectCapabilitiesPresenceChecker` class in the next step.

## Implementing VsProjectCapabilitiesPresenceChecker

The `VsProjectCapabilitiesPresenceChecker` class must implement the `IVsBooleanSymbolPresenceChecker` interface, which is defined as follows:


    public interface IVsBooleanSymbolPresenceChecker
    {
        /// <summary>
        /// Checks whether the symbols defined may have changed since the last time
        /// this method was called.
        /// </summary>
        /// <param name="versionObject">
        /// The response version object assigned at the last call.
        /// May be null to get the initial version.
        /// At the conclusion of this method call, the reference may be changed so that on a subsequent call
        /// we know what version was last observed. The caller should treat this value as an opaque object,
        /// and should not assume any significance from whether the pointer changed or not.
        /// </param>
        /// <returns>
        /// <c>true</c> if the symbols defined may have changed since the last call to this method
        /// or if <paramref name="versionObject"/> was <c>null</c> upon entering this method.
        /// <c>false</c> if the responses would all be the same.
        /// </returns>
        bool HasChangedSince(ref object versionObject);

        /// <summary>
        /// Checks for the presence of a given symbol.
        /// </summary>
        /// <param name="symbol">The symbol to check for.</param>
        /// <returns><c>true</c> if the symbol is present; <c>false</c> otherwise.</returns>
        bool IsSymbolPresent(string symbol);
    }


A sample implementation of this interface would then be:


    class VsProjectCapabilitiesPresenceChecker : IVsBooleanSymbolPresenceChecker
    {
        /// <summary>
        /// The set of capabilities this project system actually has.
        /// This should be kept current, and honestly reflect what the project can do.
        /// </summary>
        private static readonly HashSet<string> ActualProjectCapabilities = new HashSet<string>(StringComparer.OrdinalIgnoreCase)
            {
                "AssemblyReferences",
                "DeclaredSourceItems",
                "UserSourceItems",
            };

        public bool HasChangedSince(ref object versionObject)
        {
            // If your project capabilities do not change over time while the project is open,
            // you may simply `return false;` from your `HasChangedSince` method.
            return false;
        }

        public bool IsSymbolPresent(string symbol)
        {
            return ActualProjectCapabilities.Contains(symbol);
        }
    }


Remember to add/remove capabilities from the `ActualProjectCapabilities` set based on what your project system actually supports. All defined [project capabilities are documented with their descriptions][3] for
your reference.

## DTE Support

NuGet drives the project system to add references, content items, and MSBuild imports by calling into DTE, which is a set of COM interfaces that you may already implement.

If your project type is [based on CPS][4], DTE is already implemented for you.

[1]: https://github.com/Microsoft/VSProjectSystem/blob/master/doc/overview/about_project_capabilities.md
[2]: https://www.nuget.org/packages/Microsoft.VisualStudio.Shell.Interop.14.0.DesignTime
[3]: https://github.com/Microsoft/VSProjectSystem/blob/master/doc/overview/project_capabilities.md
[4]: https://github.com/Microsoft/VSProjectSystem/blob/master/doc/overview/intro.md
[5]: https://github.com/Microsoft/VSProjectSystem/blob/master/doc/overview/about_project_capabilities.md#how-to-declare-project-capabilities-in-your-project
