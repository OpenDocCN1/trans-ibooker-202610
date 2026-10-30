

## 理解通用语言规范

如你所知，不同的语言以独特的、特定于语言的术语表达相同的编程结构。例如，在 C# 中你使用加号运算符 (+) 表示字符串连接，而在 VB 中你通常使用与号 (&)。即使两种不同的语言表达相同的程序化习惯用法（例如，无返回值的函数），表面上的语法也可能看起来截然不同。

```
// C# method returning nothing.
public void MyMethod()
{
    // Some interesting code...
}
```

```
' VB method returning nothing.
Public Sub MyMethod()
    ' Some interesting code...
End Sub
```

正如你已经看到的，这些细微的语法差异在 .NET Core 运行时眼中是无关紧要的，因为相应的编译器（本例中为 csc.exe 或 vbc.exe）会发出相似的 CIL 指令集。然而，语言在整体功能水平上也可能存在差异。例如，一个 .NET Core 语言可能有也可能没有表示无符号数据的关键字，也可能支持也可能不支持指针类型。鉴于这些可能的差异，最好有一个所有 .NET Core 语言都期望遵循的基线。

CLS 是一组规则，详细描述了给定 .NET Core 编译器必须支持的最小且完整的特性集，以便生成可由 .NET 运行时托管的代码，同时能够以统一的方式被所有针对 .NET Core 平台的语言访问。在许多方面，CLS 可以被视为 CTS 定义的完整功能的子集。

CLS 最终是一组规则，编译器构建者必须遵守这些规则，如果他们希望其产品在 .NET Core 宇宙中无缝运行。每条规则都被赋予一个简单名称（例如 CLS Rule 6），并描述了该规则如何影响构建编译器的那些人以及以某种方式与之交互的那些人。CLS 的精华是规则 1。

> Rule 1: CLS rules apply only to those parts of a type that are exposed outside the defining assembly.

根据此规则，你可以（正确地）推断出 CLS 的其余规则不适用于构建 .NET Core 类型内部工作所使用的逻辑。必须符合 CLS 的类型唯一方面是成员定义本身（即命名约定、参数和返回类型）。成员的实现逻辑可以使用任意数量的非 CLS 技术，因为外部世界不会知道其中的差异。

举例说明，以下 C# Add() 方法不符合 CLS，因为参数和返回值使用了无符号数据（这不是 CLS 的要求）：

```
class Calc
{
    // Exposed unsigned data is not CLS compliant!
    public ulong Add(ulong addend1, ulong addend2)
    {
        return addend1 + addend2;
    }
}
```

然而，考虑以下在方法内部使用无符号数据的代码：

```
class Calc
{
    public int Add(int addend1, int addend2)
    {
        // As this ulong variable is only used internally,
        // we are still CLS compliant.
        ulong temp = 0;
        ...
        return addend1 + addend2;
    }
}
```

该类仍然符合 CLS 的规则，可以放心所有 .NET Core 语言都能调用 Add() 方法。

当然，除了规则 1 之外，CLS 还定义了许多其他规则。例如，CLS 描述了给定语言必须如何表示文本字符串，枚举应如何在内部表示（用于存储的基础类型），如何定义静态成员等等。幸运的是，你不必记住这些规则才能成为一名熟练的 .NET 开发者。总体而言，对 CTS 和 CLS 规范的深入理解通常只对工具/编译器构建者感兴趣。

## 确保 CLS 合规性

正如你在本书的后续章节中会看到的，C# 确实定义了一些不符合 CLS 的编程结构。好消息是，你可以指示 C# 编译器使用单个 .NET 属性来检查你的代码是否符合 CLS。

```
// Tell the C# compiler to check for CLS compliance.
[assembly: CLSCompliant(true)]
```

第 17 章深入探讨了基于属性的编程的细节。在此之前，只需理解 [CLSCompliant] 属性将指示 C# 编译器根据 CLS 规则检查代码的每一行。如果发现任何 CLS 违规，你会收到编译器警告和违规代码的描述。

## 理解 .NET Core 运行时

除了 CTS 和 CLS 规范之外，拼图的最后一块是 .NET Core 运行时，或简称为 .NET 运行时。从编程角度来看，运行时可以理解为执行给定编译代码单元所需的服务集合。例如，当 Java 开发者将软件部署到新计算机时，他们需要确保 Java 虚拟机（JVM）已安装在机器上才能运行其软件。

.NET Core 平台提供了另一种运行时系统。.NET Core 运行时与其他运行时之间的关键区别在于，.NET Core 运行时提供了一个单一的、定义良好的运行时层，由所有 .NET Core 语言和平台共享。

## 区分程序集、命名空间和类型

我们每个人都了解代码库的重要性。框架库的目的是为开发者提供一套现成的代码供其在应用程序中使用。然而，C# 语言并不附带特定于语言的代码库。相反，C# 开发者利用语言中立的 .NET Core 库。为了使基类库中的所有类型保持良好的组织，.NET Core 平台大量使用命名空间概念。

命名空间是一个语义相关的类型集合，包含在程序集中或可能分布在多个相关程序集中。例如，System.IO 命名空间包含文件 I/O 相关类型，System.Data 命名空间定义基本数据库类型，等等。重要的是要指出，单个程序集可以包含任意数量的命名空间，每个命名空间可以包含任意数量的类型。

这种方法与特定于语言的库之间的关键区别在于，任何针对 .NET Core 运行时目标的语言都使用相同的命名空间和相同类型。例如，以下两个程序都说明了无处不在的 Hello World 应用程序，分别用 C# 和 VB 编写：

// Hello World in C#.
using System;

public class MyApp
{
    static void Main()
    {
        Console.WriteLine("Hi from C#");
    }
}
```

```
' Hello World in VB.
Imports System
Public Module MyApp
    Sub Main()
        Console.WriteLine("Hi from VB")
    End Sub
End Module
```

请注意，每个语言都在使用 System 命名空间中定义的 Console 类。除了一些明显的语法差异外，这些应用程序在物理和逻辑上看起来和感觉都非常相似。

显然，一旦你熟悉了选择的 .NET Core 编程语言，作为 .NET Core 开发者你的下一个目标就是熟悉（众多） .NET Core 命名空间中定义的丰富类型。最初需要掌握的最基本的命名空间名为 System。该命名空间提供了一组核心类型，你将作为 .NET Core 开发者一次又一次地需要利用它们。事实上，你无法构建任何功能性的 C# 应用程序而不至少引用 System 命名空间，因为核心数据类型（例如 System.Int32、System.String）在这里定义。表 1-3 按相关功能对一些（但 certainly 不是全部） .NET Core 命名空间进行了概述。

## 表 1-3. .NET 命名空间采样

| .NET Namespace | Meaning in Life |
|---|---|
| System | Within System, you find numerous useful types dealing with intrinsic data, mathematical computations, random number generation, environment variables, and garbage collection, as well as a number of commonly used exceptions and attributes. |
| System.Collections System.Collections.Generic | These namespaces define a number of stock container types, as well as base types and interfaces that allow you to build customized collections. |
| System.Data System.Data.Common System.Data.SqlClient | These namespaces are used for interacting with relational databases using ADO.NET. |
| System.IO System.IO.Compression System.IO.Ports | These namespaces define numerous types used to work with file I/O, compression of data, and port manipulation. |
| System.Reflection System.Reflection.Emit | These namespaces define types that support runtime type discovery as well as dynamic creation of types. |
| System.Runtime.InteropServices | This namespace provides facilities to allow .NET types to interact with unmanaged code (e.g., C-based DLLs and COM servers) and vice versa. |
| System.Drawing System.Windows.Forms | These namespaces define types used to build desktop applications using .NET's original UI toolkit (Windows Forms). |
| System.Windows System.Windows.Controls System.Windows.Shapes | The System.Windows namespace is the root for several namespaces that are used in Windows Presentation Foundation applications. |
| System.Windows.Forms System.Drawing | The System.Windows.Forms namespace is the root for several namespaces used in Windows Forms applications. |
| System.Linq System.Linq.Expressions | These namespaces define types used when programming against the LINQ API. |
| System.AspNetCore | This is one of many namespaces that allows you to build ASP.NET Core web applications and RESTful services. |
| System.Threading System.Threading.Tasks | These namespaces define numerous types to build multithreaded applications that can distribute workloads across multiple CPUs. |
| System.Security | Security is an integrated aspect of the .NET universe. In the security-centric namespaces, you find numerous types dealing with permissions, cryptography, etc. |
| System.Xml | The XML-centric namespaces contain numerous types used to interact with XML data. |

## 以编程方式访问命名空间

值得重申的是，命名空间只不过是我们这些凡人为了逻辑上理解和组织相关类型而方便使用的一种方式。再次考虑 System 命名空间。从你的角度来看，你可以假设 System.Console 代表一个名为 Console 的类，包含在名为 System 的命名空间中。然而，在 .NET Core 运行时眼中并非如此。运行时引擎只看到一个名为 System.Console 的单一类。

