Once the references are set, build the solution and look at the output in the target directories (bin\Debug\net5.0), and you will see the CarLibrary.dll in the directory and not the CarLibrary.nupkg file. This is because .NET Core unpacks the contents and adds in the assemblies contained as direct references.

Now set one of the clients as the startup project and run the application, and it works exactly like before. Next, update the version number of CarLibrary to 1.0.0.4, and repackage it. In the Publish directory, there are now two CarLibrary NuGet packages. If you rerun the add package commands, the project will be updated to use the new version. If the older version was preferred, the add package command allows for adding version numbers for a specific package.

# Publishing Console Applications (Updated .NET 5)

Now that you have your C# CarClient application (and its related CarLibrary assembly), how do you get it out to your users? Packaging up your application and its related dependencies is referred to as *publishing*. Publishing .NET Framework applications required the framework to be installed on the target machine, and .NET Core applications can also be published in a similar manner, referred to as a *framework-dependent* deployment. However, .NET Core applications can also be published as a *self-contained* application, which doesn’t require .NET Core to be installed at all!

When publishing applications as self-contained, you must specify the target runtime identifier. The runtime identifier is used to package your application for a specific operating system. For a full list of available runtime identifiers, see the .NET Core RID Catalog at https://docs.microsoft.com/en-us/dotnet/core/rid-catalog.

Note Publishing ASP.NET Core applications is a more involved process and will be covered later in this book.

# Publishing Framework-Dependent Applications

Framework-dependent deployments is the default mode for the dotnet publish command. To package your application and the required files, all you need to execute with the CLI is the following command:

```
dotnet publish
```

Note The publish command uses the default configuration for your project, which is typically debug.

This places your application and its supporting files (16 files in total) into the bin\Debug\net5.0\publish directory. Examining the files that were added to that directory, you see the two *.dll files (CarLibrary.dll and CSharpCarClient.dll) that contain all the application code. As a reminder, the CSharpCarClient.exe file is a packaged version of dotnet.exe that is configured to launch CSharpCarClient.dll. The additional files in the directory are .NET Core files that are not part of the .NET Core Runtime.

To create a release version (which will be placed in the bin\release\net5.0\publish directory), enter the following command:

```
dotnet publish -c release
```