It’s still a good mechanism for leveraging existing code in newer applications, but not a silver bullet.

# Configuring Applications

While it is possible to keep all the information needed by your .NET Core application in the source code, being able to change certain values at runtime is vitally important in most applications of significance. This is typically done through a configuration file that is shipped with your application.

> Note The previous .NET Framework relied mostly on XML files named app.config (or web.config for ASP.NET applications). While XML-based configuration files can still be used, the main method for configuring .NET Core applications is with JavaScript Object Notation (JSON) files as shown in this section. Configuration will be addressed in depth in the chapters in WPF and ASP.NET Core.

To illustrate the process, create a new .NET Core Console application named FunWithConfiguration and add the following package reference to your project:

```
dotnet new console -lang c# -n FunWithConfiguration -o .\FunWithConfiguration -f net5.0
dotnet add FunWithConfiguration package Microsoft.Extensions.Configuration.Json
```

This adds a reference for the JSON file-based .NET Core configuration subsystem (and its dependencies) into your project. To leverage this, first add a new JSON file into your project named appsettings.json. Update the project file to make sure the file is always copied to the output directory when the project is built.

```
<ItemGroup>
    <None Update="appsettings.json">
        <CopyToOutputDirectory>Always</CopyToOutputDirectory>
    </None>
</ItemGroup>
```

Finally, update the file to match the following:

```
{
  "CarName": "Suzy"
}
```

> Note If you are not familiar with JSON, it is a name-value pair format with each object enclosed in curly braces. The entire file can be read as a single object, and subobjects are also marked with curly braces. Later in this book, you will work with more complex JSON files.

The final step is you read in the configuration file and get the CarName value. Update the using statements in Program.cs to the following:

```
using System;
using System.IO;
using Microsoft.Extensions.Configuration;
```