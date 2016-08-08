# Transform Web.Config Using .transform Files

In NuGet's traditional way of configuration-file transformation, you add a file to your package's content and give it 
the same name as the file you want to transform, followed by a .transform extension. For example, 
to transform a web.config file, you create a web.config.transform file. The transformation file 
contains XML that looks like a web.config or app.config file, but it includes only the sections 
that need to be merged into the project's configuration file.

For example, suppose you want your package to add an item to the modules collection of the web.config 
file of the project that it's installed in. To do this, you add a file named web.config.transform to 
the package and put the following XML in it:

    <configuration>
        <system.webServer>
            <modules>
                <add name="MyNuModule" type="Sample.MyNuModule" />
            </modules>
        <system.webServer>
    </configuration>

Suppose the package is installed in a project that contains the following web.config file:

    <configuration>
        <system.webServer>
            <modules>
                <add name="MyModule" type="Sample.MyModule" />
            </modules>
        <system.webServer>
    </configuration>

After NuGet installs the package, the web.config file will look like the following example, in which the 
changed line is highlighted:

    <configuration>
        <system.webServer>
            <modules>
                <add name="MyModule" type="Sample.MyModule" />
                <add name="MyNuModule" type="Sample.MyNuModule" />
            </modules>
        <system.webServer>
    </configuration>

Notice that NuGet didn't replace the modules section, it just merged the new entry into it. When NuGet 
merges a transform file into a project's configuration file, it only adds elements or adds attributes 
to existing elements in the configuration file; it does not change existing elements or attributes in 
any other way.

As noted earlier, an example of a package that needs to change a project's web.config file during 
installation is ELMAH (Error Logging Modules and Handlers for ASP.NET). ELMAH requires that its 
HTTP modules and HTTP handlers be registered in the web.config file. The ELMAH package includes a 
file named web.config.transform that specifies how to register them.

![](/images/Create/web.config.transform.png)

Suppose the XML in the web.config.transform file looks like the following example:

    <configuration>
        <system.web>
            <httpModules>
                <add name="ErrorLog" type="Elmah.ErrorLogModule, Elmah" />
            </httpModules>
            <httpHandlers>
                <add verb="POST,GET,HEAD" path="elmah.axd"
                  type="Elmah.ErrorLogPageFactory, Elmah" />
            </httpHandlers>
        </system.web>
        <system.webServer>
            <validation validateIntegratedModeConfiguration="false" />
            <modules>
                <add name="ErrorLog" type="Elmah.ErrorLogModule, Elmah" />
            </modules>
            <handlers>
                <add name="Elmah" verb="POST,GET,HEAD" path="elmah.axd"
                  type="Elmah.ErrorLogPageFactory, Elmah" />
            </handlers>
        </system.webServer>
    </configuration>

Furthermore, suppose you install the ELMAH package in a project that has the following web.config file:

    <configuration> 
      <system.web> 
        <compilation debug="true" targetFramework="4.0"> 
          <assemblies> 
            <add assembly="System.Data.Entity, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" /> 
            <add assembly="System.Web.Entity, Version=4.0.0.0, Culture=neutral, PublicKeyToken=B77A5C561934E089" /> 
          </assemblies> 
        </compilation> 
      </system.web> 
      <system.webServer> 
          <modules runAllManagedModulesForAllRequests="true"> 
              <add name="ErrorLog" /> 
          </modules> 
          <handlers> 
              <add name="Elmah" verb="HEAD" path="myOwnElmah.axd" type="My.Elmah, Elmah"/> 
          </handlers> 
      </system.webServer> 
    </configuration>

After you install the package, the **web.config** file looks like the following example 
(the changes are highlighted):

    <configuration> 
      <system.web> 
        <compilation debug="true" targetFramework="4.0"> 
          <assemblies> 
            <add assembly="System.Data.Entity, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" /> 
            <add assembly="System.Web.Entity, Version=4.0.0.0, Culture=neutral, PublicKeyToken=B77A5C561934E089" /> 
          </assemblies> 
        </compilation> 
    <httpModules> 
          <add name="ErrorLog" type="Elmah.ErrorLogModule, Elmah" /> 
        </httpModules> 
        <httpHandlers> 
          <add verb="POST,GET,HEAD" path="elmah.axd" type="Elmah.ErrorLogPageFactory, Elmah" /> 
        </httpHandlers> 
      </system.web> 
      <system.webServer> 
        <modules runAllManagedModulesForAllRequests="true"> 
          <add name="ErrorLog" type="Elmah.ErrorLogModule, Elmah" /> 
        </modules> 
        <handlers> 
          <add name="Elmah" verb="HEAD" path="myOwnElmah.axd" type="My.Elmah, Elmah" /> 
          <add name="Elmah" verb="POST,GET,HEAD" path="elmah.axd" type="Elmah.ErrorLogPageFactory, Elmah" /> 
        </handlers> 
        <validation validateIntegratedModeConfiguration="false" /> 
      </system.webServer> 
    </configuration>

The following changes have been made:

* `<httpModules>` and `<httpHandlers>` elements have been added under the `<system.web>` element.
* A type attribute has been added to the ErrorLog item in the `<system.webserver/modules>` collection. 
NuGet found an element with one matching attribute and one new one in the transformation file, so it 
added the attribute to the existing element. Note that when the package is removed, it will appear to 
NuGet that this element was added, so the entire element will be removed from the project's web.config file.
* A new Elmah item has been added to the `<system.webserver/handlers>` collection. NuGet found an element 
with one matching element and other non-matching elements, so it added the new element rather than change 
the existing one.
* A validation element has been added under the `<system.webserver>` element.
