```
<OutputType>Exe</OutputType>
<TargetFramework>net6.0</TargetFramework>
<Nullable>enable</Nullable>
<ImplicitUsings>enable</ImplicitUsings>
</PropertyGroup>

<ItemGroup>
  <PackageReference
    Include="Microsoft.Extensions.Configuration"
    Version="6.0.0" />
  <PackageReference
    Include="Microsoft.Extensions.Configuration.Binder"
    Version="6.0.0" />
  <PackageReference
    Include="Microsoft.Extensions.Configuration.FileExtensions"
    Version="6.0.0" />
  <PackageReference
    Include="Microsoft.Extensions.Configuration.Json"
    Version="6.0.0" />
</ItemGroup>

</Project>
```

2. Add a file named appsettings.json to the Instrumenting project folder.

3. Modify appsettings.json to define a setting named PacktSwitch with a Level value, as shown in the following code:

```
{
  "PacktSwitch": {
    "Level": "Info"
  }
}
```

4. In Visual Studio 2022, in Solution Explorer, right-click appsettings.json, select Properties, and then in the Properties window, change Copy to Output Directory to Copy if newer. This is necessary because unlike Visual Studio Code, which runs the console app in the project folder, Visual Studio runs the console app in Instrumenting\bin\Debug\net6.0 or Instrumenting\bin\Release\net6.0.

5. At the top of Program.cs, import the Microsoft.Extensions.Configuration namespace.

6. Add some statements to the end of Program.cs to create a configuration builder that looks in the current folder for a file named appsettings.json, build the configuration, create a trace switch, set its level by binding to the configuration, and then output the four trace switch levels, as shown in the following code:

```
ConfigurationBuilder builder = new();

builder.SetBasePath(Directory.GetCurrentDirectory())
```