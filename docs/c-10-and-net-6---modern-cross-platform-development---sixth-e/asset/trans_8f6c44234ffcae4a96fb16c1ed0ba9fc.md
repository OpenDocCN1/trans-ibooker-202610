
```
IL_0027: call string [System.Runtime.Extensions]
    System.IO.Path::GetFullPath(string) /* 0A000037 */
IL_002c: stloc.0
IL_002d: ldloc.0
IL_002e: call string [System.Runtime.Extensions]
    System.IO.Path::GetDirectoryName(string) /* 0A000038 */
IL_0033: stloc.1
IL_0034: ldloc.1
IL_0035: brtrue.s IL_0039
IL_0037: ldnull
IL_0038: ret
IL_0039: ldloc.1
IL_003a: newobj instance void
    System.IO.DirectoryInfo::.ctor(string) /* 06000097 */
IL_003f: ret
} // end of method Directory::GetParent
```

![](img/c63b00dca7192cc58cfb9e1f1e64f483_331_0.png)

**良好实践：** IL 代码编辑窗口除非你对 C# 和 .NET 开发非常深入，了解 C# 编译器如何将源代码翻译成 IL 代码很重要，否则并不特别有用。更有用的编辑窗口包含由微软专家编写的等效 C# 源代码。你可以通过查看专业人士如何实现类型来学习很多良好实践。例如，GetParent 方法展示了如何检查参数是否为 null 以及其他参数异常。

- 12. 不保存更改的情况下关闭编辑窗口。
- 13. 在 **资源管理器** 中，在 **ILSPY 反编译成员** 中，右键单击程序集并选择 **卸载程序集**。

## 不，你在技术上无法阻止反编译

我有时会被问到是否有办法保护编译后的代码以防止反编译。简短的回答是否定的，如果你想想，你会明白为什么必须如此。你可以使用像 **Dotfuscator** 这样的混淆工具使其更困难，但最终你无法完全阻止它。

所有编译后的应用程序都包含对运行它的平台、操作系统和硬件的指令。这些指令在功能上必须与原始源代码相同，只是对人类来说更难阅读。这些指令必须可读才能执行你的代码；因此它们也必须可读才能被反编译。如果你使用某种自定义技术来保护代码不被反编译，那么你也会阻止你的代码运行！

虚拟机模拟硬件，因此可以捕获正在运行的应用程序与其认为正在运行的软件和硬件之间的所有交互。

如果你能保护你的代码，那么你也无法附加调试器并逐步执行它。如果编译后的应用程序有 pdb 文件，那么你可以附加调试器并逐行逐步执行语句。即使没有 pdb 文件，你仍然可以附加调试器并对代码的工作方式有所了解。

这适用于所有编程语言。不仅仅是 .NET 语言如 C#、Visual Basic 和 F#，也包括 C、C++、Delphi、汇编语言：所有这些都可以附加调试器或被反汇编或反编译。专业人士使用的一些工具显示在下表中：

| 类型 | 产品 | 描述 |
|---|---|---|
| 虚拟机 | VMware | 专业人员如恶意软件分析师总是会在虚拟机内部运行软件。 |
| 调试器 | SoftICE | 通常在虚拟机中在操作系统下方运行。 |
| 调试器 | WinDbg | 对于理解 Windows 内部结构很有用，因为它比其他调试器更了解 Windows 数据结构。 |
| 反汇编器 | IDA Pro | 由专业恶意软件分析师使用。 |
| 反编译器 | HexRays | 反编译 C 应用。IDA Pro 的插件。 |
| 反编译器 | DeDe | 反编译 Delphi 应用。 |
| 反编译器 | dotPeek | 来自 JetBrains 的 .NET 反编译器。 |

![](img/c63b00dca7192cc58cfb9e1f1e64f483_332_0.png)

> **良好实践：** 调试、反汇编和反编译别人的软件很可能违反其许可协议，并且在许多司法管辖区是非法的。与其试图用技术解决方案来保护你的知识产权，有时法律是你唯一的救济途径。

## 为 NuGet 分发打包你的库

在学习如何创建和打包我们自己的库之前，我们将回顾项目如何使用现有包。

## 引用 NuGet 包

假设你想添加由第三方开发者创建的包，例如 Newtonsoft.Json，这是一个用于 JavaScript Object Notation（JSON）序列化格式的流行包：

- 1. 在 `AssembliesAndNamespaces` 项目中，添加对 `Newtonsoft.Json` NuGet 包的引用，要么使用 Visual Studio 2022 的 GUI，要么使用 Visual Studio Code 的 `dotnet add package` 命令。
- 2. 打开 `AssembliesAndNamespaces.csproj` 文件并注意已添加包引用，如以下标记所示：

```xml
<ItemGroup>
  <PackageReference Include="newtonsoft.json" Version="13.0.1" />
</ItemGroup>
```

如果你有更新版本的 newtonsoft.json 包，那么自本书编写以来它已被更新。

## 修复依赖项

为了始终如一地还原包并编写可靠的代码，修复依赖项很重要。修复依赖项意味着你使用的是为特定版本的 .NET 发布的一系列包，例如 .NET 6.0 的 SQLite，如以下标记中突出显示所示：

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

要修复依赖项，每个包都应具有单一版本且没有附加限定符。附加限定符包括测试版（beta1）、发布候选版（rc4）和通配符（*）。

通配符允许自动引用和使用未来版本，因为它们始终代表最新发布。但通配符因此很危险，因为它们可能导致使用未来不兼容的包，从而破坏你的代码。

在编写书籍时这可能是值得冒的风险，因为新预览版本每月发布，你不想不断更新包引用，就像我在 2021 年所做的那样，如以下标记所示：

```xml
<PackageReference
  Include="Microsoft.EntityFrameworkCore.Sqlite"
  Version="6.0.0-preview.*" />
```

如果你使用 dotnet add package 命令或 Visual Studio 的管理 NuGet 包，那么它默认会使用包的最新特定版本。但如果你从博客文章复制粘贴配置或手动添加引用，你可能会包含通配符限定符。

以下依赖项是未修复的 NuGet 包引用的示例，因此除非你了解其影响，否则应避免：

```xml
<PackageReference Include="System.Net.Http" Version="4.1.0-*" />
<PackageReference Include="Newtonsoft.Json" Version="12.0.3-beta1" />
```

> 良好实践：微软保证如果你将依赖项修复为随特定版本的 .NET 发行的一起发布的，例如 6.0.0，这些包将全部协同工作。几乎总是修复你的依赖项。

## 为 NuGet 打包库

现在，让我们打包你之前创建的 SharedLibrary 项目：

- 1. 在 SharedLibrary 项目中，将 Class1.cs 文件重命名为 StringExtensions.cs。
- 2. 修改其内容以提供一些使用正则表达式验证各种文本值的有用扩展方法，如以下代码所示：

```
using System.Text.RegularExpressions;

namespace Packt.Shared
{
    public static class StringExtensions
    {
        public static bool IsValidXmlTag(this string input)
        {
            return Regex.IsMatch(input, @"^<([a-z]+)([^<>]+)(?:>(.*)</\1>|\s+/>)$");
        }

        public static bool IsValidPassword(this string input)
        {
            // minimum of eight valid characters
            return Regex.IsMatch(input, "^[a-zA-Z0-9_-]{8,}$");
        }

        public static bool IsValidHex(this string input)
        {
            // three or six valid hex number characters
            return Regex.IsMatch(input, "^#?([a-fA-F0-9]{3}|[a-fA-F0-9]{6})$");
        }
    }
}
```
