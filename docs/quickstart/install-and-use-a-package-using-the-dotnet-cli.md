---
title: Install and use a NuGet package with the dotnet CLI
description: Get a quick tutorial on how to use the dotnet CLI to install and use a NuGet package in a .NET project.
author: JonDouglas
ms.author: jodou
ms.date: 08/21/2023
ms.topic: quickstart
---

# Quickstart: Install and use a package with the dotnet CLI

NuGet packages contain compiled binary code that developers make available for other developers to use in their projects. For more information, see [What is NuGet](../What-is-NuGet.md). This quickstart describes how to install the popular [Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json) NuGet package into a .NET project by using the [dotnet add package](/dotnet/core/tools/dotnet-add-package) command.

You refer to installed packages in code with a `using <namespace>` directive, where `<namespace>` is often the package name. You can then use the package's API in your project.

> [!Tip]
> Browse [nuget.org/packages](https://nuget.org/packages) to find packages you can reuse in your own applications. You can search directly at [https://nuget.org](https://nuget.org/packages), or find and install packages from within Visual Studio. For more information, see [Find and evaluate NuGet packages for your project](../consume-packages/finding-and-choosing-packages.md).

## Prerequisites

- The [.NET SDK](https://www.microsoft.com/net/download), which provides the `dotnet` command-line tool. Starting in Visual Studio 2017, the dotnet CLI automatically installs with any .NET or .NET Core related workloads.

## Create a project

You can install NuGet packages into a .NET project. For this walkthrough, create a simple .NET console project by using the dotnet CLI, as follows:

1. Create a folder named *Nuget.Quickstart* for the project.

1. Open a command prompt and switch to the new folder.

1. Create the project by using the following command:

    ```dotnetcli
    dotnet new console
    ```

1. Use `dotnet run` to test the app. You should see the output `Hello, World!`.

## Add the Newtonsoft.Json NuGet package

1. Use the following command to install the `Newtonsoft.json` package:

    ```dotnetcli
    dotnet add package Newtonsoft.Json
    ```

2. After the command completes, open the *Nuget.Quickstart.csproj* file in Visual Studio to see the added NuGet package reference:

    ```xml
    <ItemGroup>
      <PackageReference Include="Newtonsoft.Json" Version="13.0.1" />
    </ItemGroup>
    ```

## Use the Newtonsoft.Json API in the app

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
            public string Name { get; set; }
            public string Email { get; set; }
            public DateTime DOB { get; set; }
        }
        internal class Program
        {
            static void Main(string[] args)
            {
                Account account = new Account
                {
                    Name = "John Doe",
                    Email = "john@nuget.org",
                    DOB = new DateTime(1980, 2, 20, 0, 0, 0, DateTimeKind.Utc),
                };
    
                string json = JsonConvert.SerializeObject(account, Formatting.Indented);
                Console.WriteLine(json);
            }
        }
    }
    ```

1. Save the file, then build and run the app by using the `dotnet run` command. The output is the JSON representation of the `Account` object in the code:

    ```output
    {
      "Name": "John Doe",
      "Email": "john@nuget.org",
      "DOB": "1980-02-20T00:00:00Z"
    }
    ```

Congratulations on installing and using your first NuGet package!

## Related video

> [!VIDEO https://learn.microsoft.com/shows/NuGet-101/Install-and-Use-a-NuGet-Package-with-the-NET-CLI-3-of-5/player]

Find more NuGet videos on [Channel 9](/shows/NuGet-101/) and [YouTube](https://www.youtube.com/playlist?list=PLdo4fOcmZ0oVLvfkFk8O9h6v2Dcdh2bh_).

## Next steps

Learn more about installing and using NuGet packages with the dotnet CLI:

> [!div class="nextstepaction"]
> [Install and use packages by using the dotnet CLI](../consume-packages/install-use-packages-dotnet-cli.md)

- [Overview and workflow of package consumption](../consume-packages/overview-and-workflow.md)
- [Find and choose packages](../consume-packages/finding-and-choosing-packages.md)
- [Package references in project files](../consume-packages/package-references-in-project-files.md)
