#Use a Package

This tutorial walks you through installing a popular Json framework ([Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json/)) for a Universal Windows Platform (UWP) project.

<div class="block-callout-info">
	<strong>Note:</strong><br>
	You need to have Visual Studio 2015 Update 3 with Tools for Universal Windows Apps installed to follow this tutorial. If you don't have Visual Studio already, you can download <a href="https://developer.microsoft.com/en-us/windows/downloads">Visual Studio Community 2015</a> for free.
</div>

##Create a new UWP project
In Visual Studio, choose **File**, **New**, **Project**. In the **New Project** dialog, expand the **Visual C#** node, then expand the **Windows** node and choose **Universal**. Create a **Blank App (Universal Windows)**.

![Create new Project](/images/ConsumeNugetSample/01.PNG)


Accept the default values for Target Version and Minimum Version and click ok.

![Target and Minimum versions](/images/ConsumeNugetSample/02.PNG)


##Add a NuGet package
In the Solution Explorer, right click on **References** and in the context menu, choose **Manage NuGet Packages**.

![References context](/images/ConsumeNugetSample/03.PNG)


Choose "nuget.org" as the **Package source**, and select the **Browse** tab. Search for **Newtonsoft.Json** and click **Install**.

![Browse nuget packages](/images/ConsumeNugetSample/04.PNG)


Review changes and click ok.

![Browse nuget packages](/images/ConsumeNugetSample/05.PNG)

<div class="block-callout-warning">
	<a href="http://nuget.org">nuget.org</a> likely already has a package that could make application development easy for you. Be sure to search for packages inside Visual Studio or on <a href="http://nuget.org">nuget.org</a> for existing packages.
</div>

##Build the solution
In the **Build** menu, choose **Build Solution**. Building the solution also restores NuGet packages.



##Utilize the package
Now that we have added the Newtonsoft.json nuget Package, we will use JsonConvert.SerializeObject, a method from **Newtonsoft.Json**, to convert an object to a JSON string and display it.


From the Solution Explorer, open MainPage.xaml and copy the following lines of code, replacing the existing `Grid` element:

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <StackPanel VerticalAlignment="Center">
            <Button Click="Button_Click" Content="Click Me" />
            <TextBlock Name="TextBlock" Text="TextBlock" />
        </StackPanel>
    </Grid>


Expand MainPage.xaml, open MainPage.xaml.cs and copy the following line of code in the `MainPage` class (not the constructor):

    public class Account
    {
        public string Name { get; set; }
        public string Email { get; set; }
        public DateTime DOB { get; set; }
    }

    private void Button_Click(object sender, RoutedEventArgs e)
    {
        Account account = new Account
        {
            Name = "John Doe",
            Email = "john@microsoft.com",
            DOB = new DateTime(1980, 2, 20, 0, 0, 0, DateTimeKind.Utc),
        };
        string json = JsonConvert.SerializeObject(account, Formatting.Indented);
        TextBlock.Text = json;
    }


You will see a red squiggle under **JsonConvert**. Even though we added the Newtonsoft.json nuget package, we need to add a using statement to this class. With your cursor on **JsonConvert**, click the Lightbulb and then click show potential fixes.

![Lightbulb fix](/images/ConsumeNugetSample/06.PNG)


Select the first suggested fix, **using Newtonsoft.Json;**

![Lightbulb fix](/images/ConsumeNugetSample/07.PNG)



##Run the app
Press F5, or go to the **Debug** menu and select **Start Debugging**. After building, the app opens up.

![Output](/images/ConsumeNugetSample/08.PNG)


Click on the button and the contents of the TextBlock are replaced with the json string.

![Output](/images/ConsumeNugetSample/09.PNG)


That's it!
You can repeat these steps to install any NuGet package for any project type.


##Related Reading
* [NuGet 3.0, .NET and project.json](/ndocs/consume-packages/projectjson-intro)
* [Dependency Resolution in NuGet v3 / project.json](/ndocs/consume-packages/projectjson-dependency)
* [NuGet Configuration File](/ndocs/consume-packages/nuget-config-file-overview)
* [NuGet Config File Defaults](/ndocs/consume-packages/nuget-config-file-defaults)
