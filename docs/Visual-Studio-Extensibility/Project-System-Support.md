---
# required metadata

title: NuGet Support for the Visual Studio Project System | Microsoft Docs
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 1/9/2017
ms.topic: article
ms.prod: nuget
#ms.service:
ms.technology: nuget
ms.assetid: 9d7fa7f6-82ed-4df6-9734-f43a3d8e3b98

# optional metadata

#description:
#keywords:
#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer:
- karann
- unnir
#ms.suite:
#ms.tgt_pltfrm:
#ms.custom:

---

# NuGet support for the Visual Studio project system

To support third-party project types in Visual Studio, NuGet 3.x+ supports the [Common Project System (CPS)](https://github.com/Microsoft/VSProjectSystem/blob/master/doc/overview/intro.md), and NuGet 3.2+ supports non-CPS project systems as well.

To integrate with NuGet, a project system must advertise its own support for all the project capabilities described in this topic.


> [!NOTE]
> Do not declare capabilities that your project does not actually have for the sake of enabling packages to install in your project. Many features of Visual Studio and other extensions depend on project capabilities besides the NuGet client. Falsely advertising capabilities of your project can lead these components to malfunction and your users' experience to degrade.

## Advertise project capabilities

The NuGet client determines which packages are compatible with your project type based on the [project's capabilities](https://github.com/Microsoft/VSProjectSystem/blob/master/doc/overview/about_project_capabilities.md), as described in the following table.


|Capability|Description|
|----------------|-----------|
|AssemblyReferences|Indicates that the project supports assembly references (distinct from WinRTReferences)|
|DeclaredSourceItems|Indicates that the project is a typical MSBuild project (not DNX) in that it declares source items in the project itself (rather than a project.json file that assumes all files in the directory are part of a compilation).|
|UserSourceItems|Indicates that the user is allowed to add arbitrary files to their project.|

For CPS-based project systems, the implementation details for project capabilities described in the rest of this section have been done for you. See [declaring project capabilities in CPS projects](https://github.com/Microsoft/VSProjectSystem/blob/master/doc/overview/about_project_capabilities.md#how-to-declare-project-capabilities-in-your-project).

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

Remember to add/remove capabilities from the `ActualProjectCapabilities` set based on what your project system actually supports. See the [project capabilities documentation](https://github.com/Microsoft/VSProjectSystem/blob/master/doc/overview/project_capabilities.md) for full descriptions.

## Responding to queries

A project declares this capability by supporting the  `VSHPROPID_ProjectCapabilitiesChecker` property through the `IVsHierarchy::GetProperty`. It should return an instance of
`Microsoft.VisualStudio.Shell.Interop.IVsBooleanSymbolPresenceChecker`, which is defined in the `Microsoft.VisualStudio.Shell.Interop.14.0.DesignTime.dll` assembly. Reference this assembly by installing the [its NuGet package](https://www.nuget.org/packages/Microsoft.VisualStudio.Shell.Interop.14.0.DesignTime).

For example, you might add the following `case` statement to your `IVsHierarchy::GetProperty` method's `switch` statement:

    case __VSHPROPID8.VSHPROPID_ProjectCapabilitiesChecker:
        propVal = new VsProjectCapabilitiesPresenceChecker();
        return VSConstants.S_OK;


## DTE Support

NuGet drives the project system to add references, content items, and MSBuild imports by calling into [DTE](https://msdn.microsoft.com/library/mt452175.aspx), which is the top-level Visual Studio automation interface. DTE is a set of COM interfaces that you may already implement.

If your project type is based on CPS, DTE is implemented for you.
