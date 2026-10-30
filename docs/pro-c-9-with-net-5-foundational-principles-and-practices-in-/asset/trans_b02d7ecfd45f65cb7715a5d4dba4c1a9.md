

设置好引用后，构建解决方案并查看目标目录（bin\Debug\net5.0）中的输出，你会看到目录中有 CarLibrary.dll，而没有 CarLibrary.nupkg 文件。这是因为 .NET Core 会解包内容并将包含的程序集作为直接引用添加进来。

现在将其中一个客户端设为启动项目并运行应用程序，它的工作方式与之前完全相同。接下来，将 CarLibrary 的版本号更新为 1.0.0.4，并重新打包它。在 Publish 目录中，现在有两个 CarLibrary NuGet 包。如果重新运行 add package 命令，项目将被更新为使用新版本。如果更喜欢旧版本，add package 命令允许为特定包添加版本号。

## Publishing Console Applications (Updated .NET 5)

现在你有了 C# CarClient 应用程序（及其相关的 CarLibrary 程序集），如何将其发布给用户呢？将应用程序及其相关依赖项打包起来称为 *publishing*。发布 .NET Framework 应用程序需要在目标机器上安装框架，而 .NET Core 应用程序也可以以类似的方式发布，称为 *framework-dependent* 部署。然而，.NET Core 应用程序也可以发布为 *self-contained* 应用程序，完全不需要安装 .NET Core！

当以自包含方式发布应用程序时，必须指定目标运行时标识符。运行时标识符用于将应用程序打包给特定的操作系统。有关可用运行时标识符的完整列表，请参阅 .NET Core RID Catalog，网址为 https://docs.microsoft.com/en-us/dotnet/core/rid-catalog。

> **Note:** 发布 ASP.NET Core 应用程序是一个更复杂的过程，将在本书后面介绍。

### Publishing Framework-Dependent Applications

框架相关部署是 dotnet publish 命令的默认模式。要打包应用程序及其所需文件，只需使用 CLI 执行以下命令即可：

```
dotnet publish
```

> **Note:** publish 命令使用项目的默认配置，通常为 debug。

这会将应用程序及其支持文件（共 16 个文件）放置到 bin\Debug\net5.0\publish 目录中。检查添加到该目录的文件，你会看到两个 *.dll 文件（CarLibrary.dll 和 CSharpCarClient.dll），其中包含所有应用程序代码。需要提醒的是，CSharpCarClient.exe 文件是 dotnet.exe 的打包版本，已配置为启动 CSharpCarClient.dll。该目录中的其他文件是 .NET Core 文件，不属于 .NET Core 运行时。

要创建发布版本（将放置在 bin\Release\net5.0\publish 目录中），请输入以下命令：

```
dotnet publish -c release
```

### Publishing Self-Contained Applications

与框架相关部署类似，自包含部署包含所有应用程序代码和引用的程序集，但还包含应用程序所需的 .NET Core 运行时文件。要将应用程序作为自包含部署发布，请使用以下 CLI 命令（选择名为 selfcontained 的文件夹作为输出目标）：

```
dotnet publish -r win-x64 -c release -o selfcontained --self-contained true
```

> **Note:** 创建自包含部署时，需要运行时标识符，以便发布过程知道要连同应用程序代码一起包含哪些运行时文件。

这也会将应用程序及其支持文件（共 235 个文件）放置到 selfcontained 目录中。如果将这些文件复制到另一台 64 位 Windows 计算机上，即使未安装 .NET 5 运行时也可以运行该应用程序。

### Publishing Self-Contained Applications as a Single File

在大多数情况下，部署 235 个文件（对于一个只打印几行文本的应用程序）可能不是将应用程序分发给用户的最有效方式。幸运的是，.NET 5 大大改进了将应用程序和跨平台运行时文件发布为单个文件的能力。唯一未包含的文件是必须存在于单个 EXE 之外的本机库。

以下命令为 64 位 Windows 操作系统创建单个文件、自包含部署包，并将结果文件放置在名为 singlefile 的文件夹中。

```
dotnet publish -r win-x64 -c release -o singlefile --self-contained true -p:PublishSingleFile=true
```

当你检查创建的文件时，你会发现一个可执行文件（CSharpCarClient.exe）、一个调试文件（CSharpCarClient.pdb）和四个特定于操作系统的 DLL。虽然之前的发布过程产生了 235 个文件，但单文件版本的 CSharpCarClient.exe 大小为 54 MB！创建单文件发布将 235 个文件打包到单个文件中。文件数量减少所带来的好处是以文件大小为代价换来的。

最后说明，本机库可以与单文件一起打包。将 CSharpCarClient.csproj 文件更新为以下内容：

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

如果再次运行相同的命令，输出确实是单个文件。然而，这只是一种传输机制。本机文件将在应用程序在目标机器上执行时被提取到临时位置。

## How .NET Core Locates Assemblies

到目前为止，本书中构建的所有程序集都是直接相关的（除了你刚刚完成的 NuGet 示例）。你是在项目之间添加了项目引用或直接引用。在这些情况下（以及 NuGet 示例中），依赖程序集会直接复制到客户端应用程序的目标目录中。定位依赖程序集不是问题，因为它们与需要它们的应用程序位于磁盘的同一位置。

但是 .NET Core 框架呢？它们是如何定位的？以前版本的 .NET 会将框架文件安装到全局程序集缓存（GAC）中，所有 .NET 应用程序都知道如何定位框架文件。

然而，GAC 阻止了 .NET Core 的并排功能，因此没有单一的运行时和框架文件存储库。相反，构成框架的文件安装在 C:\Program Files\dotnet（在 Windows 上）中，按版本分隔。根据应用程序的版本（在 .csproj 文件中指定），必要的运行时和框架文件会从指定版本的目录中加载给应用程序。

具体来说，当启动运行时版本时，运行时主机提供一组探测路径，用于查找应用程序的依赖项。有五个探测属性（每个都是可选的），如表 16-1 所列。

**Table 16-1. Application Probing Properties**

| Option | Meaning in Life |
|---|---|
| TRUSTED_PLATFORM_ASSEMBLIES | List of platform and application assembly file paths |
| PLATFORM_RESOURCE_ROOTS | List of directory paths to search for satellite resource assemblies |
| NATIVE_DLL_SEARCH_DIRECTORIES | List of directory paths to search for unmanaged (native) libraries |
| APP_PATHS | List of directory paths to search for managed assemblies |
| APP_NI_PATHS | List of directory paths to search for native images of managed assemblies |

要查看这些属性的默认路径，创建一个名为 FunWithProbingPaths 的新 .NET Core 控制台应用程序。将顶层语句更新为以下内容：

```
using System;
using System.Linq;

Console.WriteLine("*** Fun with Probing Paths ***");
Console.WriteLine($"TRUSTED_PLATFORM_ASSEMBLIES: ");
//Use ':' on non-Windows platforms
var list = AppContext.GetData("TRUSTED_PLATFORM_ASSEMBLIES")
    .ToString().Split(';');

foreach (var dir in list)
{
    Console.WriteLine(dir);
}
Console.WriteLine();
Console.WriteLine($"PLATFORM_RESOURCE_ROOTS: {AppContext.GetData ("PLATFORM_RESOURCE_ROOTS")}");
Console.WriteLine();
```
