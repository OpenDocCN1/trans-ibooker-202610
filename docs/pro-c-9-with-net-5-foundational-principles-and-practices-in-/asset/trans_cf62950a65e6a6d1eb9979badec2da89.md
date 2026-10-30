

允许在给定的字符串中重复使用占位符。例如，如果你是一名 Beatles 粉丝，想要构建字符串 "9, Number 9, Number 9"，你会这样写：

```
// John says...
Console.WriteLine("{0}, Number {0}, Number {0}", 9);
```

另外，请注意，可以将每个占位符放置在字符串字面量中的任意位置，并且不必遵循递增的顺序。例如，考虑以下代码片段：

```
// Prints: 20, 10, 30
Console.WriteLine("{1}, {0}, {2}", 10, 20, 30);
```

字符串也可以使用字符串插值进行格式化，这将在本章后面介绍。

## Formatting Numerical Data

如果需要对数值数据进行更精细的格式化，每个占位符都可以选择性地包含各种格式字符。表 3-3 显示了最常见的格式化选项。

Table 3-3. .NET Core Numerical Format Characters

| String Format Character | Meaning in Life |
|---|---|
| C or c | 用于格式化货币。默认情况下，该标志会加上本地文化符号前缀（对于美国英语为美元符号 [$]）。 |
| D or d | 用于格式化十进制数字。该标志还可以指定用于填充值的最小位数。 |
| E or e | 用于指数记数法。大写或小写控制指数常数是使用大写字母 E 还是小写字母 e。 |
| F or f | 用于定点格式化。该标志还可以指定用于填充值的最小位数。 |
| G or g | 代表通用。此字符可用于将数字格式化为定点或指数格式。 |
| N or n | 用于基本数值格式化（带逗号）。 |
| X or x | 用于十六进制格式化。如果使用大写 X，你的十六进制格式也将包含大写字符。 |

这些格式字符使用冒号标记后缀添加到给定的占位符值（例如，{0:C}, {1:d}, {2:X}）。为了说明这一点，将 Main() 方法更新为调用名为 FormatNumericalData() 的新辅助函数。在你的 Program 类中实现此方法，以多种方式格式化固定的数值。

```
// Now make use of some format tags.
static void FormatNumericalData()
{
    Console.WriteLine("The value 99999 in various formats:");
    Console.WriteLine("c format: {0:c}", 99999);
```

```
Console.WriteLine("d9 format: {0:d9}", 99999);
Console.WriteLine("f3 format: {0:f3}", 99999);
Console.WriteLine("n format: {0:n}", 99999);

// Notice that upper- or lowercasing for hex
// determines if letters are upper- or lowercase.
Console.WriteLine("E format: {0:E}", 99999);
Console.WriteLine("e format: {0:e}", 99999);
Console.WriteLine("X format: {0:X}", 99999);
Console.WriteLine("x format: {0:x}", 99999);
}
```

调用 FormatNumericalData() 方法后的输出如下所示：

```
The value 99999 in various formats:

c format: $99,999.00
d9 format: 000099999
f3 format: 99999.000
n format: 99,999.00
E format: 9.999900E+004
e format: 9.999900e+004
X format: 1869F
x format: 1869f
```

在本文中需要时你会看到额外的格式化示例；然而，如果你有兴趣进一步深入研究字符串格式化，请查阅 .NET Core 文档中的主题“Formatting Types”。

## Formatting Numerical Data Beyond Console Applications

最后请注意，字符串格式化字符的使用并不限于控制台程序。相同的格式化语法也可以在调用静态 string.Format() 方法时使用。当你需要在运行时为任何应用程序类型（例如，桌面 GUI 应用、ASP.NET Web 应用等）组合文本数据时，这会很有帮助。

string.Format() 方法返回一个新字符串对象，该对象根据提供的标志进行格式化。以下代码以十六进制格式化字符串：

```
// Using string.Format() to format a string literal.
string userMessage = string.Format("100000 in hex is {0:x}", 100000);
```

## Working with System Data Types and Corresponding C# Keywords

像任何编程语言一样，C# 为基本数据类型定义了关键字，用于表示局部变量、类数据成员变量、方法返回值和参数。与其他编程语言不同的是，这些关键字不仅仅是简单的编译器识别的标记。相反，C# 数据类型关键字实际上是 System 命名空间中完整类型的简写表示。表 3-4 列出了每个系统数据类型、其范围、对应的 C# 关键字以及类型是否符合通用语言规范（CLS）。出于可读性考虑，所有系统类型均位于 System 命名空间中，在图表中省略了该命名空间。

## Table 3-4. The Intrinsic Data Types of C#

| C# Shorthand | CLS Compliant? | System Type | Range | Meaning in Life |
|---|---|---|---|---|
| bool | Yes | Boolean | true or false | 代表真或假 |
| sbyte | No | SByte | -128 to 127 | 有符号 8 位数字 |
| byte | Yes | Byte | 0 to 255 | 无符号 8 位数字 |
| short | Yes | Int16 | -32,768 to 32,767 | 有符号 16 位数字 |
| ushort | No | UInt16 | 0 to 65,535 | 无符号 16 位数字 |
| int | Yes | Int32 | -2,147,483,648 to 2,147,483,647 | 有符号 32 位数字 |
| uint | No | UInt32 | 0 to 4,294,967,295 | 无符号 32 位数字 |
| long | Yes | Int64 | -9,223,372,036,854,775,808 to 9,223,372,036,854,775,807 | 有符号 64 位数字 |
| ulong | No | UInt64 | 0 to 18,446,744,073,709,551,615 | 无符号 64 位数字 |
| char | Yes | Char | U+0000 to U+ffff | 单个 16 位 Unicode 字符 |
| float | Yes | Single | -3.4 10³⁸ to +3.4 10³⁸ | 32 位浮点数 |
| double | Yes | Double | ±5.0 10⁻³²⁴ to ±1.7 10³⁰⁸ | 64 位浮点数 |
| decimal | Yes | Decimal | (-7.9 x 10²⁸ to 7.9 x 10²⁸)/(10⁰ to ²⁸) | 128 位有符号数字 |
| string | Yes | String | Limited by system memory | 代表一组 Unicode 字符 |
| object | Yes | Object | Can store any data type in an object variable | .NET 宇宙中所有类型的基类 |

> Note Recall from Chapter 1 that CLS-compliant .NET Core code can be used by any other .NET Core programming language. If you expose non-CLS-compliant data from your programs, other .NET Core languages might not be able to make use of it.

## Understanding Variable Declaration and Initialization

当你声明局部变量（例如成员作用域内的变量）时，你通过指定数据类型，然后是变量名来完成。首先，创建一个名为 BasicDataTypes 的新控制台应用程序项目，并使用以下命令将其添加到解决方案中：

```
dotnet new console -lang c# -n BasicDataTypes -o .\BasicDataTypes -f net5.0
dotnet sln .\Chapter3_AllProjects.sln add .\BasicDataTypes
```

将代码更新为以下内容：

```
using System;
using System.Numerics;

Console.WriteLine("***** Fun with Basic Data Types *****
");
```

现在，添加以下静态局部函数并从顶层语句调用它：

```
static void LocalVarDeclarations()
{
    Console.WriteLine("=> Data Declarations:");
    // Local variables are declared as so:
    // dataType varName;
    int myInt;
    string myString;
    Console.WriteLine();
}
```

请注意，在为局部变量分配初始值之前使用它是一个编译器错误。鉴于此，在声明时为本地数据点分配初始值是一个良好的实践。你可以在一行中完成，也可以通过将声明和赋值分离为两个代码语句。

```
static void LocalVarDeclarations()
{
    Console.WriteLine("=> Data Declarations:");
    // Local variables are declared and initialized as follows:
    // dataType varName = initialValue;
    int myInt = 0;

    // You can also declare and assign on two lines.
    string myString;
    myString = "This is my character data";

    Console.WriteLine();
}
```

你也可以在一行代码中声明同一底层类型的多个变量，如以下三个 bool 变量所示：

```
static void LocalVarDeclarations()
{
    Console.WriteLine("=> Data Declarations:");
    int myInt = 0;
    string myString;
    myString = "This is my character data";

    // Declare 3 bools on a single line.
    bool b1 = true, b2 = false, b3 = b1;
    Console.WriteLine();
}
```

由于 C# 的 bool 关键字只是 System.Boolean 结构的简写表示，因此也可以使用其全名分配任何数据类型（当然，同样的原则适用于任何 C# 数据类型关键字）。以下是 LocalVarDeclarations() 的最终实现，展示了声明局部变量的各种方法：

```
static void LocalVarDeclarations()
{
    Console.WriteLine("=> Data Declarations:");
    // Local variables are declared and initialized as follows:
    // dataType varName = initialValue;
    int myInt = 0;

    string myString;
    myString = "This is my character data";

    // Declare 3 bools on a single line.
    bool b1 = true, b2 = false, b3 = b1;

    // Use System.Boolean data type to declare a bool.
    System.Boolean b4 = false;

    Console.WriteLine("Your data: {0}, {1}, {2}, {3}, {4}, {5}",
        myInt, myString, b1, b2, b3, b4);
    Console.WriteLine();
}
```

## The default Literal (New 7.1)

default 字面量为变量分配其数据类型的默认值。这适用于标准数据类型以及自定义类（第 5 章）和泛型类型（第 10 章）。创建一个名为 DefaultDeclarations() 的新方法并添加以下代码：

```
static void DefaultDeclarations()
{
    Console.WriteLine("=> Default Declarations:");
    int myInt = default;
}
```
