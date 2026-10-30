

在幕后，编译器会填补空白。检查更新后代码生成的 IL，你会看到应用程序入口点的以下 TypeDef：

// TypeDef #1 (02000002)
// --------------------------------------------------
//   TypDefName: <Program>$  (02000002)
//   Flags      : [NotPublic] [AutoLayout] [Class] [Abstract] [Sealed] [AnsiClass]
//   [BeforeFieldInit]  (00100180)
//   Extends    : 0100000D [TypeRef] System.Object
//   Method #1 (06000001) [ENTRYPOINT]
// --------------------------------------------------
//               MethodName: <Main>$ (06000001)

将其与第 1 章中入口点的 TypeDef 进行比较：

// TypeDef #1 (02000002)
// --------------------------------------------------
//   TypDefName: CalculatorExamples.Program  (02000002)
//   Flags      : [NotPublic] [AutoLayout] [Class] [AnsiClass]
//   [BeforeFieldInit]  (00100000)
//   Extends    : 0100000C [TypeRef] System.Object
//   Method #1 (06000001) [ENTRYPOINT]
// --------------------------------------------------
//               MethodName: Main (06000001)

注意，对于第 1 章的示例，TypDefName 值显示为命名空间（CalculatorExamples）加上类名（Program），而 MethodName 值为 Main。在使用顶层语句的更新示例中，编译器已将 TypDefName 的值填充为 <Program>$，将方法名填充为 <Main>$。

## 指定应用程序错误代码（更新 9.0）

绝大多数你的 Main() 方法（或顶层语句）的返回值为 void，但能够返回 int（或 Task<int>）使 C# 与其他基于 C 的语言保持一致。根据惯例，返回值为 0 表示程序已成功终止，而其他值（如 -1）表示错误条件（请注意，即使你构造了一个原型为返回 void 的 Main() 方法，值 0 也会自动返回）。

当使用顶层语句（因此没有 Main() 方法）时，如果执行代码返回一个整数，则该整数即为返回码。如果没有显式返回，仍然返回 0，与显式使用 Main() 方法的情况相同。

在 Windows 操作系统中，应用程序的返回值存储在名为 %ERRORLEVEL% 的系统环境变量中。如果你创建了一个以编程方式启动另一个可执行文件的应用程序（该主题将在第 19 章中讨论），你可以使用已启动进程的 ExitCode 属性获取 %ERRORLEVEL% 的值。

由于应用程序的返回值在应用程序终止时传递给系统，因此应用程序在运行时显然无法获取并显示其最终错误代码。但是，为了说明程序终止后如何查看此错误级别，请首先按如下方式更新顶层语句：

// Note we are explicitly returning an int, rather than void.
// Display a message and wait for Enter key to be pressed.
Console.WriteLine("***** My First C# App *****");
Console.WriteLine("Hello World!");

```
Console.WriteLine();
Console.ReadLine();
```

```
// Return an arbitrary error code.
return -1;
```

如果程序仍在使用 Main() 方法作为入口点，请将方法签名更改为返回 int 而不是 void，如下所示：

```
static int Main()
{
...
}
```

现在让我们借助批处理文件捕获程序的返回值。使用 Windows 资源管理器，导航到包含项目文件的文件夹（例如，C:\SimpleCSharpApp）并在该文件夹中添加一个新文本文件（命名为 SimpleCSharpApp.cmd）。将文件夹的内容更新为以下内容（如果你以前从未编写过 *.cmd 文件，无需关心细节）：

```
@echo off
rem A batch file for SimpleCSharpApp.exe
rem which captures the app's return value.

dotnet run
@if "%ERRORLEVEL%" == "0" goto success

:fail
echo This application has failed!
echo return value = %ERRORLEVEL%
goto end
:success
echo This application has succeeded!
echo return value = %ERRORLEVEL%
goto end
:end
echo All Done.
```

此时，打开命令提示符（或使用 VSC 终端）并导航到包含新的 *.cmd 文件的文件夹。通过键入其名称并按 Enter 键执行该文件。由于你的 Main() 方法返回 -1，你应该会看到如下输出。如果 Main() 方法返回 0，你会看到消息“This application has succeeded!”打印到控制台。

```
***** My First C# App *****

Hello World!

This application has failed!
return value = -1
All Done.
```

前面 *.cmd 文件的 PowerShell 等效文件如下：

```
dotnet run
if ($LastExitCode -eq 0) {
    Write-Host "This application has succeeded!"
} else
{
    Write-Host "This application has failed!"
}
Write-Host "All Done."
```

要运行它，请在 VSC 终端中输入 PowerShell，然后通过键入以下内容执行脚本：

```
.\SimpleCSharpApp.ps1
```

你将在终端窗口中看到以下内容：

```
***** My First C# App *****

Hello World!

This application has failed!
All Done.
```

绝大多数（如果不是全部）你的 C# 应用程序将使用 void 作为 Main() 的返回值，这如你所回忆的那样，隐式返回错误代码零。为此，本书中当前示例之外的 Main() 方法将返回 void。

## 处理命令行参数（更新 9.0）

现在你更好地理解了 Main() 方法或顶层语句的返回值，让我们来检查传入的字符串数据数组。假设你现在想要更新应用程序以处理任何可能的命令行参数。一种方法是使用 C# for 循环。（请注意，C# 的迭代结构将在本章末尾详细讨论。）

// Display a message and wait for Enter key to be pressed.
Console.WriteLine("***** My First C# App *****");
Console.WriteLine("Hello World!");
Console.WriteLine();
// Process any incoming args.
for (int i = 0; i < args.Length; i++)
{
    Console.WriteLine("Arg: {0}", args[i]);
}
Console.ReadLine();
// Return an arbitrary error code.
return 0;

> 注意 此示例使用顶层语句，不使用 Main() 方法。更新 Main() 方法以接受 args 参数的内容将在后面介绍。

再次检查使用顶层语句的程序生成的 IL，注意到 <Main>$ 方法接受一个名为 args 的字符串数组，如下所示（为节省空间已缩写）：

```
.class private abstract auto ansi sealed beforefieldinit '<Program>$'
    extends [System.Runtime]System.Object
{
    .custom instance void [System.Runtime]System.Runtime.CompilerServices.
CompilerGeneratedAttribute::.ctor()=
    ( 01 00 00 00 )
    .method private hidebysig static
        void '<Main>$' (string[] args) cil managed
    {
        .entrypoint
... } // end of method '<Program>$'::'<Main>$'
} // end of class '<Program>$'
```

如果程序仍在使用 Main() 方法作为入口点，请确保方法签名接受一个名为 args 的字符串数组，如下所示：

```
static int Main(string[] args)
{
...
}
```

在这里，你使用 System.Array 的 Length 属性检查字符串数组是否包含若干项。正如你在第 4 章中将看到的那样，所有 C# 数组实际上都别名为 System.Array 类，因此共享一组公共成员。当你遍历数组中的每个项时，其值会被打印到控制台窗口。命令行提供参数同样简单，如下所示：

```
C:\SimpleCSharpApp>dotnet run /arg1 -arg2

***** My First C# App *****
Hello World!
Arg: /arg1
Arg: -arg2
```

作为标准 for 循环的替代方案，你可以使用 C# foreach 关键字遍历传入的字符串数组。以下是一些示例用法（但循环结构的细节你将在本章后面看到）：

// Notice you have no need to check the size of the array when using "foreach".
// Process any incoming args using foreach.
foreach(string arg in args)

