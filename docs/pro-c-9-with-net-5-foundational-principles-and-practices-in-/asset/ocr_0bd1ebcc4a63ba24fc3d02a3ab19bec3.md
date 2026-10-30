# Publishing Self-Contained Applications

Like framework-dependent deploys, self-contained deployments include all your application code and referenced assemblies, but also include the .NET Core Runtime files required by your application. To publish your application as a self-contained deployment, use the following CLI command (selecting a folder named selfcontained as the output destination):

```
dotnet publish -r win-x64 -c release -o selfcontained --self-contained true
```

> **Note** When creating self-contained deploys, the runtime identifier is required, so the publish process knows what runtime files to include along with your application code.

This also places your application and its supporting files (235 files in total) into the selfcontained directory. If you copied these files to another 64-bit Windows computer, you can run the application even if the .NET 5 runtime isn’t installed.

# Publishing Self-Contained Applications as a Single File

In most situations, deploying 235 files (for an application that prints a few lines of text) is probably not the most effective way to get your application out to users. Fortunately, .NET 5 greatly improved the ability to publish your application and the cross-platform runtime files into a single file. The only files that aren’t included are native libraries that must exist outside of the singe EXE.

The following command creates a single-file, self-contained deployment package for 64-bit Windows operating systems and places the resulting files in a folder named singlefile.

```
dotnet publish -r win-x64 -c release -o singlefile --self-contained true -p:PublishSingleFile=true
```

When you examine files that were created, you will find a single executable (CSharpCarClient.exe), a debug file (CSharpCarClient.pdb), and four operating-specific DLLs. While the previous publish process produced 235 files, the single file version of CSharpCarClient.exe clocks in at 54 MB! Creating the single file publication packed the 235 files into the single file. What was made up for in file count reduction was traded for in file size.

As a final note, the native libraries can be packaged with the single file. Update the CSharpCarClient.csproj file to the following:

```
<Project Sdk="Microsoft.NET.Sdk">
  <ItemGroup>
    <PackageReference Include="CarLibrary" Version="1.0.0.3" />
  </ItemGroup>
  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>net5.0</TargetFramework>
    <IncludeNativeLibrariesForSelfExtract>true</IncludeNativeLibrariesForSelfExtract>
  </PropertyGroup>
</Project>
```

If you run the same command again, the output is indeed a single file. However, this is only a transport mechanism. The native files will be extracted to a temporary location on the target machine when the application is executed.