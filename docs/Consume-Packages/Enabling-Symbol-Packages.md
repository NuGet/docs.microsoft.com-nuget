# Enabling Symbol Packages

Package authors have the option to create [Symbol packages](/ndocs/create-packages/symbol-packages) for their libraries and upload them to the [SymbolSource repository](http://www.symbolsource.org/Public).

When you configure Visual Studio to use symbol packages in the debugger, it will automatically download an available symbol package when you install an associated package from nuget.org.

To enable this behavior, do the following:

1. Select **Tools > Options** on the menu and expand **Debugger > Symbols**.
2. In the right hand pane, select the folder icon on the upper right, and enter `https://nuget.smbsrc.net` (if you start typing `nuget` you'll be given an auto-complete option for this). Click OK when you're done.

	![Adding the NuGet symbol source to Visual Studio](/images/Consume/Symbols_01-AddingSource.png)

When a symbol package is available, you'll be able to step directly into package code within the Visual Studio debugger.

<div class="block-callout-info">
	<strong>Note</strong><br>
	This same feature allows you to step into the .NET Framework code. See http://referencesource.microsoft.com/. 
</div>

