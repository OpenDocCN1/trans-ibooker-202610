
5. 添加语句以定义一个具有静态抽象方法的接口、实现它的类，然后在顶层程序中调用该方法，如以下代码所示：

```
using static System.Console;

Doer.DoSomething();

public interface IWithStaticAbstract
{
    static abstract void DoSomething();
}

public class Doer : IWithStaticAbstract
{
    public static void DoSomething()
    {
        WriteLine("I am an implementation of a static abstract method.");
    }
}
```

6. 运行控制台应用程序并注意它正确输出。

## 泛型数学

为什么 Microsoft 添加了定义静态抽象方法的能力？它们有什么用？

很长一段时间以来，开发人员一直要求 Microsoft 能够在泛型类型上使用像 * 这样的运算符。这将使开发人员能够定义数学方法来对任何泛型类型执行加法、平均值等操作，而不必为他们想要支持的所有数字类型创建数十个重载方法。接口中对静态抽象方法的支持是一个基础功能，可实现泛型数学。

如果您感兴趣，可以在以下链接阅读更多相关信息：

https://devblogs.microsoft.com/dotnet/preview-features-in-net-6-generic-math/

## 练习与探索

通过回答一些问题、进行一些实践操作，并对本章主题进行更深入的研究来测试您的知识和理解。

## 练习 7.1 – 测试你的知识

回答以下问题：

-   1. 命名空间和程序集有什么区别？
-   2. 如何在 .csproj 文件中引用另一个项目？
-   3. ILSpy 之类的工具有什么好处？
-   4. C# float 别名代表哪个 .NET 类型？
-   5. 将应用程序从 .NET Framework 移植到 .NET 6 时，移植前应运行什么工具，以及可以运行什么工具来完成大部分移植工作？
-   6. .NET 应用程序的框架依赖部署和自包含部署有什么区别？
-   7. RID 是什么？
-   8. dotnet pack 和 dotnet publish 命令有什么区别？
-   9. 为 .NET Framework 编写的哪些类型的应用程序可以移植到现代 .NET？
-   10. 可以使用为 .NET Framework 编写的包与现代 .NET 吗？

## 练习 7.2 – 探索主题

使用以下页面上的链接了解本章涵盖主题的更多详细信息：

```
https://github.com/markjprice/cs10dotnet6/blob/main/book-links.md#chapter-7---understanding-and-packaging-net-types
```

## 练习 7.3 – 探索 PowerShell

PowerShell 是 Microsoft 用于在每个操作系统上自动化任务的脚本语言。Microsoft 建议使用带有 PowerShell 扩展的 Visual Studio Code 来编写 PowerShell 脚本。

由于 PowerShell 是一种独立的庞大语言，本书中没有空间涵盖它。相反，我在书籍的 GitHub 仓库中创建了一些补充页面，向您介绍一些关键概念并展示一些示例：

```
https://github.com/markjprice/cs10dotnet6/tree/main/docs/powershell
```

## 总结

在本章中，我们回顾了通往 .NET 6 的历程，我们探索了程序集和命名空间之间的关系，我们看到了为在多个操作系统上分发而发布应用程序的选项，封装和分发了一个类库，并讨论了移植现有 .NET Framework 代码库的选项。

在下一章中，您将了解现代 .NET 中包含的一些常见基类库类型。

## 08

## 使用常见的 .NET 类型

本章介绍 .NET 中包含的一些常见类型。这些包括用于操作数字、文本、集合、网络访问、反射和属性的类型；改进使用 spans、索引和范围的工作；操作图像；以及国际化。

本章涵盖以下主题：

-   - 使用数字
-   - 使用文本
-   - 使用日期和时间
-   - 使用正则表达式进行模式匹配
-   - 在集合中存储多个对象
-   - 使用 spans、索引和范围
-   - 使用网络资源
-   - 使用反射和属性
-   - 使用图像
-   - 使您的代码国际化

## 使用数字

数据中最常见的类型之一是数字。用于在 .NET 中处理数字的最常见类型如以下表格所示：

| 命名空间 | 示例类型(s) | 描述 |
|---|---|---|
| System | SByte, Int16, Int32, Int64 | 整数；即零、正负整数 |
| System | Byte, UInt16, UInt32, UInt64 | 基数；即零和正整数 |
| System | Half, Single, Double | 实数；即浮点数 |
| System | Decimal | 精确实数；即用于科学、工程或金融场景 |
| System.Numerics | BigInteger, Complex, Quaternion | 任意大整数、复数和四元数 |

.NET 自 .NET Framework 1.0 以来就拥有 32 位 float 和 64 位 double 类型。IEEE 754 规范还定义了 16 位浮点标准。机器学习和其他算法将受益于这种更小、精度更低的数字类型，因此 Microsoft 在 .NET 5 及更高版本中引入了 System.Half 类型。

目前 C# 语言未定义 half 别名，因此必须使用 .NET 类型 System.Half。这将来可能会改变。

## 使用大整数

在具有 C# 别名的 .NET 类型中可以存储的最大整数约为十八点五千万亿，存储在无符号长整数中。但如果您需要存储更大的数字怎么办？

让我们探索数值：

-   1. 使用您喜欢的代码编辑器创建一个名为 `Chapter08` 的新解决方案/工作区。
-   2. 添加控制台应用程序项目，如以下列表中定义：
-      1. 项目模板：**控制台应用程序** / `console`
-      2. 工作区/解决方案文件和文件夹：`Chapter08`
-      3. 项目文件和文件夹：`WorkingWithNumbers`
-   3. 在 `Program.cs` 中，删除现有语句并添加导入 `System.Numerics` 的语句，如以下代码所示：
-   4. 添加语句以输出 `ulong` 类型的最大值，以及使用 `BigInteger` 的 30 位数字，如以下代码所示：

```
using System.Numerics;
```

```
WriteLine("Working with large integers:");
WriteLine("---------------------------------");

ulong big = ulong.MaxValue;
WriteLine($"{big,40:N0}");

BigInteger bigger =
    BigInteger.Parse("123456789012345678901234567890");

WriteLine($"{bigger,40:N0}");
```

格式代码中的 40 表示右对齐 40 个字符，因此两个数字都对齐到右边缘。N0 表示使用千位分隔符和零个小数位。

-   5. 运行代码并查看结果，如以下输出所示：

```
Working with large integers:
---------------------------------
                18,446,744,073,709,551,615
123,456,789,012,345,678,901,234,567,890
```

## 使用复数

复数可以表示为 a + bi，其中 a 和 b 是实数，i 是虚数单位，其中 i² = -1。如果实部 a 为零，则为纯虚数。如果虚部 b 为零，则为实数。

复数在许多 STEM（科学、技术、工程和数学）研究领域有实际应用。此外，它们是通过分别相加加数的实部和虚部来相加的；考虑以下情况：

```
(a + bi) + (c + di) = (a + c) + (b + d)i
```

让我们探索复数：

-   1. 在 Program.cs 中，添加语句以相加两个复数，如以下代码所示：

```
WriteLine("Working with complex numbers:");
Complex c1 = new(real: 4, imaginary: 2);
Complex c2 = new(real: 3, imaginary: 7);
Complex c3 = c1 + c2;

// output using default ToString implementation
WriteLine($"{c1} added to {c2} is {c3}");

// output using custom format
```
