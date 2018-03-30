#### Windows
1. Visit [nuget.org/downloads](https://nuget.org/downloads) and select NuGet 3.3 or higher (2.8.6 is not compatible with Mono). The latest version is always recommended, and 4.1.0+ is required to publish packages to nuget.org.
2. Each download is the `nuget.exe` file directly. Instruct your browser to save the file to a folder of your choice. The file is *not* an installer; you won't see anything if you run it directly from the browser.
3. Add the folder where you placed `nuget.exe` to your PATH environment variable to use the CLI tool from anywhere.

#### macOS/Linux
Behaviors may vary slightly by OS distribution. From a shell prompt:

1. Install [Mono 4.4.2 or later](http://www.mono-project.com/docs/getting-started/install/).
2. Execute the following commands to download the latest stable `nuget.exe` to `/usr/local/bin`, give the file permissions to execute, and create a symbolic link so `.exe` is not required when running the CLI:
    
    ```bash
    sudo curl -o /usr/local/bin/nuget.exe https://dist.nuget.org/win-x86-commandline/latest/nuget.exe
    sudo chmod 755 /usr/local/bin/nuget.exe
    sudo ln -s /usr/local/bin/nuget.exe /usr/local/bin/nuget
    ```
3. Test the installation by entering `nuget` with no parameters. NuGet CLI help should display.