# NuGet API in Visual Studio

Besides the UI that everyone has become familiar with, the NuGet extension also exports some useful services, via [MEF](http://msdn.microsoft.com/en-us/library/dd460648.aspx "MEF"), which allow other components in VS to interact with NuGet. These include installing packages, uninstalling packages and querying for installed packages.

As of NuGet 2.0, NuGet exports six interfaces/services via MEF, all of which reside in the `NuGet.VisualStudio` namespace in the `NuGet.VisualStudio.dll` assembly:

## Available Services

**IVsPackageInstaller**: Contains methods to install NuGet packages into projects.

**IVsPackageInstallerEvents**: Offers events which raise when packages are installed and uninstalled in the current solution.

**IVsPackageInstallerServices**: Contains a method to retrieve installed packages in the current solution, as well as methods to check if a specified package Id is installed in a project.

**IVsPackageUninstaller**: Contains methods to uninstall NuGet packages from projects.

**IConsoleInitializer**: Trigger eager initialization of the NuGet Package Manager Console.

**IVsTemplateWizard**: This interface is designed for project/item templates to include pre-installed packages, and is *not* meant to be invoked from code.

Detailed API references is available below.

## Using NuGet Services

**Step 1**

Install the **NuGet.VisualStudio** nuget package into your project. The [NuGet.VisualStudio](https://nuget.org/packages/NuGet.VisualStudio) package contains the NuGet.VisualStudio.dll assembly which includes all the above interfaces.

<div class="block-callout-info">
    <strong>Note:</strong><br>
    When installed, the package will automatically set the **Embed Interop Types** property of the assembly reference to **True**. The reason it does so is to make your code  resilient against version changes when users update to newer versions of NuGet.
</div>

<div class="block-callout-warning">
    <strong>Note:</strong><br>
    You must NOT use any other types besides the above interfaces in your code. You must NOT reference any other NuGet assemblies either, including NuGet.Core.dll.
</div>


**Step 2**

With the package installed, you are ready to obtain those services from your code. Because they are exported as MEF contracts, you can import them either via MEF's Import attribute, or through the [IComponentModel service](http://msdn.microsoft.com/en-us/library/microsoft.visualstudio.componentmodelhost.icomponentmodel.aspx "IComponentModel interface") in code. 

Sample code snippet:

    var componentModel = (IComponentModel)GetService(typeof(SComponentModel));
    IVsPackageInstallerServices installerServices = componentModel.GetService<IVsPackageInstallerServices>();
    var installedPackages = installerServices.GetInstalledPackages();

## IVsPackageInstaller interface

    public interface IVsPackageInstaller
    {
        /// <summary>
        /// Installs a single package from the specified package source.
        /// </summary>
        /// <param name="source">The package source to install the package from.</param>
        /// <param name="project">The target project for package installation.</param>
        /// <param name="packageId">The package id of the package to install.</param>
        /// <param name="version">The version of the package to install</param>
        /// <param name="ignoreDependencies">A boolean indicating whether or not to ignore the package's dependencies during installation.</param>
        void InstallPackage(string source, Project project, string packageId, string version, bool ignoreDependencies);

        /// <summary>
        /// Installs a single package from the specified package source.
        /// </summary>
        /// <param name="repository">The package repository to install the package from.</param>
        /// <param name="project">The target project for package installation.</param>
        /// <param name="packageId">The package id of the package to install.</param>
        /// <param name="version">The version of the package to install</param>
        /// <param name="ignoreDependencies">A boolean indicating whether or not to ignore the package's dependencies during installation.</param>
        /// <param name="skipAssemblyReferences">A boolean indicating if assembly references from the package should be skipped.</param>
        void InstallPackage(IPackageRepository repository, Project project, string packageId, string version, bool ignoreDependencies, bool skipAssemblyReferences);

        /// <summary>
        /// Installs one or more packages that exist on disk in a folder defined in the registry.
        /// </summary>
        /// <param name="keyName">The registry key name (under NuGet's repository key) that defines the folder on disk containing the packages.</param>
        /// <param name="isPreUnzipped">A boolean indicating whether the folder contains packages that are pre-unzipped.</param>
        /// <param name="skipAssemblyReferences">A boolean indicating whether the assembly references from the packages should be skipped.</param>
        /// <param name="project">The target project for package installation.</param>
        /// <param name="packageVersions">A dictionary of packages/versions to install where the key is the package id and the value is the version.</param>
        /// <remarks>
        /// If any version of the package is already installed, no action will be taken.
        /// <para>
        /// Dependencies are always ignored.
        /// </para>
        /// </remarks>
        void InstallPackagesFromRegistryRepository(string keyName, bool isPreUnzipped, bool skipAssemblyReferences, Project project, IDictionary<string, string> packageVersions);

        /// <summary>
        /// Installs one or more packages that are embedded in a Visual Studio Extension Package.
        /// </summary>
        /// <param name="extensionId">The Id of the Visual Studio Extension Package.</param>
        /// <param name="isPreUnzipped">A boolean indicating whether the folder contains packages that are pre-unzipped.</param>
        /// <param name="skipAssemblyReferences">A boolean indicating whether the assembly references from the packages should be skipped.</param>
        /// <param name="project">The target project for package installation</param>
        /// <param name="packageVersions">A dictionary of packages/versions to install where the key is the package id and the value is the version.</param>
        /// <remarks>
        /// If any version of the package is already installed, no action will be taken.
        /// <para>
        /// Dependencies are always ignored.
        /// </para>
        /// </remarks>
        void InstallPackagesFromVSExtensionRepository(string extensionId, bool isPreUnzipped, bool skipAssemblyReferences, Project project, IDictionary<string, string> packageVersions);
    }


## IVsPackageInstallerEvents interface

    /// <summary>
    /// Contains events which are raised when packages are installed or uninstalled from projects and the current solution.
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

## IVsPackageInstallerServices interface

    /// <summary>
    /// Contains methods to query for installed packages within the current solution.
    /// </summary>
    public interface IVsPackageInstallerServices
    {
        /// <summary>
        /// Get the list of NuGet packages installed in the current solution.
        /// </summary>
        IEnumerable<IVsPackageMetadata> GetInstalledPackages();

        /// <summary>
        /// Checks if a NuGet package with the specified Id is installed in the specified project.
        /// </summary>
        /// <param name="project">The project to check for NuGet package.</param>
        /// <param name="id">The id of the package to check.</param>
        /// <returns><c>true</c> if the package is install. <c>false</c> otherwise.</returns>
        bool IsPackageInstalled(Project project, string id);

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
        bool IsPackageInstalledEx(Project project, string id, string versionString);

        /// <summary>
        /// Get the list of NuGet packages installed in the specified project.
        /// </summary>
        /// <param name="project">The project to get NuGet packages from.</param>
        IEnumerable<IVsPackageMetadata> GetInstalledPackages(Project project);
    }

## IVsPackageUninstaller interface

    /// <summary>
    /// Contains methods to uninstall packages from a project within the current solution.
    /// </summary>
    public interface IVsPackageUninstaller
    {
        /// <summary>
        /// Uninstall the specified package from a project and specify whether to uninstall its dependency packages too.
        /// </summary>
        /// <param name="project">The project from which the package is uninstalled.</param>
        /// <param name="packageId">The package to be uninstalled</param>
        /// <param name="removeDependencies">A boolean to indicate whether the dependency packages should be uninstalled too.</param>
        void UninstallPackage(Project project, string packageId, bool removeDependencies);
    }

## IVsPackageMetadata interface

	/// <summary>
    /// Contains information about an installed package.
    /// </summary>
    public interface IVsPackageMetadata
    {
        /// <summary>
        /// Id of the package.
        /// </summary>
        string Id { get; }

        /// <summary>
        /// The version of the package.
        /// </summary>
        string VersionString { get; }

        /// <summary>
        /// Title of the package.
        /// </summary>
        string Title { get; }

        /// <summary>
        /// Description of the package.
        /// </summary>
        string Description { get; }

        /// <summary>
        /// The authors of the package.
        /// </summary>
        IEnumerable<string> Authors { get; }

        /// <summary>
        /// The location where the package is installed on disk.
        /// </summary>
        string InstallPath { get; }
    }
