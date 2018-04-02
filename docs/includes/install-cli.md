#### Windows
1. Visit [nuget.org/downloads](https://nuget.org/downloads) and select NuGet 3.3 or higher (2.8.6 is not compatible with Mono). The latest version is always recommended, and 4.1.0+ is required to publish packages to nuget.org.
2. Each download is the `nuget.exe` file directly. Instruct your browser to save the file to a folder of your choice. The file is *not* an installer; you won't see anything if you run it directly from the browser.
3. Add the folder where you placed `nuget.exe` to your PATH environment variable to use the CLI tool from anywhere.

#### macOS/Linux
Behaviors may vary slightly by OS distribution.

1. Install [Mono 4.4.2 or later](http://www.mono-project.com/docs/getting-started/install/).
2. Execute the following commands at a shell prompt:
    
    ```bash
    # Download the latest stable `nuget.exe` to `/usr/local/bin`
    sudo curl -o /usr/local/bin/nuget.exe https://dist.nuget.org/win-x86-commandline/latest/nuget.exe
    # Give the file permissions to execute
    sudo chmod 755 /usr/local/bin/nuget.exe
    ```
3. Create an alias by adding the following script to the appropriate file for your OS (typically `~/.bash_aliases` or `~/.bash_profile`):
    
    ```bash
    # Create as alias for nuget
    alias nuget="mono /usr/local/bin/nuget.exe"
    ```
4. Reload the shell.  Test the installation by entering `nuget` with no parameters. NuGet CLI help should display.