```
```xml
<ItemGroup>
  <PackageReference Include="newtonsoft.json" Version="13.0.1" />
</ItemGroup>
```
```

If you have a more recent version of the newtonsoft.json package, then it has been updated since this chapter was written.

# Fixing dependencies

To consistently restore packages and write reliable code, it's important that you fix dependencies. Fixing dependencies means you are using the same family of packages released for a specific version of .NET, for example, SQLite for .NET 6.0, as shown highlighted in the following markup:

```
```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>net6.0</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference
      Include="Microsoft.EntityFrameworkCore.Sqlite"
      Version="6.0.0" />
  </ItemGroup>

</Project>
```
```

To fix dependencies, every package should have a single version with no additional qualifiers. Additional qualifiers include betas (beta1), release candidates (rc4), and wildcards (*).

Wildcards allow future versions to be automatically referenced and used because they always represent the most recent release. But wildcards are therefore dangerous because they could result in the use of future incompatible packages that break your code.

This can be worth the risk while writing a book where new preview versions are released every month and you do not want to keep updating the package references, as I did during 2021, and as shown in the following markup:

```
```xml
<PackageReference
  Include="Microsoft.EntityFrameworkCore.Sqlite"
  Version="6.0.0-preview.*" />
```
```

If you use the dotnet add package command, or Visual Studio's Manage NuGet Packages, then it will by default use the latest specific version of a package. But if you copy and paste configuration from a blog article or manually add a reference yourself, you might include wildcard qualifiers.