#Use a package

This tutorial walks you through installing and using the popular [Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json/) package in a Universal Windows Platform (UWP) project:

- [Install pre-requisites](#install-pre-requisites)
- [Create a new UWP project](#create-a-new-uwp-project)
- [Add the Newtonsoft.Json NuGet package](#add-the-newtonsoft-json-nuget-package)
- [Use the Newtonsoft.Json API in the app](#use-the-newtonsoft-json-api-in-the-app)

You'll use a similar same workflow for virtually every NuGet package you use in a project.

<div class="block-callout-info">
    <strong>Start with nuget.org</strong><br>
    Installing packages from <a href="http://nuget.org">nuget.org</a> is a very common workflow that .NET developers use to find components they can reuse in their own applications. You can always search nuget.org directly or find and install packages within Visual Studio as we'll do here.
</div>


##Install pre-requisites
This tutorial requires Visual Studio 2015 Update 3 with Tools for Universal Windows Apps. 

You can install the Community edition for free from [visualstudio.com](https://www.visualstudio.com/) or use the Professional or Enterprise editions. The UWP tools option can be selected through the Custom install option during setup, checking the box under **Windows and Web Development > Universal Windows App Development Tools**. If you already have Visual Studio installed, you can run the installer again and click **Modify** to add the UWP tools.


##Create a new UWP project
In Visual Studio, choose **File > New > Project**, expand **Visual C# > Windows > Universal**, select the **Blank App (Universal Windows)**, and click OK. Accept the default values for Target Version and Minimum Version when prompted.

![Creating a new UWP project](/images/ConsumeNugetSample/QS_Use-01-NewProject.png)


##Add the Newtonsoft.Json NuGet package

1. In Solution Explorer, right click on **References** and choose **Manage NuGet Packages**.

	![Manage NuGet Packages command for project References](/images/ConsumeNugetSample/QS_Use-02-ManageNuGetPackages.png)

2. Choose "nuget.org" as the **Package source**, click the **Browse** tab, search for **Newtonsoft.Json**, select that package in the list, and click **Install**:

	![Locating Newtonsoft.Json package](/images/ConsumeNugetSample/QS_Use-03-NewtonsoftJson.png)

3. If prompted to review changes, click OK.

4. Right-click the solution in Solution Explorer and click **Build Solution**. This restore anys NuGet packages listed under **References**. For more details, see [Package Restore](/ndocs/consume-packages/package-restore).



##Use the Newtonsoft.Json API in the app

With the Newtonsoft.Json package in the project, you can call its `JsonConvert.SerializeObject` method to convert an object to a human-readable string.

1. Open MainPage.xaml and replace the existing `Grid` element with the following:
		
		<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
		    <StackPanel VerticalAlignment="Center">
		        <Button Click="Button_Click" Content="Click Me" Margin="10"/>
		        <TextBlock Name="TextBlock" Text="TextBlock" Margin="10"/>
		    </StackPanel>
		</Grid>	

2. Expand MainPage.xaml, open MainPage.xaml.cs, and insert the following code inside the `MainPage` class, after the constructor:
		
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

3. Even though you added the Newtonsoft.Json package to the project, you'll still see a red squiggle under `JsonConvert` because you need a `using` statement. Hover over the underlined `JsonConvert` and you'll see the Lightbulb and the option to **Show potential fixes**:

    ![Lightbulb with show potential fixes command](/images/ConsumeNugetSample/QS_Use-04-ShowPotentialFixes.png)


4. Click on **Show potential fixes** and select the first suggested fix, `using Newtonsoft.Json;`. This adds the necessary line to the top of the file.

	![Lightbulb giving option to add a using statement](/images/ConsumeNugetSample/QS_Use-05-AddUsing.png)

5. Build and run the app by pressing F5 or selecting **Debug > Start Debugging**:

	![Initial output of the app](/images/ConsumeNugetSample/QS_Use-06-AppStart.png)

6. Click on the button to see the contents of the TextBlock replaced with some JSON text:

	![Output of the app after clicking the button](/images/ConsumeNugetSample/QS_Use-07-AppEnd.png)



##Related topics
* [Overview and workflow of package consumption](/ndocs/consume-packages/overview-and-workflow)
* [Finding and choosing](/ndocs/consume-packages/finding-and-choosing-packages)
* [Configuring NuGet Behavior](/ndocs/consume-packages/configuring-nuget-behavior)
