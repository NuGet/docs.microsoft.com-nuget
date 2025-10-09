---
title: NuGet API in Visual Studio
description: Interface reference for the API that NuGet exports through the Managed Extensibility Framework in Visual Studio
author: nkolev92
ms.author: nikolev
ms.date: 01/09/2017
ms.topic: reference
---

# NuGet API in Visual Studio

In addition to the Package Manager UI and Console in Visual Studio, NuGet also exports some useful services that other extensions can use. These interfaces allow other components in Visual Studio to interact with NuGet, which can be used to install and uninstall packages, and to obtain information about installed packages.

NuGet provides services via two different technologies, each of which have their interfaces defined in a different NuGet package. NuGet's older services are available via [the Managed Extensibility Framework (MEF)](/dotnet/framework/mef/), which are available in the package [NuGet.VisualStudio](https://www.nuget.org/packages/NuGet.VisualStudio) ([go to NuGet's MEF services](#mef-services)). There are newer APIs, designed to be usable with `async` code, available in the package `NuGet.VisualStudio.Contracts`, using a Visual Studio's `IServiceBroker` ([go to NuGet's Brokered Services](#brokered-services)).

## Package Versions

NuGet's product follows Visual Studio's version, but is 11.0 versions behind. For example, NuGet 6.0 corresponds to Visual Studio 2022 17.0, NuGet 5.11 corresponds to Visual Studio 2019 16.11, and so on.

Starting from Visual Studio 17.1, NuGet's Visual Studio extensibility API packages match the version of Visual Studio that the APIs are targeting. For example, NuGet.VisualStudio and NuGet.VisualStudio.Contracts package version 17.1.0 should be used when your extension targets Visual Studio 17.1 and higher. In Visual Studio 17.0 and earlier, NuGet's package versions are the same as NuGet's product version. For example, if your extension targets Visual Studio 2022 version 17.0, you should use version 6.0 of NuGet's Visual Studio extensibility packages.

## NuGet Client SDK in Visual Studio Extensions

Only the APIs in `NuGet.VisualStudio` and `NuGet.VisualStudio.Contracts` are supported in Visual Studio extensions.
NuGet provides binding redirects for these assemblies, so these assemblies do not need to be included in your extension.

Using NuGet Client SDK packages, for example `NuGet.Protocol`, is not supported in Visual Studio extensions.
NuGet does not provide binding redirects for these assemblies.
See [the NuGet Client SDK support policy](../reference/NuGet-Client-SDK.md#support-policy) for more information.

## Services List

### Brokered Services

These services are available in the package [NuGet.VisualStudio.Contracts](https://nuget.org/packages/NuGet.VisualStudio.Contracts/).

- [`INuGetProjectService`](#inugetprojectservice-interface): Methods to interact with a project. (5.7+)

### MEF Services

From NuGet 6.0, all of these APIs are available in the package [NuGet.VisualStudio](https://www.nuget.org/packages/NuGet.VisualStudio/). In NuGet 5.11 and earlier, the APIs in the namespace `NuGet.VisualStudio` are available in the package [NuGet.VisualStudio](https://www.nuget.org/packages/NuGet.VisualStudio/), and APIs in the namespace `NuGet.SolutionRestoreManager` are available in the package [NuGet.SolutionRestoreManager.Interop](https://www.nuget.org/packages/NuGet.SolutionRestoreManager.Interop/).

#### NuGet.VisualStudio

- [`IRegistryKey`](#iregistrykey-interface): Method to retrieve a value from a registry subkey. (3.3+)
- [`IVsCredentialProvider`](#ivscredentialprovider-interface) Contains methods to get credentials for NuGet operations. (4.0+)
- [`IVsFrameworkCompatibility`](#ivsframeworkcompatibility-interface) Contains methods to discover frameworks and compatibility between frameworks. (4.0+)
- [`IVsFrameworkCompatibility2`](#ivsframeworkcompatibility2-interface) Contains methods to discover frameworks and compatibility between frameworks. (4.0+)
- [`IVsFrameworkCompatibility3`](#ivsframeworkcompatibility3-interface) Contains methods to discover frameworks and compatibility between frameworks. (5.8+)
- [`IVsFrameworkParser`](#ivsframeworkparser-interface) An interface for dealing with the conversion between strings and [FrameworkName](/dotnet/api/system.runtime.versioning.frameworkname) (4.0+)
- [`IVsFrameworkParser2`](#ivsframeworkparser2-interface) An interface to parse .NET Framework strings. See [NuGet-IVsFrameworkParser](https://aka.ms/NuGet-IVsFrameworkParser). (5.8+)
- [`IVsPackageInstaller`](#ivspackageinstaller-interface): Methods to install NuGet packages into projects. (3.3+)
- [`IVsPackageInstaller2](#ivspackageinstaller2-interface) Contains method to install latest version of a single package into a project within the current solution.
- [`IVsPackageInstallerEvents`](#ivspackageinstallerevents-interface): Events for package install/uninstall. (3.3+)
- [`IVsPackageInstallerProjectEvents`](#ivspackageinstallerprojectevents-interface): Batch events for package install/uninstall. (3.3+)
- [`IVsPackageInstallerServices`](#ivspackageinstallerservices-interface): Methods to retrieve installed packages in the current solution and to check whether a given package is installed in a project. (3.3+)
- [`IVsPackageRestorer`](#ivspackagerestorer-interface): Methods to restore packages installed in a project. (3.3+)
- [`IVsPackageSourceProvider`](#ivspackagesourceprovider-interface): Methods to retrieve a list of NuGet package sources. (3.3+)
- [`IVsPackageUninstaller`](#ivspackageuninstaller-interface): Methods to uninstall NuGet packages from projects. (3.3+)
- [`IVsPathContext`](#ivspathcontext-interface) NuGet path information specific to the current context (e.g. project context). (4.0+)
- [`IVsPathContext2`](#ivspathcontext2-interface) NuGet path information specific to the current context (e.g. project context). (5.0+)
- [`IVsPathContextProvider`](#ivspathcontextprovider-interface) A factory to initialize [IVsPathContext](#ivspathcontext-interface) instances. (4.0+)
- [`IVsPathContextProvider2`](#ivspathcontextprovider2-interface) A factory to initialize [IVsPathContext2](#ivspathcontext2-interface) instances. (5.0+)
- [`IVsProjectJsonToPackageReferenceMigrator`](#ivsprojectjsontopackagereferencemigrator-interface) Contains methods to migrate a project.json based legacy project to PackageReference based project. (4.3+)
- [`IVsSemanticVersionComparer`](#ivssemanticversioncomparer-interface) An interface for comparing two opaque version strings by treating them as NuGet semantic (4.0+)
- [`IVsNuGetProjectUpdateEvents`](#ivsnugetprojectupdateevents-interface) (6.2+)

#### NuGet.SolutionRestoreManager

These interfaces are designed for project systems to interact with NuGet, allowing the project system to notify NuGet of changes to `PackageReference`s, and orchestrate batch updates. Visual Studio extensions that are not project systems probably will not benefit from these APIs.

- [`IVsSolutionRestoreService`](#ivssolutionrestoreservice-interface) (4.0+)
- [`IVsSolutionRestoreService2`](#ivssolutionrestoreservice2-interface) (4.3+)
- [`IVsSolutionRestoreService3`](#ivssolutionrestoreservice3-interface) (5.1+)
- [`IVsSolutionRestoreService4`](#ivssolutionrestoreservice4-interface) (6.0+)
- [`IVsSolutionRestoreService5`](#ivssolutionrestoreservice5-interface) (6.11+)
- [`IVsSolutionRestoreStatusProvider`](#ivssolutionrestorestatusprovider-interface) (6.0+)

## Using NuGet Services

> [!Warning]
> Do not use any other types besides the public interfaces in your code, and do not reference any other NuGet assemblies, such as `NuGet.Protocol.dll`, `NuGet.Frameworks.dll`, and so on.

In order to maximize the backwards compatibility promises we make, but also providing ourselves the flexibility to implement new features, performance improvements, and bug fixes in Visual Studio, we do not support the NuGet Client SDK being used in Visual Studio, and we do not provide binding redirects in `devenv.exe.config` to assemblies other than our VS extensibility contracts.

If you would like a new NuGet related API in Visual Studio, please search [NuGet's Home repo](https://github.com/NuGet/Home/) and upvote any existing issues if you find a similar one. If you can't find an existing feature request to upvote, please create one.

### Brokered Services

1. Install the [`NuGet.VisualStudio.Contracts`](https://www.nuget.org/packages/NuGet.VisualStudio.Contracts/) package into your project, as well as [`Microsoft.VisualStudio.SDK`](https://www.nuget.org/packages/Microsoft.VisualStudio.SDK).

1. Use the `IAsyncServiceProvider` to get Visual Studio's service broker, and use that to get NuGet's service. Note that [`AsyncPackage` extends `IVsAsyncServiceProvider2`](/dotnet/api/microsoft.visualstudio.shell.asyncpackage), so your class that implements `AsyncPackage` can be used as the `IAsyncServiceProvider`. Also see the docs on [`IBrokeredServiceContainer`](/dotnet/api/microsoft.visualstudio.shell.servicebroker.ibrokeredservicecontainer) and [`IServiceBroker`](/dotnet/api/microsoft.servicehub.framework.iservicebroker)

   ```cs
   // Your AsyncPackage implements IAsyncServiceProvider
   IAsyncServiceProvider asyncServiceProvider = this;
   var brokeredServiceContainer = await asyncServiceProvider.GetServiceAsync<SVsBrokeredServiceContainer, IBrokeredServiceContainer>();
   var serviceBroker = brokeredServiceContainer.GetFullAccessServiceBroker();
   var nugetProjectService = await serviceBroker.GetProxyAsync<INuGetProjectService>(NuGetServices.NuGetProjectServiceV1);
   ```

1. When your code no longer needs NuGet's brokered service, dispose it. For example, if you only need NuGet's brokered service during a single method call, you can wrap it in a C#`using` statement:

   ```cs
   InstalledPackagesResult installedPackagesResult;
   using (nugetProjectService as IDisposable)
   {
       installedPackagesResult = await nugetProjectService.GetInstalledPackages(projectGuid, cancellationToken);
   }
   ```

### MEF Services

1. Install the [`NuGet.VisualStudio`](https://www.nuget.org/packages/NuGet.VisualStudio) package into your project, which contains the `NuGet.VisualStudio.dll` assembly.

    In NuGet 5.11 and earlier, the package automatically sets the [**Embed Interop Types**](/dotnet/framework/interop/type-equivalence-and-embedded-interop-types) property of the assembly reference to **True**. [Visual Studio 2022 policy regarding embed interop types changed](/visualstudio/extensibility/migration/migrated-assemblies?view=vs-2022&preserve-view=true), so NuGet.VisualStudio package version 6.0.0 and above no longer use this.

1. To use a service, import it through the [MEF Import attribute](/dotnet/framework/mef/index#imports-and-exports-with-attributes), or through the [IComponentModel service](/dotnet/api/microsoft.visualstudio.componentmodelhost.icomponentmodel).

    ```cs
    //Using the Import attribute
    [Import(typeof(IVsPackageInstaller2))]
    public IVsPackageInstaller2 packageInstaller;
    packageInstaller.InstallLatestPackage(null, currentProject,
        "Newtonsoft.Json", false, false);

    //Using the IComponentModel service
    var componentModel = (IComponentModel)GetService(typeof(SComponentModel));
    IVsPackageInstallerServices installerServices =
        componentModel.GetService<IVsPackageInstallerServices>();

    var installedPackages = installerServices.GetInstalledPackages();
    ```

For reference, the source code for NuGet.VisualStudio is contained within the [NuGet.Clients repository](https://github.com/NuGet/NuGet.Client/tree/dev/src/NuGet.Clients/NuGet.VisualStudio).

## Understanding the .NET project systems

When SDK style projects were added for .NET Core 1.0, it was designed to be more asynchronous than previous Visual Studio project systems. This has an impact on how all other Visual Studio components interact with it directly, or though other components such as NuGet. This is most noticeable on solution load and project load, where projects are not fully available some time after Visual Studio's older synchronous API notifications have already fired.

During solution load, NuGet ignores `IVsSolutionEvents.OnAfterProjectLoad`, in order to avoid delaying the synchronous part of solution load. NuGet will synchronize its internal data structures after the synchronous part of solution load has completed. This is also true for non-SDK style projects.

Even after all `IVsSolutionEvents.OnAfterSolutionLoad` event handlers finish, this only signals the end of the synchronous part of solution load. The asynchronous part of solution load is still in progress. Therefore, if your extension calls NuGet APIs like `GetInstalledPackagesAsync` or `InstallPackage` soon after project or solution load, NuGet might throw an `InvalidOperationException` with message similar to "The operation failed as details for project {project name} could not be loaded.".

When a solution contains at least one SDK style project, NuGet will automatically perform a restore after solution load, and you should not call any Nuget APIs until this is complete. You can use `IVsNuGetProjectUpdateEvents` to get a notification when the solution restore, or when specific project restores, complete. If a solution does not contain any SDK style projects, then restore will not be scheduled automatically, and may not happen until a build is scheduled.

In order to determine whether a project uses NuGet's asynchronous flow (SDK style project), use [`PackageUtilities.IsCapabilityMatch`](/dotnet/api/microsoft.visualstudio.shell.packageutilities.iscapabilitymatch) with the expression `CPS + PackageReference`.

## INuGetProjectService interface

```cs
    /// <summary>Service to interact with projects in a solution</summary>
    /// <remarks>This interface should not be implemented. New methods may be added over time.</remarks>
    public interface INuGetProjectService
    {
        /// <Summary>Gets the list of packages installed in a project.</Summary>
        /// <param name="projectId">Project ID (GUID).</param>
        /// <param name="cancellationToken">Cancellation token.</param>
        /// <returns>The list of packages in the project.</returns>
        Task<InstalledPackagesResult> GetInstalledPackagesAsync(Guid projectId, CancellationToken cancellationToken);
    }
```

## IRegistryKey interface

```cs
/// <summary>
/// Specifies methods for manipulating a key in the Windows registry.
/// </summary>
public interface IRegistryKey
    {
    /// <summary>
    /// Retrieves the specified subkey for read or read/write access.
    /// </summary>
    /// <param name="name">The name or path of the subkey to create or open.</param>
    /// <returns>The subkey requested, or null if the operation failed.</returns>
    IRegistryKey OpenSubKey(string name);


    /// <summary>
    /// Retrieves the value associated with the specified name.
    /// </summary>
    /// <param name="name">The name of the value to retrieve. This string is not case-sensitive.</param>
    /// <returns>The value associated with name, or null if name is not found.</returns>
    object GetValue(string name);


    /// <summary>
    /// Closes the key and flushes it to disk if its contents have been modified.
    /// </summary>
    void Close();
}
```

## IVsCredentialProvider interface

```cs
    /// <summary>
    /// Contains methods to get credentials for NuGet operations.
    /// </summary>
    public interface IVsCredentialProvider
    {
        /// <summary>
        /// Get credentials for the supplied package source Uri.
        /// </summary>
        /// <param name="uri">The NuGet package source Uri for which credentials are being requested. Implementors are
        /// expected to first determine if this is a package source for which they can supply credentials.
        /// If not, then Null should be returned.</param>
        /// <param name="proxy">Web proxy to use when comunicating on the network.  Null if there is no proxy
        /// authentication configured.</param>
        /// <param name="isProxyRequest">True if if this request is to get proxy authentication
        /// credentials. If the implementation is not valid for acquiring proxy credentials, then
        /// null should be returned.</param>
        /// <param name="isRetry">True if credentials were previously acquired for this uri, but
        /// the supplied credentials did not allow authorized access.</param>
        /// <param name="nonInteractive">If true, then interactive prompts must not be allowed.</param>
        /// <param name="cancellationToken">This cancellation token should be checked to determine if the
        /// operation requesting credentials has been cancelled.</param>
        /// <returns>Credentials acquired by this provider for the given package source uri.
        /// If the provider does not handle requests for the input parameter set, then null should be returned.
        /// If the provider does handle the request, but cannot supply credentials, an exception should be thrown.</returns>
        Task<ICredentials> GetCredentialsAsync(Uri uri,
            IWebProxy proxy,
            bool isProxyRequest,
            bool isRetry,
            bool nonInteractive,
            CancellationToken cancellationToken);
    }
```

## IVsFrameworkCompatibility interface

```cs
    /// <summary>
    /// Contains methods to discover frameworks and compatibility between frameworks.
    /// </summary>
    public interface IVsFrameworkCompatibility
    {
        /// <summary>
        /// Gets all .NETStandard frameworks currently supported, in ascending order by version.
        /// </summary>
        /// <remarks>This API is <a href="https://github.com/microsoft/vs-threading/blob/main/doc/cookbook_vs.md#how-do-i-effectively-verify-that-my-code-is-fully-free-threaded">free-threaded.</a></remarks>
        IEnumerable<FrameworkName> GetNetStandardFrameworks();

        /// <summary>
        /// Gets frameworks that support packages of the provided .NETStandard version.
        /// </summary>
        /// <remarks>
        /// The result list is not exhaustive as it is meant to human-readable. For example,
        /// equivalent frameworks are not returned. Additionally, a framework name with version X
        /// in the result implies that framework names with versions greater than or equal to X
        /// but having the same <see cref="FrameworkName.Identifier"/> are also supported.
        ///
        /// <para>This API is <a href="https://github.com/microsoft/vs-threading/blob/main/doc/cookbook_vs.md#how-do-i-effectively-verify-that-my-code-is-fully-free-threaded">free-threaded.</a></para>
        /// </remarks>
        /// <param name="frameworkName">The .NETStandard version to get supporting frameworks for.</param>
        [Obsolete("This API does not support .NET 5 and higher target frameworks with platforms. Use IVsFrameworkCompatibility3 instead.")]
        IEnumerable<FrameworkName> GetFrameworksSupportingNetStandard(FrameworkName frameworkName);

        /// <summary>
        /// Selects the framework from <paramref name="frameworks"/> that is nearest
        /// to the <paramref name="targetFramework"/>, according to NuGet's framework
        /// compatibility rules. <c>null</c> is returned of none of the frameworks
        /// are compatible.
        /// </summary>
        /// <remarks>This API is <a href="https://github.com/microsoft/vs-threading/blob/main/doc/cookbook_vs.md#how-do-i-effectively-verify-that-my-code-is-fully-free-threaded">free-threaded.</a></remarks>
        /// <param name="targetFramework">The target framework.</param>
        /// <param name="frameworks">The list of frameworks to choose from.</param>
        /// <exception cref="ArgumentException">If any of the arguments are <c>null</c>.</exception>
        /// <returns>The nearest framework.</returns>
        [Obsolete("This API does not support .NET 5 and higher target frameworks with platforms. Use IVsFrameworkCompatibility3 instead.")]
        FrameworkName GetNearest(FrameworkName targetFramework, IEnumerable<FrameworkName> frameworks);
    }
```

## IVsFrameworkCompatibility2 interface

```cs
    /// <summary>
    /// Contains methods to discover frameworks and compatibility between frameworks.
    /// </summary>
    [Obsolete("This API does not support .NET 5 and higher target frameworks with platforms. Use IVsFrameworkCompatibility3 instead.")]
    public interface IVsFrameworkCompatibility2 : IVsFrameworkCompatibility
    {
        /// <summary>
        /// Selects the framework from <paramref name="frameworks"/> that is nearest
        /// to the <paramref name="targetFramework"/>, according to NuGet's framework
        /// compatibility rules. <c>null</c> is returned of none of the frameworks
        /// are compatible.
        /// </summary>
        /// <remarks>This API is <a href="https://github.com/microsoft/vs-threading/blob/main/doc/cookbook_vs.md#how-do-i-effectively-verify-that-my-code-is-fully-free-threaded">free-threaded.</a></remarks>
        /// <param name="targetFramework">The target framework.</param>
        /// <param name="fallbackTargetFrameworks">
        /// Target frameworks to use if the provided <paramref name="targetFramework"/> is not compatible.
        /// These fallback frameworks are attempted in sequence after <paramref name="targetFramework"/>.
        /// </param>
        /// <param name="frameworks">The list of frameworks to choose from.</param>
        /// <exception cref="ArgumentException">If any of the arguments are <c>null</c>.</exception>
        /// <returns>The nearest framework.</returns>
        [Obsolete("This API does not support .NET 5 and higher target frameworks with platforms. Use IVsFrameworkCompatibility3 instead.")]
        FrameworkName GetNearest(
            FrameworkName targetFramework,
            IEnumerable<FrameworkName> fallbackTargetFrameworks,
            IEnumerable<FrameworkName> frameworks);
    }
```

## IVsFrameworkCompatibility3 interface

```cs
    /// <summary>
    /// Contains methods to discover frameworks and compatibility between frameworks.
    /// </summary>
    public interface IVsFrameworkCompatibility3
    {
        /// <summary>
        /// Selects the framework from <paramref name="frameworks"/> that is nearest
        /// to the <paramref name="targetFramework"/>, according to NuGet's framework
        /// compatibility rules. <c>null</c> is returned of none of the frameworks
        /// are compatible.
        /// </summary>
        /// <param name="targetFramework">The target framework.</param>
        /// <param name="frameworks">The list of frameworks to choose from.</param>
        /// <exception cref="ArgumentNullException">If any of the arguments are null.</exception>
        /// <exception cref="ArgumentException">If any of the frameworks cannot be parsed.</exception>
        /// <returns>The nearest framework.</returns>
        /// <remarks>This API is <a href="https://github.com/microsoft/vs-threading/blob/9f065f155525c4561257e02ad61e66e93e073886/doc/cookbook_vs.md#how-do-i-effectively-verify-that-my-code-is-fully-free-threaded">free-threaded</a>.</remarks>
        IVsNuGetFramework GetNearest(IVsNuGetFramework targetFramework, IEnumerable<IVsNuGetFramework> frameworks);

        /// <summary>
        /// Selects the framework from <paramref name="frameworks"/> that is nearest
        /// to the <paramref name="targetFramework"/>, according to NuGet's framework
        /// compatibility rules. <c>null</c> is returned of none of the frameworks
        /// are compatible.
        /// </summary>
        /// <param name="targetFramework">The target framework.</param>
        /// <param name="fallbackTargetFrameworks">
        /// Target frameworks to use if the provided <paramref name="targetFramework"/> is not compatible.
        /// These fallback frameworks are attempted in sequence after <paramref name="targetFramework"/>.
        /// </param>
        /// <param name="frameworks">The list of frameworks to choose from.</param>
        /// <exception cref="ArgumentNullException">If any of the arguments are null.</exception>
        /// <exception cref="ArgumentException">If any of the frameworkscannot be parsed.</exception>
        /// <returns>The nearest framework.</returns>
        /// <remarks>This API is <a href="https://github.com/microsoft/vs-threading/blob/9f065f155525c4561257e02ad61e66e93e073886/doc/cookbook_vs.md#how-do-i-effectively-verify-that-my-code-is-fully-free-threaded">free-threaded</a>.</remarks>
        IVsNuGetFramework GetNearest(
            IVsNuGetFramework targetFramework,
            IEnumerable<IVsNuGetFramework> fallbackTargetFrameworks,
            IEnumerable<IVsNuGetFramework> frameworks);
    }
```

## IVsFrameworkParser interface

```cs
    /// <summary>
    /// An interface for dealing with the conversion between strings and <see cref="FrameworkName"/>
    /// instances.
    /// </summary>
    [Obsolete("This API does not support .NET 5 and higher target frameworks with platforms. Use IVsFrameworkParser2 instead.")]
    public interface IVsFrameworkParser
    {
        /// <summary>
        /// Parses a short framework name (e.g. "net45") or a full framework name
        /// (e.g. ".NETFramework,Version=v4.5") into a <see cref="FrameworkName"/>
        /// instance.
        /// </summary>
        /// <remarks>This API is <a href="https://github.com/microsoft/vs-threading/blob/main/doc/cookbook_vs.md#how-do-i-effectively-verify-that-my-code-is-fully-free-threaded">free-threaded.</a></remarks>
        /// <param name="shortOrFullName">The framework string.</param>
        /// <exception cref="ArgumentNullException">If the provided string is null.</exception>
        /// <exception cref="ArgumentException">If the provided string cannot be parsed.</exception>
        /// <returns>The parsed framework.</returns>
        [Obsolete("This API does not support .NET 5 and higher target frameworks with platforms. Use IVsFrameworkParser2 instead.")]
        FrameworkName ParseFrameworkName(string shortOrFullName);

        /// <summary>
        /// Gets the shortened version of the framework name from a <see cref="FrameworkName"/>
        /// instance.
        /// </summary>
        /// <remarks>
        /// For example, ".NETFramework,Version=v4.5" is converted to "net45". This is the value
        /// used inside of .nupkg folder structures as well as in project.json files.
        /// <para>This API is <a href="https://github.com/microsoft/vs-threading/blob/main/doc/cookbook_vs.md#how-do-i-effectively-verify-that-my-code-is-fully-free-threaded">free-threaded.</a></para>
        /// </remarks>
        /// <param name="frameworkName">The framework name.</param>
        /// <exception cref="ArgumentNullException">If the input is null.</exception>
        /// <exception cref="ArgumentException">
        /// If the provided framework name cannot be converted to a short name.
        /// </exception>
        /// <returns>The short framework name. </returns>
        [Obsolete("This API does not support .NET 5 and higher target frameworks with platforms. Use IVsFrameworkParser2 instead.")]
        string GetShortFrameworkName(FrameworkName frameworkName);
    }
```

## IVsFrameworkParser2 interface

```cs
    /// <summary>An interface to parse .NET Framework strings. See <a href="http://aka.ms/NuGet-IVsFrameworkParser">http://aka.ms/NuGet-IVsFrameworkParser</a>.</summary>
    public interface IVsFrameworkParser2
    {
        /// <summary>
        /// Parses a short framework name (e.g. "net45") or a full Target Framework Moniker
        /// (e.g. ".NETFramework,Version=v4.5") into a <see cref="IVsNuGetFramework"/>
        /// instance.
        /// </summary>
        /// <param name="input">The framework string</param>
        /// <param name="nuGetFramework">The resulting <see cref="IVsNuGetFramework"/>. If the method returns false, this return NuGet's "Unsupported" framework details.</param>
        /// <returns>A boolean to specify whether the input could be parsed into a valid <see cref="IVsNuGetFramework"/> object.</returns>
        /// <remarks>This API is not needed to get framework information about loaded projects, and should not be used to parse the project's TargetFramework property. See <a href="http://aka.ms/NuGet-IVsFrameworkParser">http://aka.ms/NuGet-IVsFrameworkParser</a>.<br/>
        /// This API is <a href="https://github.com/microsoft/vs-threading/blob/9f065f155525c4561257e02ad61e66e93e073886/doc/cookbook_vs.md#how-do-i-effectively-verify-that-my-code-is-fully-free-threaded">free-threaded</a>.</remarks>
        bool TryParse(string input, out IVsNuGetFramework nuGetFramework);
    }
```

## IVsPackageInstaller interface

```cs
    /// <summary>
    /// Contains methods to install packages into a project within the current solution.
    /// </summary>
    [ComImport]
    [Guid("4F3B122B-A53B-432C-8D85-0FAFB8BE4FF4")]
    public interface IVsPackageInstaller
    {
        /// <summary>
        /// Installs a single package from the specified package source.
        /// </summary>
        /// <remarks>Can be called from a background thread, if the UI thread is not blocked waiting for this to finish.
        /// See <a href="https://github.com/nuget/home/issues/11476">https://github.com/nuget/home/issues/11476</a></remarks>
        /// <param name="source">
        /// The package source to install the package from. This value can be <c>null</c>
        /// to indicate that the user's configured sources should be used. Otherwise,
        /// this should be the source path as a string. If the user has credentials
        /// configured for a source, this value must exactly match the configured source
        /// value.
        /// </param>
        /// <param name="project">The target project for package installation.</param>
        /// <param name="packageId">The package ID of the package to install.</param>
        /// <param name="version">
        /// The version of the package to install. <c>null</c> can be provided to
        /// install the latest version of the package.
        /// </param>
        /// <param name="ignoreDependencies">
        /// A boolean indicating whether or not to ignore the package's dependencies
        /// during installation.
        /// </param>
        [Obsolete("System.Version does not support SemVer pre-release versions. Use the overload with string version instead.")]
        void InstallPackage(string source, Project project, string packageId, Version version, bool ignoreDependencies);

        /// <summary>
        /// Installs a single package from the specified package source.
        /// </summary>
        /// <remarks>Can be called from a background thread, if the UI thread is not blocked waiting for this to finish.
        /// See <a href="https://github.com/nuget/home/issues/11476">https://github.com/nuget/home/issues/11476</a></remarks>
        /// <param name="source">
        /// The package source to install the package from. This value can be <c>null</c>
        /// to indicate that the user's configured sources should be used. Otherwise,
        /// this should be the source path as a string. If the user has credentials
        /// configured for a source, this value must exactly match the configured source
        /// value.
        /// </param>
        /// <param name="project">The target project for package installation.</param>
        /// <param name="packageId">The package ID of the package to install.</param>
        /// <param name="version">
        /// The version of the package to install. <c>null</c> can be provided to
        /// install the latest version of the package.
        /// </param>
        /// <param name="ignoreDependencies">
        /// A boolean indicating whether or not to ignore the package's dependencies
        /// during installation.
        /// </param>
        void InstallPackage(string source, Project project, string packageId, string version, bool ignoreDependencies);

        /// <summary>
        /// Installs a single package from the specified package source.
        /// </summary>
        /// <param name="repository">The package repository to install the package from.</param>
        /// <param name="project">The target project for package installation.</param>
        /// <param name="packageId">The package id of the package to install.</param>
        /// <param name="version">
        /// The version of the package to install. <c>null</c> can be provided to
        /// install the latest version of the package.
        /// </param>
        /// <param name="ignoreDependencies">
        /// A boolean indicating whether or not to ignore the package's dependencies
        /// during installation.
        /// </param>
        /// <param name="skipAssemblyReferences">
        /// A boolean indicating if assembly references from the package should be
        /// skipped.
        /// </param>
        [Obsolete]
        void InstallPackage(IPackageRepository repository, Project project, string packageId, string version, bool ignoreDependencies, bool skipAssemblyReferences);

        /// <summary>
        /// Installs one or more packages that exist on disk in a folder defined in the registry.
        /// </summary>
        /// <param name="keyName">
        /// The registry key name (under NuGet's repository key) that defines the folder on disk
        /// containing the packages.
        /// </param>
        /// <param name="isPreUnzipped">
        /// A boolean indicating whether the folder contains packages that are
        /// pre-unzipped.
        /// </param>
        /// <param name="skipAssemblyReferences">
        /// A boolean indicating whether the assembly references from the packages
        /// should be skipped.
        /// </param>
        /// <param name="project">The target project for package installation.</param>
        /// <param name="packageVersions">
        /// A dictionary of packages/versions to install where the key is the package id
        /// and the value is the version.
        /// </param>
        /// <remarks>
        /// If any version of the package is already installed, no action will be taken.
        /// <para>
        /// Dependencies are always ignored.
        /// </para>
        /// <para>Can be called from a background thread, if the UI thread is not blocked waiting for this to finish.
        /// See <a href="https://github.com/nuget/home/issues/11476">https://github.com/nuget/home/issues/11476</a></para>
        /// </remarks>
        void InstallPackagesFromRegistryRepository(string keyName, bool isPreUnzipped, bool skipAssemblyReferences, Project project, IDictionary<string, string> packageVersions);

        /// <summary>
        /// Installs one or more packages that exist on disk in a folder defined in the registry.
        /// </summary>
        /// <param name="keyName">
        /// The registry key name (under NuGet's repository key) that defines the folder on disk
        /// containing the packages.
        /// </param>
        /// <param name="isPreUnzipped">
        /// A boolean indicating whether the folder contains packages that are
        /// pre-unzipped.
        /// </param>
        /// <param name="skipAssemblyReferences">
        /// A boolean indicating whether the assembly references from the packages
        /// should be skipped.
        /// </param>
        /// <param name="ignoreDependencies">A boolean indicating whether the package's dependencies should be ignored</param>
        /// <param name="project">The target project for package installation.</param>
        /// <param name="packageVersions">
        /// A dictionary of packages/versions to install where the key is the package id
        /// and the value is the version.
        /// </param>
        /// <remarks>
        /// If any version of the package is already installed, no action will be taken.
        /// <para>Can be called from a background thread, if the UI thread is not blocked waiting for this to finish.
        /// See <a href="https://github.com/nuget/home/issues/11476">https://github.com/nuget/home/issues/11476</a></para>
        /// </remarks>
        void InstallPackagesFromRegistryRepository(string keyName, bool isPreUnzipped, bool skipAssemblyReferences, bool ignoreDependencies, Project project, IDictionary<string, string> packageVersions);

        /// <summary>
        /// Installs one or more packages that are embedded in a Visual Studio Extension Package.
        /// </summary>
        /// <param name="extensionId">The Id of the Visual Studio Extension Package.</param>
        /// <param name="isPreUnzipped">
        /// A boolean indicating whether the folder contains packages that are
        /// pre-unzipped.
        /// </param>
        /// <param name="skipAssemblyReferences">
        /// A boolean indicating whether the assembly references from the packages
        /// should be skipped.
        /// </param>
        /// <param name="project">The target project for package installation</param>
        /// <param name="packageVersions">
        /// A dictionary of packages/versions to install where the key is the package id
        /// and the value is the version.
        /// </param>
        /// <remarks>
        /// If any version of the package is already installed, no action will be taken.
        /// <para>
        /// Dependencies are always ignored.
        /// </para>
        /// <para>Can be called from a background thread, if the UI thread is not blocked waiting for this to finish.
        /// See <a href="https://github.com/nuget/home/issues/11476">https://github.com/nuget/home/issues/11476</a></para>
        /// </remarks>
        void InstallPackagesFromVSExtensionRepository(string extensionId, bool isPreUnzipped, bool skipAssemblyReferences, Project project, IDictionary<string, string> packageVersions);

        /// <summary>
        /// Installs one or more packages that are embedded in a Visual Studio Extension Package.
        /// </summary>
        /// <param name="extensionId">The Id of the Visual Studio Extension Package.</param>
        /// <param name="isPreUnzipped">
        /// A boolean indicating whether the folder contains packages that are
        /// pre-unzipped.
        /// </param>
        /// <param name="skipAssemblyReferences">
        /// A boolean indicating whether the assembly references from the packages
        /// should be skipped.
        /// </param>
        /// <param name="ignoreDependencies">A boolean indicating whether the package's dependencies should be ignored</param>
        /// <param name="project">The target project for package installation</param>
        /// <param name="packageVersions">
        /// A dictionary of packages/versions to install where the key is the package id
        /// and the value is the version.
        /// </param>
        /// <remarks>
        /// If any version of the package is already installed, no action will be taken.
        /// <para>Can be called from a background thread, if the UI thread is not blocked waiting for this to finish.
        /// See <a href="https://github.com/nuget/home/issues/11476">https://github.com/nuget/home/issues/11476</a></para>
        /// </remarks>
        void InstallPackagesFromVSExtensionRepository(string extensionId, bool isPreUnzipped, bool skipAssemblyReferences, bool ignoreDependencies, Project project, IDictionary<string, string> packageVersions);
    }
```

## IVsPackageinstaller2 interface

```cs

    /// <summary>
    /// Contains method to install latest version of a single package into a project within the current solution.
    /// </summary>
    public interface IVsPackageInstaller2 : IVsPackageInstaller
    {
        /// <summary>
        /// Installs the latest version of a single package from the specified package source.
        /// </summary>
        /// <remarks>Can be called from a background thread, if the UI thread is not blocked waiting for this to finish.
        /// See <a href="https://github.com/nuget/home/issues/11476">https://github.com/nuget/home/issues/11476</a></remarks>
        /// <param name="source">
        /// The package source to install the package from. This value can be <c>null</c>
        /// to indicate that the user's configured sources should be used. Otherwise,
        /// this should be the source path as a string. If the user has credentials
        /// configured for a source, this value must exactly match the configured source
        /// value.
        /// </param>
        /// <param name="project">The target project for package installation.</param>
        /// <param name="packageId">The package ID of the package to install.</param>
        /// <param name="includePrerelease">
        /// Whether or not to consider prerelease versions when finding the latest version
        /// to install.
        /// </param>
        /// <param name="ignoreDependencies">
        /// A boolean indicating whether or not to ignore the package's dependencies
        /// during installation.
        /// </param>
        /// <exception cref="InvalidOperationException">
        /// Thrown when <see paramref="includePrerelease"/> is <c>false</c> and no stable version
        /// of the package exists.
        /// </exception>
        void InstallLatestPackage(
            string source,
            Project project,
            string packageId,
            bool includePrerelease,
            bool ignoreDependencies);
    }
```

## IVsPackageInstallerEvents interface

> [!Note]
> These events are only raised for packages.config projects. To get updates for both packages.config and PackageReference use [`IVsNuGetProjectUpdateEvents`](#ivsnugetprojectupdateevents-interface) instead.

```cs
    /// <summary>
    /// Contains events which are raised when packages are installed or uninstalled from projects and the current
    /// solution.
    /// </summary>
    public interface IVsPackageInstallerEvents
    {
        /// <summary>
        /// Raised when a package is about to be installed into the current solution.
        /// </summary>
        event VsPackageEventHandler PackageInstalling;

        /// <summary>
        /// Raised after a package has been installed into the current solution.
        /// </summary>
        event VsPackageEventHandler PackageInstalled;

        /// <summary>
        /// Raised when a package is about to be uninstalled from the current solution.
        /// </summary>
        event VsPackageEventHandler PackageUninstalling;

        /// <summary>
        /// Raised after a package has been uninstalled from the current solution.
        /// </summary>
        event VsPackageEventHandler PackageUninstalled;

        /// <summary>
        /// Raised after a package has been installed into a project within the current solution.
        /// </summary>
        event VsPackageEventHandler PackageReferenceAdded;

        /// <summary>
        /// Raised after a package has been uninstalled from a project within the current solution.
        /// </summary>
        event VsPackageEventHandler PackageReferenceRemoved;
    }
```

## IVsPackageInstallerProjectEvents interface

> [!Note]
> These events are only raised for packages.config projects. To get updates for both packages.config and PackageReference use [`IVsNuGetProjectUpdateEvents`](#ivsnugetprojectupdateevents-interface) instead.

```cs
    /// <summary>
    /// Contains batch events which are raised when packages are installed or uninstalled from projects with packages.config
    /// and the current solution.
    /// </summary>
    public interface IVsPackageInstallerProjectEvents
    {
        /// <summary>
        /// Raised before any IVsPackageInstallerEvents events are raised for a project.
        /// </summary>
        event VsPackageProjectEventHandler BatchStart;

        /// <summary>
        /// Raised after all IVsPackageInstallerEvents events are raised for a project.
        /// </summary>
        event VsPackageProjectEventHandler BatchEnd;

    }
```

## IVsPackageInstallerServices interface

```cs
    /// <summary>
    /// Contains methods to query for installed packages within the current solution.
    /// </summary>
    [Obsolete("Use INuGetProjectService in the NuGet.VisualStudio.Contracts package instead.")]
    public interface IVsPackageInstallerServices
    {
        /// <summary>
        /// Get the list of NuGet packages installed in the current solution.
        /// </summary>
        [Obsolete("This method can cause UI delays if called on the UI thread. Use INuGetProjectService.GetInstalledPackagesAsync in the NuGet.VisualStudio.Contracts package instead, and iterate all projects in the solution")]
        IEnumerable<IVsPackageMetadata> GetInstalledPackages();

        /// <summary>
        /// Checks if a NuGet package with the specified Id is installed in the specified project.
        /// </summary>
        /// <param name="project">The project to check for NuGet package.</param>
        /// <param name="id">The id of the package to check.</param>
        /// <returns><c>true</c> if the package is install. <c>false</c> otherwise.</returns>
        /// <exception cref="InvalidOperationException">A "project not nominated" exception will be thrown if the project system has not yet told NuGet about the project.
        /// You can use <see cref="IVsNuGetProjectUpdateEvents"/> or Microsoft.VisualStudio.OperationProgress to be notified when the project is ready.</exception>
        [Obsolete("This method can cause UI delays if called on the UI thread. Use INuGetProjectService.GetInstalledPackagesAsync in the NuGet.VisualStudio.Contracts package instead, and check the specific package you're interested in")]
        bool IsPackageInstalled(Project project, string id);

        /// <summary>
        /// Checks if a NuGet package with the specified Id and version is installed in the specified project.
        /// </summary>
        /// <param name="project">The project to check for NuGet package.</param>
        /// <param name="id">The id of the package to check.</param>
        /// <param name="version">The version of the package to check.</param>
        /// <returns><c>true</c> if the package is install. <c>false</c> otherwise.</returns>
        /// <exception cref="InvalidOperationException">A "project not nominated" exception will be thrown if the project system has not yet told NuGet about the project.
        /// You can use <see cref="IVsNuGetProjectUpdateEvents"/> or Microsoft.VisualStudio.OperationProgress to be notified when the project is ready.</exception>
        [Obsolete("This method can cause UI delays if called on the UI thread. Use INuGetProjectService.GetInstalledPackagesAsync in the NuGet.VisualStudio.Contracts package instead, and check the specific package you're interested in")]
        bool IsPackageInstalled(Project project, string id, SemanticVersion version);

        /// <summary>
        /// Checks if a NuGet package with the specified Id and version is installed in the specified project.
        /// </summary>
        /// <param name="project">The project to check for NuGet package.</param>
        /// <param name="id">The id of the package to check.</param>
        /// <param name="versionString">The version of the package to check.</param>
        /// <returns><c>true</c> if the package is install. <c>false</c> otherwise.</returns>
        /// <remarks>
        /// The reason this method is named IsPackageInstalledEx, instead of IsPackageInstalled, is that
        /// when client project compiles against this assembly, the compiler would attempt to bind against
        /// the other overload which accepts SemanticVersion and would require client project to reference NuGet.Core.
        /// </remarks>
        /// <exception cref="InvalidOperationException">A "project not nominated" exception will be thrown if the project system has not yet told NuGet about the project.
        /// You can use <see cref="IVsNuGetProjectUpdateEvents"/> or Microsoft.VisualStudio.OperationProgress to be notified when the project is ready.</exception>
        [Obsolete("This method can cause UI delays if called on the UI thread. Use INuGetProjectService.GetInstalledPackagesAsync in the NuGet.VisualStudio.Contracts package instead, and check the specific package you're interested in")]
        bool IsPackageInstalledEx(Project project, string id, string versionString);

        /// <summary>
        /// Get the list of NuGet packages installed in the specified project.
        /// </summary>
        /// <param name="project">The project to get NuGet packages from.</param>
        /// <exception cref="InvalidOperationException">A "project not nominated" exception will be thrown if the project system has not yet told NuGet about the project.
        /// You can use <see cref="IVsNuGetProjectUpdateEvents"/> or Microsoft.VisualStudio.OperationProgress to be notified when the project is ready.</exception>
        [Obsolete("This method can cause UI delays if called on the UI thread. Use INuGetProjectService.GetInstalledPackagesAsync in the NuGet.VisualStudio.Contracts package instead")]
        IEnumerable<IVsPackageMetadata> GetInstalledPackages(Project project);
    }
```

## IVsPackageRestorer interface

```cs

    /// <summary>
    /// Contains methods to restore packages installed in a project within the current solution.
    /// </summary>
    public interface IVsPackageRestorer
    {
        /// <summary>
        /// Returns a value indicating whether the user consent to download NuGet packages
        /// has been granted.
        /// </summary>
        /// <remarks>Can be called from a background thread.</remarks>
        /// <returns>true if the user consent has been granted; otherwise, false.</returns>
        bool IsUserConsentGranted();

        /// <summary>
        /// Restores NuGet packages installed in the given project within the current solution.
        /// </summary>
        /// <remarks>Can be called from a background thread.</remarks>
        /// <param name="project">The project whose NuGet packages to restore.</param>
        void RestorePackages(Project project);
    }
```

## IVsPackageSourceProvider interface

```cs
    /// <summary>
    /// A public API for retrieving the list of NuGet package sources.
    /// </summary>
    public interface IVsPackageSourceProvider
    {
        /// <summary>
        /// Provides the list of package sources.
        /// </summary>
        /// <remarks>Can be called from a background thread.</remarks>
        /// <param name="includeUnOfficial">Unofficial sources will be included in the results</param>
        /// <param name="includeDisabled">Disabled sources will be included in the results</param>
        /// <remarks>Does not require the UI thread.</remarks>
        /// <exception cref="ArgumentException">Thrown if a NuGet configuration file is invalid.</exception>
        /// <exception cref="ArgumentNullException">Thrown if a NuGet configuration file is invalid.</exception>
        /// <exception cref="InvalidOperationException">Thrown if a NuGet configuration file is invalid.</exception>
        /// <exception cref="InvalidDataException">Thrown if a NuGet configuration file is invalid.</exception>
        /// <returns>Key: source name Value: source URI</returns>
        IEnumerable<KeyValuePair<string, string>> GetSources(bool includeUnOfficial, bool includeDisabled);

        /// <summary>
        /// Raised when sources are added, removed, disabled, or modified.
        /// </summary>
        event EventHandler SourcesChanged;
    }
```

## IVsPackageUninstaller interface

```cs
    /// <summary>
    /// Contains methods to uninstall packages from a project within the current solution.
    /// </summary>
    public interface IVsPackageUninstaller
    {
        /// <summary>
        /// Uninstall the specified package from a project and specify whether to uninstall its dependency packages
        /// too.
        /// </summary>
        /// <remarks>Can be called from a background thread, if the UI thread is not blocked waiting for this to finish.
        /// See <a href="https://github.com/nuget/home/issues/11476">https://github.com/nuget/home/issues/11476</a></remarks>
        /// <param name="project">The project from which the package is uninstalled.</param>
        /// <param name="packageId">The package to be uninstalled</param>
        /// <param name="removeDependencies">
        /// A boolean to indicate whether the dependency packages should be
        /// uninstalled too.
        /// </param>
        void UninstallPackage(Project project, string packageId, bool removeDependencies);
    }
```

## IVsPathContext interface

```cs
    /// <summary>
    /// NuGet path information specific to the current context (e.g. project context).
    /// Represents captured snapshot associated with current project/solution settings.
    /// Should be discarded immediately after all queries are done.
    /// </summary>
    public interface IVsPathContext
    {
        /// <summary>
        /// User package folder directory. The path returned is an absolute path.
        /// </summary>
        string UserPackageFolder { get; }

        /// <summary>
        /// Fallback package folder locations. The paths (if any) in the returned list are absolute paths. If no
        /// fallback package folders are configured, an empty list is returned. The item type of this sequence is
        /// <see cref="string"/>.
        /// </summary>
        /// <remarks>Can be called from a background thread.</remarks>
        IEnumerable FallbackPackageFolders { get; }

        /// <summary>
        /// Fetch a package directory containing the provided asset path.
        /// </summary>
        /// <param name="packageAssetPath">Absolute path to package asset file.</param>
        /// <param name="packageDirectoryPath">Full path to a package directory. 
        /// <code>null</code> if returned falue is <code>false</code>.</param>
        /// <returns>
        /// <code>true</code> when a package containing the given file was found, <code>false</code> - otherwise.
        /// </returns>
        /// <example>
        /// Suppose the project is a packages.config project and the following asset paths are provided:
        /// 
        /// - C:\src\MyProject\packages\NuGet.Versioning.3.5.0-rc1-final\lib\net45\NuGet.Versioning.dll
        /// - C:\path\to\non\package\assembly\Newtonsoft.Json.dll
        /// - C:\src\MyOtherProject\packages\NuGet.Core.2.12.0\lib\net40\NuGet.Core.dll
        /// - C:\src\MyProject\packages\Autofac.3.5.2\lib\net40\Autofac.dll
        /// - C:\src\MyProject\packages\Autofac.3.5.2\lib\net40\Autofac.Fake.dll
        /// 
        /// The result will be:
        /// 
        /// - C:\src\MyProject\packages\NuGet.Versioning.3.5.0-rc1-final
        /// - null
        /// - null
        /// - C:\src\MyProject\packages\Autofac.3.5.2
        /// - C:\src\MyProject\packages\Autofac.3.5.2
        /// </example>
        bool TryResolvePackageAsset(string packageAssetPath, out string packageDirectoryPath);
    }
```

## IVsPathContext2 interface

```cs
    /// <summary>
    /// NuGet path information specific to the current context (e.g. project context) or solution context
    /// Represents captured snapshot associated with current project/solution settings.
    /// Should be discarded immediately after all queries are done.
    /// </summary>
    public interface IVsPathContext2 : IVsPathContext
    {
        /// <summary>
        /// Solution packages folder directory. This will always be set irrespective if folder actually exists or not.
        /// The path returned is an absolute path.
        /// </summary>
        string SolutionPackageFolder { get; }
    }
```

## IVsPathContextProvider interface

```cs
    /// <summary>
    /// A factory to initialize <see cref="IVsPathContext"/> instances.
    /// </summary>
    public interface IVsPathContextProvider
    {
        /// <summary>
        /// Attempts to create an instance of <see cref="IVsPathContext"/>.
        /// </summary>
        /// <remarks>Can be called from a background thread.</remarks>
        /// <param name="projectUniqueName">
        /// Unique identificator of the project. Should be a full path to project file.
        /// </param>
        /// <param name="context">The path context associated with given project.</param>
        /// <returns>
        /// <code>True</code> if operation has succeeded and context was created.
        /// False, otherwise, e.g. when provided project is not managed by NuGet.
        /// </returns>
        /// <throws>
        /// <code>ArgumentNullException</code> if projectUniqueName is passed as null.
        /// <code>InvalidOperationException</code> when it fails to create a context and return appropriate error message.
        /// </throws>
        bool TryCreateContext(string projectUniqueName, out IVsPathContext context);
    }
```

## IVsPathContextProvider2 interface

```cs
    /// <summary>
    /// A factory to initialize <see cref="IVsPathContext2"/> instances.
    /// </summary>
    public interface IVsPathContextProvider2 : IVsPathContextProvider
    {
        /// <summary>
        /// Attempts to create an instance of <see cref="IVsPathContext2"/> for the solution.
        /// </summary>
        /// <remarks>This API is free-threaded, but APIs on the returned IVsPathContext2 may not be.</remarks>
        /// <param name="context">The path context associated with this solution.</param>
        /// <returns>
        /// <code>True</code> if operation has succeeded and context was created.
        /// <code>False</code> otherwise.
        /// </returns>
        /// <throws>
        /// <code>InvalidOperationException</code> when it fails to create a context and return appropriate error message.
        /// </throws>
        bool TryCreateSolutionContext(out IVsPathContext2 context);

        /// <summary>
        /// Attempts to create an instance of <see cref="IVsPathContext2"/> for the solution.
        /// </summary>
        /// <remarks>This API is free-threaded, but APIs on the returned IVsPathContext2 may not be.</remarks>
        /// <param name="solutionDirectory">
        /// path to the solution directory. Must be an absolute path.
        /// It will be performant to pass the solution directory if it's available.
        /// </param>
        /// <param name="context">The path context associated with this solution.</param>
        /// <returns>
        /// <code>True</code> if operation has succeeded and context was created.
        /// <code>False</code> otherwise.
        /// </returns>
        /// <throws>
        /// <code>ArgumentNullException</code> if solutionDirectory is passed as null.
        /// <code>InvalidOperationException</code> when it fails to create a context and return appropriate error message.
        /// </throws>
        bool TryCreateSolutionContext(string solutionDirectory, out IVsPathContext2 context);

        /// <summary>
        /// Attempts to create an instance of <see cref="IVsPathContext"/> containing only the user wide and machine wide configurations.
        /// If a solution is loaded, note that the values in the path context might not be the actual effective values for the solution.
        /// If a customer has overriden the `globalPackagesFolder` key or cleared the `fallbackPackageFolders`, these values will be incorrect.
        /// It is important to keep this scenario in mind when working with this path. To predict differences you can call this in combination with <see cref="TryCreateSolutionContext(out IVsPathContext2)"/>.
        /// </summary>
        /// <returns>
        /// <code>True</code> if operation has succeeded and context was created.
        /// <code>False</code> otherwise.
        /// </returns>
        /// <remarks>
        /// This method can be safely invoked from a background thread. Do note that this method might switch to the UI thread internally, so be mindful of blocking the UI thread on this.
        /// </remarks>
        bool TryCreateNoSolutionContext(out IVsPathContext vsPathContext);
    }
```

## IVsProjectJsonToPackageReferenceMigrator interface

```cs
    /// <summary>
    /// Contains methods to migrate a project.json based legacy project to PackageReference based project.
    /// </summary>
    public interface IVsProjectJsonToPackageReferenceMigrator
    {
        /// <summary>
        /// Migrates a legacy Project.json based project to Package Reference based project. The result 
        /// should be casted to type <see cref="IVsProjectJsonToPackageReferenceMigrateResult"/>
        /// The backup of the original project file and project.json file is created in the Backup folder
        /// in the root of the project directory.
        /// </summary>
        /// <param name="projectUniqueName">The full path to the project that needs to be migrated</param>
        Task<object> MigrateProjectJsonToPackageReferenceAsync(string projectUniqueName);

    }
```

## IVsSemanticVersionComparer interface

```cs
    /// <summary>
    /// An interface for comparing two opaque version strings by treating them as NuGet semantic
    /// versions.
    /// </summary>
    public interface IVsSemanticVersionComparer
    {
        /// <summary>
        /// Compares two version strings as if they were NuGet semantic version
        /// strings. Returns a number less than zero if <paramref name="versionA"/>
        /// is less than <paramref name="versionB"/>. Returns zero if the two versions 
        /// are equivalent. Returns a number greater than zero if <paramref name="versionA"/>
        /// is greater than <paramref name="versionB"/>.
        /// </summary>
        /// <remarks>This API is free-threaded.</remarks>
        /// <param name="versionA">The first version string.</param>
        /// <param name="versionB">The second version string.</param>
        /// <exception cref="ArgumentNullException">If either version string is null.</exception>
        /// <exception cref="ArgumentException">If either string cannot be parsed.</exception>
        /// <returns>
        /// A standard comparison integer based on the relationship between the
        /// two provided versions.
        /// </returns>
        int Compare(string versionA, string versionB);
    }
```

## IVsNuGetProjectUpdateEvents interface

```cs
    /// <summary>
    /// NuGet project update events.
    /// This API provides means of tracking project updates by NuGet.
    /// In particular, for PackageReference projects, updates to the assets file and nuget generated props/targets.
    /// For packages.config projects, package installations will be tracked.
    /// All events are fired from a threadpool thread.
    /// </summary>
    public interface IVsNuGetProjectUpdateEvents
    {
        /// <summary>
        /// Raised when solution restore starts with the list of projects that will be restored.
        /// The list will not include all projects. Some projects may have been skipped in earlier up to date check, and other projects may no-op.
        /// </summary>
        /// <remarks>
        /// Just because a project is being restored that doesn't necessarily mean any actual updates will happen.
        /// No heavy computation should happen in any of these methods as it'll block the NuGet progress.
        /// </remarks>
        event SolutionRestoreEventHandler SolutionRestoreStarted;

        /// <summary>
        /// Raised when solution restore finishes with the list of projects that were restored.
        /// The list will not include all projects. Some projects may have been skipped in earlier up to date check, and other projects may no-op.
        /// </summary>
        /// <remarks>
        /// Just because a project is being restored that doesn't necessarily mean any actual updates will happen.
        /// No heavy computation should happen in any of these methods as it'll block the NuGet progress.
        /// </remarks>
        event SolutionRestoreEventHandler SolutionRestoreFinished;

        /// <summary>
        /// Raised when particular project is about to be updated.
        /// For PackageReference projects, this means an assets file or a nuget temp msbuild file write (nuget.g.props or nuget.g.targets). The list of updated files will include the aforementioned.
        /// If a project was restored, but no file updates happen, this event will not be fired.
        /// For packages.config projects, this means that the project file was changed.
        /// </summary>
        /// <remarks>
        /// No heavy computation should happen in any of these methods as it'll block the NuGet progress.
        /// </remarks>
        event ProjectUpdateEventHandler ProjectUpdateStarted;

        /// <summary>
        /// Raised when particular project update has been completed.
        /// For PackageReference projects, this means an assets file or a nuget temp msbuild file write (nuget.g.props or nuget.g.targets). The list of updated files will include the aforementioned.
        /// If a project was restored, but no file updates happen, this event will not be fired.
        /// For packages.config projects, this means that the project file was changed.
        /// </summary>
        /// <remarks>
        /// No heavy computation should happen in any of these methods as it'll block the NuGet progress.
        /// </remarks>
        event ProjectUpdateEventHandler ProjectUpdateFinished;
    }

    /// <summary>
    /// Defines an event handler delegate for solution restore start and end.
    /// </summary>
    /// <param name="projects">List of projects that will run restore. Never <see langword="null"/>.</param>
    public delegate void SolutionRestoreEventHandler(IReadOnlyList<string> projects);

    /// <summary>
    /// Defines an event handler delegate for project updates.
    /// </summary>
    /// <param name="projectUniqueName">Project full path. Never <see langword="null"/>. </param>
    /// <param name="updatedFiles">NuGet output files that may be updated. Never <see langword="null"/>.</param>
    public delegate void ProjectUpdateEventHandler(string projectUniqueName, IReadOnlyList<string> updatedFiles);
}
```

## IVsSolutionRestoreService interface

```cs
    /// <summary>
    /// Represents a package restore service API for integration with a project system.
    /// </summary>
    public interface IVsSolutionRestoreService
    {
        /// <summary>
        /// A task providing last/current restore operation status.
        /// Could be null if restore has not started yet.
        /// </summary>
        /// <remarks>
        /// This task is a reflection of the current state of the current-restore-operation or
        /// recently-completed-restore. The usage of this property will be to continue,
        /// e.g. to build solution or something) on completion of this task.
        /// Also, on completion, if the task returns false then it means the restore failed and
        /// the build task will be terminated.
        /// </remarks>
        Task<bool> CurrentRestoreOperation { get; }

        /// <summary>
        /// An entry point used by CPS to indicate given project needs to be restored.
        /// </summary>
        /// <param name="projectUniqueName">
        /// Unique identifier of the project. Should be a full path to project file.
        /// </param>
        /// <param name="projectRestoreInfo">Metadata <see cref="IVsProjectRestoreInfo"/> needed for restoring the project.</param>
        /// <param name="token">Cancellation token.</param>
        /// <returns>
        /// Returns a restore task corresponding to the nominated project request.
        /// NuGet will batch restore requests so it's possible the same restore task will be returned for multiple projects.
        /// When the requested restore operation for the given project completes the task will indicate operation success or failure.
        /// </returns>
        /// <exception cref="ArgumentException">Thrown if <paramref name="projectUniqueName" /> is not the path of a project file.</exception>
        /// <exception cref="ArgumentNullException">Thrown if <paramref name="projectRestoreInfo" /> is <c>null</c>.</exception>
        /// <exception cref="OperationCanceledException">Thrown if <paramref name="token" /> is cancelled.</exception>
        [Obsolete("Use IVsSolutionRestoreService5 instead")]
        Task<bool> NominateProjectAsync(string projectUniqueName, IVsProjectRestoreInfo projectRestoreInfo, CancellationToken token);
    }
```

## IVsSolutionRestoreService2 interface

```cs
    /// <summary>
    /// Represents a package restore service API for integration with a project system.
    /// </summary>
    public interface IVsSolutionRestoreService2
    {
        /// <summary>
        /// An entry point which allows non-NETCore SDK based projects to indicate given project needs to be restored.
        /// </summary>
        /// <param name="projectUniqueName">
        /// Unique identificator of the project. Should be a full path to project file.
        /// </param>
        /// <param name="token">Cancellation token.</param>
        /// <returns>
        /// Returns a restore task corresponding to the nominated project request.
        /// NuGet will batch restore requests so it's possible the same restore task will be returned for multiple projects.
        /// When the requested restore operation for the given project completes the task will indicate operation success or failure.
        /// </returns>
        Task<bool> NominateProjectAsync(string projectUniqueName, CancellationToken token);
    }
```

## IVsSolutionRestoreService3 interface

```cs
    /// <summary>
    /// Represents a package restore service API for integration with a project system.
    /// </summary>
    public interface IVsSolutionRestoreService3
    {
        /// <summary>
        /// A task providing last/current restore operation status.
        /// Could be null if restore has not started yet.
        /// </summary>
        /// <remarks>
        /// This task is a reflection of the current state of the current-restore-operation or
        /// recently-completed-restore. The usage of this property will be to continue,
        /// e.g. to build solution or something) on completion of this task.
        /// Also, on completion, if the task returns false then it means the restore failed and
        /// the build task will be terminated.
        /// </remarks>
        Task<bool> CurrentRestoreOperation { get; }

        /// <summary>
        /// An entry point used by CPS to indicate given project needs to be restored.
        /// This entry point also handles PackageDownload items
        /// </summary>
        /// <param name="projectUniqueName">
        /// Unique identifier of the project. Should be a full path to project file.
        /// </param>
        /// <param name="projectRestoreInfo">Metadata <see cref="IVsProjectRestoreInfo2"/> needed for restoring the project.</param>
        /// <param name="token">Cancellation token.</param>
        /// <returns>
        /// Returns a restore task corresponding to the nominated project request.
        /// NuGet will batch restore requests so it's possible the same restore task will be returned for multiple projects.
        /// When the requested restore operation for the given project completes the task will indicate operation success or failure.
        /// </returns>
        /// <exception cref="ArgumentException">Thrown if <paramref name="projectUniqueName" /> is not the path of a project file.</exception>
        /// <exception cref="ArgumentNullException">Thrown if <paramref name="projectRestoreInfo" /> is <c>null</c>.</exception>
        /// <exception cref="OperationCanceledException">Thrown if <paramref name="token" /> is cancelled.</exception>
        [Obsolete("Use IVsSolutionRestoreService5 instead")]
        Task<bool> NominateProjectAsync(string projectUniqueName, IVsProjectRestoreInfo2 projectRestoreInfo, CancellationToken token);
    }
```

## IVsSolutionRestoreService4 interface

```cs
    /// <summary>
    /// Represents a package restore service API for integration with a project system.
    /// Implemented by NuGet.
    /// </summary>
    public interface IVsSolutionRestoreService4 : IVsSolutionRestoreService3
    {
        /// <summary>
        /// A project system can call this service (optionally) to register itself to coordinate restore. <br/>
        /// Each project can only register once. NuGet will call into the source to wait for nominations for restore. <br/>
        /// NuGet will remove the registered object when a project is unloaded.
        /// </summary>
        /// <param name="restoreInfoSource">Represents a project specific info source</param>
        /// <param name="cancellationToken">Cancellation token.</param>
        /// <exception cref="InvalidOperationException">If the project has already been registered.</exception>
        /// <exception cref="ArgumentNullException">If <paramref name="restoreInfoSource"/> is null. </exception>
        /// <exception cref="ArgumentException">If <paramref name="restoreInfoSource"/>'s <see cref="IVsProjectRestoreInfoSource.Name"/> is <see langword="null"/>. </exception>
        Task RegisterRestoreInfoSourceAsync(IVsProjectRestoreInfoSource restoreInfoSource, CancellationToken cancellationToken);
    }
```

## IVsSolutionRestoreService5 interface

```cs
    /// <summary>
    /// Represents a package restore service API for integration with a project system.
    /// Implemented by NuGet.
    /// </summary>
    public interface IVsSolutionRestoreService5 : IVsSolutionRestoreService4
    {
        /// <summary>
        /// An entry point used by CPS to indicate given project needs to be restored.
        /// </summary>
        /// <param name="projectUniqueName">
        /// The full path to the project file. In the VS SDK's IVsSolution, this is also known as the unique name.
        /// </param>
        /// <param name="projectRestoreInfo">Metadata <see cref="IVsProjectRestoreInfo3"/> needed for restoring the project.</param>
        /// <param name="token">Cancellation token.</param>
        /// <returns>
        /// Returns a restore task corresponding to the nominated project request.
        /// NuGet will batch restore requests so it's possible the same restore task will be returned for multiple projects.
        /// When the requested restore operation for the given project completes the task will indicate operation success or failure.
        /// </returns>
        /// <exception cref="ArgumentException">Thrown if <paramref name="projectUniqueName" /> is not the path of a project file,
        /// or if <paramref name="projectRestoreInfo"/> has some basic validation errors.</exception>
        /// <exception cref="ArgumentNullException">Thrown if <paramref name="projectRestoreInfo" /> is <see langword="null" />.</exception>
        /// <exception cref="OperationCanceledException">Thrown if <paramref name="token" /> is cancelled.</exception>
        Task<bool> NominateProjectAsync(string projectUniqueName, IVsProjectRestoreInfo3 projectRestoreInfo, CancellationToken token);
    }
```

## IVsProjectRestoreInfoSource interface

```cs
    /// <summary>
    /// Represents a package restore service API for integration with a project system.
    /// Implemented by the project-system.
    /// </summary>
    public interface IVsProjectRestoreInfoSource
    {
        /// <summary>
        /// Project Unique Name.
        /// Must be equivalent to the name provided in the <see cref="IVsSolutionRestoreService3.NominateProjectAsync(string, IVsProjectRestoreInfo2, CancellationToken)"/> or equivalent.
        /// </summary>
        /// <remarks>Never <see langword="null"/>.</remarks>
        string Name { get; }

        /// <summary>
        /// Whether the source needs to do some work that could lead to a nomination. <br/>
        /// Called frequently, so it should be very efficient.
        /// </summary>
        bool HasPendingNomination { get; }

        /// <summary>
        /// NuGet calls this method to wait on a potential nomination. <br/>
        /// If the project has no pending restore data, it will return a completed task. <br/>
        /// Otherwise, the task will be completed once the project nominates. <br/>
        /// The task will be cancelled, if the source decide it no longer needs to nominate (for example: the restore state has no change) <br/>
        /// The task will be failed, if the source runs into a problem, and it cannot get the correct data to nominate (for example: DT build failed) <br/>
        /// </summary>
        /// <param name="cancellationToken">Cancellation token.</param>
        Task WhenNominated(CancellationToken cancellationToken);
    }
```

## IVsSolutionRestoreStatusProvider interface

```cs
    /// <summary>
    /// Provides the status of IVsSolutionRestore.
    /// </summary>
    public interface IVsSolutionRestoreStatusProvider
    {
        /// <summary>
        /// IsRestoreCompleteAsync indicates whether or not automatic package restore has pending work.
        /// Automatic package restore applies for both packages.config and PackageReference projects.
        ///
        /// Returns true if all projects in the solution that require nomination have been nominated for restore and all pending restores have completed.
        /// The result does not indicate that restore completed successfully, a failed restore will still return true.
        /// </summary>
        /// <remarks>
        /// Special cases:
        /// * An empty solution will return true.
        /// * If no solution is open this will true.
        /// * An invalid project that does not provide restore details will cause this to return false since restore will not run for that project.
        ///
        /// Restores running due to Install/Update/Uninstall operations are NOT included in this status. Status here is limited to IVsSolutionRestoreService.
        /// </remarks>
        Task<bool> IsRestoreCompleteAsync(CancellationToken token);
    }
```
