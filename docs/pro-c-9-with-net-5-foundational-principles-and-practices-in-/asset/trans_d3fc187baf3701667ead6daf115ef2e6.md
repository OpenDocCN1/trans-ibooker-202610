
下面展示了给定 .NET Core 程序集的内部 CLR 头信息：

```
Dump of file CarLibrary.dll
File Type: DLL

    clr Header:

        48 cb
        2.05 runtime version
        2158 [ B7C] RVA [size] of MetaData Directory
            1 flags
                IL Only
        0 entry point token
        0 [    0] RVA [size] of Resources Directory
        0 [    0] RVA [size] of StrongNameSignature Directory
        0 [    0] RVA [size] of CodeManagerTable Directory
        0 [    0] RVA [size] of VTableFixups Directory
        0 [    0] RVA [size] of ExportAddressTableJumps Directory
        0 [    0] RVA [size] of ManagedNativeHeader Directory

Summary

        2000 .reloc
        2000 .rsrc
        2000 .text
```

再说一次，作为 .NET Core 开发人员，你无需关心程序集 CLR 头信息的繁琐细节。只需了解每个 .NET Core 程序集都包含这些数据，当映像数据加载到内存时，.NET Core 运行时会幕后使用这些数据。现在，请你把注意力转向一些在日常编程任务中更有用的信息。

## CIL 代码、类型元数据和程序集清单

本质上，程序集包含 CIL 代码，如你所记得的，CIL 是一种与平台和 CPU 无关的中间语言。在运行时，内部的 CIL 会由即时（JIT）编译器根据平台和 CPU 特定的指令即时编译。基于这种设计，.NET Core 程序集确实可以在多种架构、设备和操作系统上执行。（即使你不了解 CIL 编程语言的细节，也能愉快而富有成效地生活；不过，第 19 章仍将介绍 CIL 的语法和语义。）

程序集还包含元数据，这些元数据完整描述了程序集所包含类型的格式，以及该程序集引用的外部类型的格式。.NET Core 运行时使用这些元数据来解析二进制文件中类型（及其成员）的位置、在内存中布局类型，并促进远程方法调用。在第 17 章讲解反射服务时，你将了解 .NET 元数据格式的细节。

程序集还必须包含关联的清单（也称为程序集元数据）。清单记录了程序集中的每个模块，确定了程序集的版本，并记录了当前程序集引用的任何外部程序集。在本章中，你将看到 CLR 在定位外部程序集引用的过程中广泛使用了程序集的清单。

## 可选程序集资源

最后，.NET Core 程序集可以包含任意数量的嵌入资源，例如应用程序图标、图像文件、声音片段或字符串表。实际上，.NET Core 平台支持只包含本地化资源的附属程序集。如果你希望基于特定区域性（英语、德语等）划分资源以构建国际化软件，这会非常有用。构建附属程序集的主题不在本书范围之内。如果你感兴趣，请查阅 .NET Core 文档了解附属程序集和本地化的信息。

## 类库与控制台应用程序

到目前为止，本书中的示例几乎全是 .NET Core 控制台应用程序。如果你是一位当前正在阅读本书的 .NET 开发人员，它们类似于 .NET 控制台应用程序，主要区别在于配置过程（将在后面介绍），当然，还有它们运行在 .NET Core 上。控制台应用程序有单一入口点（要么是指定的 Main() 方法，要么是顶级语句），可以与控制台交互，并且可以由操作系统直接启动。.NET Core 与 .NET 控制台应用程序的另一个区别是，.NET Core 中的控制台应用程序使用 .NET Core 应用程序主机（dotnet.exe）启动。

另一方面，类库没有入口点，因此不能直接启动。它们用于封装逻辑、自定义类型等，并被其他类库和/或控制台应用程序引用。换句话说，类库用于包含“.NET Core 程序集的作用”一节中讨论的内容。

## .NET Standard 与 .NET Core 类库

.NET Core 类库在 .NET Core 上运行，.NET 类库在 .NET 上运行。非常简单明了。然而，这也带来一个问题。假设你的组织有一个庞大的 .NET 代码库，你和你的团队可能已经在此基础上开发了多年。很可能你和你的团队多年来构建的应用程序使用了大量共享代码。也许是集中式日志记录、报告或特定领域的功能。

现在，你（和你的组织）希望将所有新开发迁移到 .NET Core。那所有这些共享代码怎么办？将所有遗留代码重写为 .NET Core 程序集的工作量可能相当大，而且在你所有的应用程序都迁移到 .NET Core 之前，你将不得不维护两个版本（一个 .NET 版本和一个 .NET Core 版本）。这会让生产力戛然而止。

幸运的是，.NET Core 的建设者已经考虑过这种情况。.NET Standard 是随 .NET Core 引入的一种新型类库项目，可以被 .NET 和 .NET Core 应用程序引用。不过，在你满怀期待之前，.NET (Core) 5 有一个问题。稍后会详细介绍。

每个 .NET Standard 版本都定义了一组通用 API，所有 .NET 版本（.NET、.NET Core、Xamarin 等）都必须支持这些 API 才能符合该标准。例如，如果你将一个类库构建为 .NET Standard 2.0 项目，它可以被 .NET 4.6.1+ 和 .NET Core 2.0+ 引用（此外还有各种版本的 Xamarin、Mono、通用 Windows 平台和 Unity）。

这意味着你可以将代码从 .NET 类库迁移到 .NET Standard 2.0 类库中，并且这些类库可以被 .NET Core 和 .NET 应用程序共享。这比维护同一代码的重复副本（每个框架一份）要好得多。

现在说说这个问题。每个 .NET Standard 版本都代表其所支持框架的最低公共标准。这意味着版本越低，你在类库中能做的事情就越少。虽然 .NET (Core) 5 和 .NET Core 3.1 都可以引用 .NET Standard 2.0 库，但你无法在 .NET Standard 2.0 库中使用大量的 C# 8.0（或任何 C# 9.0）功能。要获得完整的 C# 8.0 和 C# 9.0 支持，你必须使用 .NET Standard 2.1。而 .NET 4.8（原始 .NET Framework 的最新/最终版本）最高只支持到 .NET Standard 2.0。这仍然是在较新应用程序中利用现有代码的好机制，但并不是银弹。

## 配置应用程序

虽然可以把你 .NET Core 应用程序所需的所有信息都保存在源代码中，但在大多数重要的应用程序中，能够在运行时更改某些值至关重要。这通常通过随应用程序一起提供的配置文件来完成。

> 注意 以前的 .NET Framework 主要依赖名为 app.config（对于 ASP.NET 应用程序是 web.config）的 XML 文件。虽然基于 XML 的配置文件仍然可以使用，但配置 .NET Core 应用程序的主要方法是使用 JavaScript Object Notation（JSON）文件，如下节所示。配置将在 WPF 和 ASP.NET Core 相关章节中深入讨论。

为了说明这个过程，创建一个名为 FunWithConfiguration 的新 .NET Core 控制台应用程序，并向项目添加以下包引用：

```
dotnet new console -lang c# -n FunWithConfiguration -o .\FunWithConfiguration -f net5.0
dotnet add FunWithConfiguration package Microsoft.Extensions.Configuration.Json
```

这会向你的项目添加对基于 JSON 文件的 .NET Core 配置子系统（及其依赖项）的引用。要利用这一点，首先在项目中添加一个名为 appsettings.json 的新 JSON 文件。更新项目文件，确保在构建项目时该文件始终被复制到输出目录。

```
<ItemGroup>
    <None Update="appsettings.json">
        <CopyToOutputDirectory>Always</CopyToOutputDirectory>
    </None>
</ItemGroup>
```

最后，将文件更新为以下内容：

```
{
  "CarName": "Suzy"
}
```

> 注意 如果你不熟悉 JSON，它是一种名称-值对格式，每个对象都用花括号括起来。整个文件可以被读作一个单独的对象，子对象也用花括号标记。在本书后面，你将处理更复杂的 JSON 文件。

最后一步是读取配置文件并获取 CarName 值。将 Program.cs 中的 using 语句更新为以下内容：

```
using System;
using System.IO;
using Microsoft.Extensions.Configuration;
```

