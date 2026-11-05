
- **ASP.NET Core SignalR** 服务。
- **控制台应用程序** 命令行接口。

现代 .NET 在 Windows 上对以下类型的应用程序有不错的支持，因此它们是移植的潜在候选者：

- **Windows Forms** 应用程序。
- **Windows Presentation Foundation (WPF)** 应用程序。

现代 .NET 对跨平台桌面和移动设备上的以下类型应用程序有良好的支持：

- **Xamarin** 用于移动 iOS 和 Android 的应用。
- **.NET MAUI** 用于桌面 Windows 和 macOS，或移动 iOS 和 Android。

现代 .NET 不支持以下类型的旧版 Microsoft 项目：

- **ASP.NET Web Forms** 网站。这些最好使用 **ASP.NET Core Razor Pages** 或 **Blazor** 重新实现。
- **Windows Communication Foundation (WCF)** 服务（但有一个名为 **CoreWCF** 的开源项目，您可以根据需求使用）。WCF 服务最好使用 **ASP.NET Core gRPC** 服务重新实现。
- **Silverlight** 应用程序。这些最好使用 **.NET MAUI** 重新实现。

Silverlight 和 ASP.NET Web Forms 应用程序永远无法移植到现代 .NET，但现有的 Windows Forms 和 WPF 应用程序可以移植到 Windows 上的 .NET，以便受益于新的 API 和更快的性能。

目前在 .NET Framework 上的旧版 ASP.NET MVC Web 应用程序和 ASP.NET Web API Web 服务可以移植到现代 .NET，然后托管在 Windows、Linux 或 macOS 上。

## Should you port?

即使你 *可以* 移植，*应该* 移植吗？你能获得什么好处？一些常见的好处包括以下内容：

- **部署到 Linux、Docker 或 Kubernetes 用于网站和 Web 服务**：这些操作系统作为网站和 Web 服务平台轻量且经济高效，尤其与更昂贵的 Windows Server 相比。
- **移除对 IIS 和 System.Web.dll 的依赖**：即使你继续部署到 Windows Server，ASP.NET Core 也可以托管在轻量级、更高性能的 Kestrel（或其他）Web 服务器上。
- **命令行工具**：开发人员和管理员用于自动化任务的工具通常构建为控制台应用程序。能够跨平台运行单个工具非常有用。

## Differences between .NET Framework and modern .NET

以下是三个关键区别，如下表所示：

| Modern .NET | .NET Framework |
|---|---|
| 以 NuGet 包形式分发，因此每个应用程序都可以与其所需的 .NET 版本的应用程序本地副本一起部署。 | 以系统范围共享的程序集集合形式分发（字面上在全局程序集缓存 (GAC) 中）。 |
| 拆分为小型分层组件，因此可以执行最小化部署。 | 单一、整体式部署。 |
| 移除了旧技术，例如 ASP.NET Web Forms，以及非跨平台功能，例如 AppDomains、.NET Remoting 和二进制序列化。 | 除了与现代 .NET 中类似的一些技术（如 ASP.NET Core MVC）外，它还保留了一些旧技术，例如 ASP.NET Web Forms。 |

## Understanding the .NET Portability Analyzer

Microsoft 有一个有用的工具，您可以针对现有应用程序运行它以生成移植报告。您可以在以下链接观看该工具的演示：https://channel19.msdn.com/Blogs/Seth-Juarez/A-Brief-Look-at-the-NET-Portability-Analyzer。

## Understanding the .NET Upgrade Assistant

Microsoft 将遗留项目升级到现代 .NET 的最新工具是 .NET Upgrade Assistant。

就我本职工作而言，我在一家名为 Optimizely 的公司工作。我们有一个基于 .NET Framework 的企业级数字体验平台 (DXP)，包含内容管理系统 (CMS) 和用于构建数字商务网站的功能。Microsoft 需要一个具有挑战性的迁移项目来设计和测试 .NET Upgrade Assistant，因此我们与他们合作构建了这个很棒的工具。

目前，它支持以下 .NET Framework 项目类型，之后还将添加更多类型：

- ASP.NET MVC
- Windows Forms
- WPF
- Console Application
- Class Library

它作为全局 dotnet 工具安装，如以下命令所示：

```
dotnet tool install -g upgrade-assistant
```

您可以在以下链接阅读有关此工具的更多信息以及如何使用它：

https://docs.microsoft.com/en-us/dotnet/core/porting/upgrade-assistant-overview

## Using non-.NET Standard libraries

大多数现有 NuGet 包都可以用现代 .NET 使用，即使它们不是为 .NET Standard 或像 .NET 6 这样的现代版本编译的。如果您发现某个包在 nuget.org 网页上未正式支持 .NET Standard，您不必放弃。您应该尝试一下，看看是否有效。

例如，有一个由 Dialect Software LLC 创建的用于处理矩阵的自定义集合包，记录在以下链接：

https://www.nuget.org/packages/DialectSoftware.Collections.Matrix/

该包最后更新于 2013 年，那时远在 .NET Core 或 .NET 6 之前，因此该包是为 .NET Framework 构建的。只要像这样的程序集包仅使用 .NET Standard 中可用的 API，它就可以在现代 .NET 项目中使用。

让我们尝试使用它，看看是否有效：

- 1. 在 AssembliesAndNamespaces 项目中，添加 Dialect Software 包的包引用，如以下标记所示：
- 2. 构建 AssembliesAndNamespaces 项目以还原包。
- 3. 在 Program.cs 中，添加语句以导入 DialectSoftware.Collections 和 DialectSoftware.Collections.Generics 命名空间。
- 4. 添加语句以创建 Axis 和 Matrix<T> 的实例，为其填充值并输出它们，如以下代码所示：

```
<PackageReference
    Include="dialectsoftware.collections.matrix"
    Version="1.0.0" />
```

```
Axis x = new("x", 0, 10, 1);
Axis y = new("y", 0, 4, 1);

Matrix<long> matrix = new(new[] { x, y });

for (int i = 0; i < matrix.Axes[0].Points.Length; i++)
{
    matrix.Axes[0].Points[i].Label = "x" + i.ToString();
}

for (int i = 0; i < matrix.Axes[1].Points.Length; i++)
{
    matrix.Axes[1].Points[i].Label = "y" + i.ToString();
}

foreach (long[] c in matrix)
{
    matrix[c] = c[0] + c[1];
}

foreach (long[] c in matrix)
{
    WriteLine("{0},{1} ({2},{3}) = {4}",
        matrix.Axes[0].Points[c[0]].Label,
        matrix.Axes[1].Points[c[1]].Label,
        c[0], c[1], matrix[c]);
}
```

- 5. 运行代码，注意警告消息和结果，如以下输出所示：

```
warning NU1701: Package 'DialectSoftware.Collections.Matrix 1.0.0' was restored using '.NETFramework,Version=v4.6.1, .NETFramework,Version=v4.6.2, .NETFramework,Version=v4.7, .NETFramework,Version=v4.7.1, .NETFramework,Version=v4.7.2, .NETFramework,Version=v4.8' instead of the project target framework 'net6.0'. This package may not be fully compatible with your project.
x0,y0 (0,0) = 0
x0,y1 (0,1) = 1
x0,y2 (0,2) = 2
x0,y3 (0,3) = 3
...
```

即使该包是在 .NET 6 之前创建的，编译器和运行时也无法知道它是否有效，因此会显示警告，但由于它恰好只调用 .NET Standard 兼容的 API，所以它可以工作。

## Working with preview features

对于 Microsoft 来说，提供一些对 .NET 的许多部分都有跨领域影响的新功能，例如运行时、语言编译器和 API 库，是一项挑战。这是一个经典的先有鸡还是先有蛋的问题。你先做什么？

从实际角度来看，这意味着虽然 Microsoft 可能已完成了功能所需的大部分工作，但整个功能可能要到其现在年度的 .NET 发布周期非常晚的时候才准备就绪，太晚了，无法在“野外”进行适当的测试。

因此，从 .NET 6 开始，Microsoft 将在通用可用性 (GA) 版本中包含预览功能。开发人员可以启用这些预览功能并向 Microsoft 提供反馈。在以后的 GA 版本中，它们可以对所有人启用。

> **良好实践：** 预览功能不支持在生产代码中使用。
> 预览功能在最终发布前可能会有破坏性更改。
> 自行承担风险启用预览功能。

## Requiring preview features

[RequiresPreviewFeatures] 属性用于指示使用并且因此需要有关预览功能警告的程序集、类型或成员。然后代码分析器会扫描该程序集并在需要时生成警告。如果您的代码不使用任何预览功能，您将看不到任何警告。如果您使用任何预览功能，那么您的代码应该警告您的代码的消费者您使用预览功能。

## Enabling preview features

让我们看一个 .NET 6 中可用的预览功能的示例，即能够定义具有静态抽象方法的接口：

- 1. 使用您喜欢的代码编辑器在 Chapter07 解决方案/工作区中添加一个名为 `UsingPreviewFeatures` 的新控制台应用。
- 2. 在 Visual Studio Code 中，将 UsingPreviewFeatures 选择为活动的 OmniSharp 项目。当您看到弹出警告消息说缺少所需资源时，单击 **是** 以添加它们。
- 3. 在项目文件中，添加一个元素以启用预览功能，并添加一个元素以启用预览语言功能，如以下标记中突出显示所示：

```
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>net6.0</TargetFramework>
    <Nullable>enable</Nullable>
    <ImplicitUsings>enable</ImplicitUsings>
    <EnablePreviewFeatures>true</EnablePreviewFeatures>
    <LangVersion>preview</LangVersion>
  </PropertyGroup>
</Project>
```

- 4. 在 Program.cs 中，删除注释并静态导入 Console 类。

