#Troubleshooting NuGet cache issues

## NuGet Locals Command

NuGet locals Command is used to clear local NuGet cache(s). This document contains the most common errors messages encountered while running nuget locals command either from nuget.exe, dotnet cli or Visual Studio Package Manager settings.

## Frequenty encountered errors in NuGet locals

1. **Error**: Clearing local resources failed: Unable to delete one or more files

	Platform(s) affected: NuGet.exe, dotnet cli and Visual Studio Package Manager settings.

	Possible Reason(s):

	* One or more of the files at or within the cache directories being deleted are being used by another process. Close the process and try again.
	* The user does not have delete permission on one or more files within the cache directories being deleted.

2. **Error**: The directory is not empty

	Platform(s) affected: NuGet.exe, dotnet cli and Visual Studio Package Manager settings.

	Possible Reason(s):

	* One or more of the files at or within the cache directories being deleted are being used by another process. Close the process and try again.
	* The user does not have delete permission on one or more files within the cache directories being deleted.


3. **Error**: An invalid local resource name was provided. Please provide one of the following values: http-cache, temp, global-packages, all.

	Platform(s) affected: NuGet.exe, dotnet cli.

	Possible Reason(s):
	
	* The argument provided is not recognized as one of the known cache locations. Please check the argument provided.
	* Since NuGet no longer uses package-cache, starting from 3.6.x it is not supported by the locals command. Please do not provide package-cache as an argument.