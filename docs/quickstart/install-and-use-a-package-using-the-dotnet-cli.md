---
title: Install and Use a NuGet Package with the dotnet CLI
description: In this quickstart, find out how to use the dotnet command-line interface (CLI) to install and use a NuGet package in a .NET project.
author: JonDouglas
ms.author: jodou
ms.date: 04/17/2026
ms.topic: quickstart
# customer intent: As a developer, I want to find out how to use the dotnet CLI to install and use a NuGet package in a .NET project so that I can take advantage of available code.
---

# Quickstart: Install and use a package with the dotnet CLI

In this quickstart, you install the popular [`Newtonsoft.Json`](https://www.nuget.org/packages/Newtonsoft.Json) NuGet package into a .NET project. NuGet packages contain compiled binary code that developers make available for other developers to use in their projects. For more information, see [An introduction to NuGet](../what-is-nuget.md).

To install the package, you use the [dotnet package add](/dotnet/core/tools/dotnet-package-add) command, which is part of the dotnet command-line interface (CLI).

> [!Tip]
> Browse [nuget.org/packages](https://www.nuget.org/packages) to find packages you can reuse in your own applications. You can search directly at [https://nuget.org](https://www.nuget.org/packages), or you can find and install packages from within Visual Studio. For more information, see [Find and evaluate NuGet packages for your project](../consume-packages/Finding-and-Choosing-Packages.md).

## Prerequisites

- Visual Studio 2026. You can install the 2026 Community edition for free from [visualstudio.microsoft.com](https://visualstudio.microsoft.com/), or you can use the Professional or Enterprise edition.
- The [.NET SDK](https://dotnet.microsoft.com/download), which provides the dotnet CLI. In Visual Studio, the dotnet CLI automatically installs with any .NET-related workloads.

## Create a project

You can install NuGet packages into a .NET project. For this quickstart, take the following steps to create a basic .NET console project by using the dotnet CLI:

1. Create a folder named *Nuget.Quickstart* for the project.

1. Open a command prompt window and go to the new folder.

1. Create the project by using the following command:

   ```dotnetcli
   dotnet new console
   ```

1. Use `dotnet run` to test the app. The command writes the following output to the screen: `Hello, World!`.

## Add the Newtonsoft.Json NuGet package

1. Use the following command to install the `Newtonsoft.Json` package:

   ```dotnetcli
   dotnet package add Newtonsoft.Json
   ```

   If you're using .NET 9 or earlier, use the verb-first form instead:

   ```dotnetcli
   dotnet add package Newtonsoft.Json
   ```

1. After the command finishes, open the *Nuget.Quickstart.csproj* file in Visual Studio to check for the added NuGet package reference:

   ```xml
   <ItemGroup>
     <PackageReference Include="Newtonsoft.Json" Version="13.0.4" />
   </ItemGroup>
   ```

## Use the Newtonsoft.Json API in the app

In code, you refer to installed packages by using a `using <namespace>` directive, where `<namespace>` is often the package name. You can then use the package's API in your project.

1. In Visual Studio, open the *Program.cs* file and add the following line at the top of the file:

   ```cs
   using Newtonsoft.Json;
   ```

1. Add the following code to replace the `Console.WriteLine("Hello, World!");` statement:

   ```cs
   namespace Nuget.Quickstart
   {
       public class Account
       {
           public string? ID { get; set; }
           public decimal Balance { get; set; }
           public DateTime Created { get; set; }
       }
       internal class Program
       {
           static void Main(string[] args)
           {
               Account account = new Account
               {
                   ID = "A1bC2dE3fH4iJ5kL6mN7oP8qR9sT0u",
                   Balance = 4389.21m,
                   Created = new DateTime(2026, 4, 16, 0, 0, 0, DateTimeKind.Utc),
               };

               string json = JsonConvert.SerializeObject(account, Formatting.Indented);
               Console.WriteLine(json);
           }
       }
   }
   ```

1. Save the file, and then build and run the app by using the `dotnet run` command. The output is the JSON representation of the `Account` object in the code:

   ```output
   {
     "ID": "A1bC2dE3fH4iJ5kL6mN7oP8qR9sT0u",
     "Balance": "4389.21",
     "Created": "2026-04-16T00:00:00Z"
   }
   ```

## Related videos

> [!VIDEO https://learn-video.azurefd.net/vod/player?show=dotnet-package-management-with-nuget-for-beginners&ep=installing-a-nuget-package-using-the-dotnet-cli-nuget-for-beginners]

For videos about using NuGet for package management, see [.NET Package Management with NuGet for Beginners](/shows/dotnet-package-management-with-nuget-for-beginners/) and [NuGet for Beginners](https://www.youtube.com/playlist?list=PLdo4fOcmZ0oVLvfkFk8O9h6v2Dcdh2bh_).

## Related content

To find out more about installing and using NuGet packages by using the dotnet CLI, see the following articles:

- [Install and manage NuGet packages with the dotnet CLI](../consume-packages/install-use-packages-dotnet-cli.md)
- [Package consumption workflow](../consume-packages/Overview-and-Workflow.md)
- [Find and evaluate NuGet packages for your project](../consume-packages/Finding-and-Choosing-Packages.md)
- [`PackageReference` in project files](../consume-packages/Package-References-in-Project-Files.md)
