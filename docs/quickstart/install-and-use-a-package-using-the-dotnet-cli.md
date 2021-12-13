---
title: Install and use a NuGet package using the dotnet CLI
description: A walkthrough tutorial on the process of installing and using a NuGet package in a .NET Core project.
author: JonDouglas
ms.author: jodou
ms.date: 01/23/2018
ms.topic: quickstart
---

# Quickstart: Install and use a package using the dotnet CLI

NuGet packages contain reusable code that other developers make available to you for use in your projects. See [What is NuGet?](../What-is-NuGet.md) for background. Packages are installed into a .NET Core project using the `dotnet add package` command as described in this article for the popular [Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json/) package.

Once installed, refer to the package in code with `using <namespace>` where \<namespace\> is specific to the package you're using. You can then use the package's API.

> [!Tip]
> **Start with nuget.org**: Browsing nuget.org is how .NET developers typically find components they can reuse in their own applications. You can search nuget.org directly or find and install packages within Visual Studio as shown in this article.

## Prerequisites

- The [.NET Core SDK](https://www.microsoft.com/net/download/), which provides the `dotnet` command-line tool. Starting in Visual Studio 2017, the dotnet CLI is automatically installed with any .NET Core related workloads.

## Create a project

NuGet packages can be installed into a .NET project of some kind. For this walkthrough, create a simple .NET Core console project as follows:

1. Create a folder for the project.

1. Open a command prompt and switch to the new folder.

1. Create the project using the following command:

    ```dotnetcli
    dotnet new console
    ```

1. Use `dotnet run` to test that the app has been created properly.

## Add the Newtonsoft.Json NuGet package

1. Use the following command to install the `Newtonsoft.json` package:

    ```dotnetcli
    dotnet add package Newtonsoft.Json
    ```

2. After the command completes, open the `.csproj` file to see the added reference:

    ```xml
    <ItemGroup>
      <PackageReference Include="Newtonsoft.Json" Version="12.0.1" />
    </ItemGroup>
    ```

## Use the Newtonsoft.Json API in the app

1. Open the `Program.cs` file and add the following line at the top of the file:

    ```cs
    using Newtonsoft.Json;
    ```

1. Add the following code before the `class Program` line:

    ```cs
    public class Account
    {
        public string Name { get; set; }
        public string Email { get; set; }
        public DateTime DOB { get; set; }
    }
    ```

1. Replace the `Main` function with the following:

    ```cs
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
    ```

1. Build and run the app by using the `dotnet run` command. The output should be the JSON representation of the `Account` object in the code:

    ```output
    {
      "Name": "John Doe",
      "Email": "john@nuget.org",
      "DOB": "1980-02-20T00:00:00Z"
    }
    ```
## Related video

> [!Video https://docs.microsoft.com/shows/NuGet-101/Install-and-Use-a-NuGet-Package-with-the-NET-CLI-3-of-5/player]

Find more NuGet videos on [Channel 9](/shows/NuGet-101/) and [YouTube](https://www.youtube.com/playlist?list=PLdo4fOcmZ0oVLvfkFk8O9h6v2Dcdh2bh_).

## Next steps

Congratulations on installing and using your first NuGet package!

> [!div class="nextstepaction"]
> [Install and use packages using the dotnet CLI](../consume-packages/install-use-packages-dotnet-cli.md)

To explore more that NuGet has to offer, select the links below.

- [Overview and workflow of package consumption](../consume-packages/overview-and-workflow.md)
- [Finding and choosing packages](../consume-packages/finding-and-choosing-packages.md)
- [Package references in project files](../consume-packages/package-references-in-project-files.md)
