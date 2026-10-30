

```
"cwd": "${workspaceFolder}/SimpleCSharpConsoleApp",
// For more information about the 'console' field, see https://aka.ms/VSCode-CS-LaunchJson-Console
"console": "integratedTerminal",
"stopAtEntry": false
},
{
  "name": ".NET Core Attach",
  "type": "coreclr",
  "request": "attach",
  "processId": "${command:pickProcess}"
}
]
```

## Finding the .NET Core and C# Documentation

C# 和 .NET Core 的文档都非常优秀，可读性很强，并且充满了有用的信息。鉴于预定义 .NET 类型数量巨大 [数量达数千种]，你必须愿意撸起袖子深入查阅提供的文档。你可以在以下位置查看所有 Microsoft 文档：

https://docs.microsoft.com/en-us/dotnet/csharp/

本书前半部分你会使用最多的区域是 C# 文档和 .NET Core 文档，位于以下位置：

https://docs.microsoft.com/en-us/dotnet/csharp/
https://docs.microsoft.com/en-us/dotnet/core/

## Summary

本章的目的是为你提供使用 .NET 5 SDK 和运行时设置开发环境的信息，以及对 Visual Studio 2019 Community Edition 和 Visual Studio Code 的介绍。如果你只对构建跨平台 .NET Core 应用程序感兴趣，你有许多选择。Visual Studio [仅限 Windows]、Visual Studio for Mac [仅限 Mac] 和 Visual Studio Code [跨平台] 均由 Microsoft 提供。构建 WPF 或 WinForms 应用程序仍需要在 Windows 机器上使用 Visual Studio。

## PART II

## Core C# Programming

## CHAPTER 3

## Core C# Programming Constructs, Part 1

本章通过介绍一系列独立的小主题，开始你对 C# 编程语言的正式研究，这些主题是你探索 .NET Core Framework 时必须熟悉的。首先要了解如何构建程序的应用程序对象，并检查可执行程序入口点的组成：Main() 方法以及 C# 9.0 的新特性 top-level statements。接下来，你将研究基本的 C# 数据类型 [及其在 System 命名空间中的等效类型]，包括对 System.String 和 System.Text.StringBuilder 类的检查。

在了解了基本 .NET Core 数据类型的细节后，你将研究多种数据类型转换技术，包括窄化操作、扩展操作以及 checked 和 unchecked 关键字的用法。

本章还将探讨 C# var 关键字的角色，它允许你隐式定义局部变量。正如你将在本书后面看到的那样，隐式类型在处理 LINQ 技术集时非常有帮助，如果不是偶尔强制要求的话。你将通过快速检查允许你使用各种循环和决策结构控制应用程序流程的 C# 关键字和运算符来结束本章。

### Breaking Down a Simple C# Program

C# 要求所有程序逻辑都包含在类型定义中 [请回顾第 1 章，type 是指 {class, interface, structure, enumeration, delegate} 集合中的成员这一通用术语]。与许多其他语言不同，在 C# 中不可能创建全局函数或全局数据点。相反，所有数据成员和方法都必须包含在类型定义中。为了开始，创建一个名为 Chapter3_AllProject.sln 的新空解决方案，其中包含一个名为 SimpleCSharpApp 的 C# 控制台应用程序。

从 Visual Studio，在“创建新项目”屏幕上选择 Blank Solution 模板。解决方案打开后，在解决方案资源管理器中右键单击解决方案并选择 Add ► New Project。选择“C# console app”模板，将其命名为 SimpleCSharpApp，然后单击 Create。记得将目标框架更新为 net5.0。

从命令行执行以下操作：

```
dotnet new sln -n Chapter3_AllProjects

dotnet new console -lang c# -n SimpleCSharpApp -o .\SimpleCSharpApp -f net5.0
dotnet sln .\Chapter3_AllProjects.sln add .\SimpleCSharpApp
```

你可能会同意初始 Program.cs 文件中的代码相当乏味。

```
using System;

namespace SimpleCSharpApp
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Hello World!");
        }
    }
}
```

鉴于此，使用以下代码语句更新你的 Program 类的 Main() 方法：

```
class Program
{
    static void Main(string[] args)
    {
        // Display a simple message to the user.
        Console.WriteLine("***** My First C# App *****");
        Console.WriteLine("Hello World!");
        Console.WriteLine();

        // Wait for Enter key to be pressed before shutting down.
        Console.ReadLine();
    }
}
```

> 注 C# 是区分大小写的编程语言。因此，Main 与 main 不同，Readline 与 ReadLine 不同。请注意，所有 C# 关键字都是小写 [例如，public, lock, class, dynamic]，而命名空间、类型和成员名称 [按惯例] 以大写字母开头，嵌入词的首字母也大写 [例如，Console.WriteLine, System.Windows.MessageBox, System.Data.SqlClient]。作为经验法则，每当你收到关于“未定义符号”的编译器错误时，请先检查拼写和大小写！

之前的代码包含一个类类型的定义，该类支持一个名为 Main() 的方法。默认情况下，Visual Studio 将定义 Main() 的类命名为 Program；但是，如果你愿意，你可以更改它。在 C# 9.0 之前，每个可执行 C# 应用程序 [控制台程序、Windows 桌面程序或 Windows 服务] 必须包含一个定义 Main() 方法的类，该方法用于表示应用程序的入口点。

正式地说，定义 Main() 方法的类被称为应用程序对象。一个可执行应用程序可能有多个应用程序对象 [这在执行单元测试时很有用]，但编译器必须知道应使用哪个 Main() 方法作为入口点。这可以通过项目文件中的 <StartupObject> 元素或位于 Visual Studio 项目属性窗口应用程序选项卡上的 Startup Object 下拉列表框来完成。

请注意，Main() 的签名装饰有 static 关键字，该关键字将在第 5 章中详细讨论。目前，只需理解 static 成员是作用域到类级别 [而不是对象级别]，因此可以在无需先创建新的类实例的情况下调用它们。

除了 static 关键字之外，此 Main() 方法有一个参数，它是一个字符串数组 [string[] args]。虽然你目前没有处理这个数组，但此参数可能包含任意数量的传入命令行参数 [你将很快看到如何访问它们]。最后，此 Main() 方法被设置为 void 返回值，这意味着在退出方法作用域之前，你不使用 return 关键字显式定义返回值。

Program 类的逻辑在 Main() 中。这里，你使用了在 System 命名空间中定义的 Console 类。其成员之一是静态 WriteLine()，顾名思义，它将文本字符串和回车发送到标准输出。你还调用 Console.ReadLine() 以确保由 Visual Studio IDE 启动的命令提示符保持可见。在使用 Visual Studio 运行 .NET Core 控制台应用程序 [调试或发布模式] 时，控制台窗口默认保持可见。此行为可以通过启用“调试停止时自动关闭控制台”设置来更改，该设置位于 Tools ► Options ► Debugging 下。Console.ReadLine 方法是为了在通过 Windows 资源管理器双击产品 *.exe 文件执行程序时保持窗口打开。你将很快了解更多关于 System.Console 类的信息。

### Using Variations of the Main() Method (Updated 7.1)

默认情况下，Visual Studio 会生成一个 Main() 方法，该方法具有 void 返回值和一个字符串类型数组作为单个输入参数。这并不是 Main() 的唯一可能形式。然而，允许使用以下任何签名构建应用程序的入口点 [假设它包含在 C# 类或结构定义中]：

```
// int return type, array of strings as the parameter.
static int Main(string[] args)
{
    // Must return a value before exiting!
    return 0;
}
```

```
// No return type, no parameters.
static void Main()
{
}
```

```
// int return type, no parameters.
static int Main()
{
    // Must return a value before exiting!
    return 0;
}
```

随着 C# 7.1 的发布，Main() 方法现在可以是异步的。异步编程在第 15 章中介绍，但现在请注意还有另外四种签名。

```
static Task Main()
static Task<int> Main()
static Task Main(string[])
static Task<int> Main(string[])
```

