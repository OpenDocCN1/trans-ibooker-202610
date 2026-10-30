

更好的做法是将每个类放在自己的文件中，每个文件都有命名空间定义。为了确保每个类型都被打包到同一个逻辑组中，只需将给定的类定义包装在同一个命名空间作用域中，如下所示：

```
namespace MyShapes
{
    // Circle class
    public class Circle { /* Interesting methods... */ }
}
```

```
// Hexagon.cs
namespace MyShapes
{
    // Hexagon class
    public class Hexagon { /* More interesting methods... */ }
}
```

```
// Square.cs
namespace MyShapes
{
    // Square class
    public class Square { /* Even more interesting methods...*/}
}
```

Guidance 认为每个代码文件中只包含一个类是一种最佳实践。虽然之前的一些示例没有这样做，但那是为了教学上的简便。从本书后续内容开始，我的意图是始终将每个类分离到自己的文件中。

注意 MyShapes 命名空间如何充当这些类的概念上的“容器”。当另一个命名空间（例如 CustomNamespaces）想要使用另一个命名空间中的类型时，你可以使用 using 关键字，就像使用 .NET Core 基类库的命名空间一样，如下所示：

```
// Bring in a namespace from the base class libraries.
using System;
// Make use of types defined the MyShapes namespace.
using MyShapes;

Hexagon h = new Hexagon();
Circle c = new Circle();
Square s = new Square();
```

对于此示例，假设定义 MyShapes 命名空间的 C# 文件是同一个控制台应用程序项目的一部分；换句话说，所有文件都被编译成单个程序集。如果你将 MyShapes 命名空间定义在外部程序集中，在成功编译之前你还需要添加对该库的引用。你将在本章学习使用外部库构建应用程序的所有细节。

## 使用完全限定名解决名称冲突

从技术上讲，你并不是必须使用 C# using 关键字来引用外部命名空间中定义的类型。你可以使用类型的完全限定名，其定义如你可能在第 1 章回忆的那样，是类型名称加上定义它的命名空间前缀。以下是示例：

```
// Note we are not importing MyShapes anymore!
using System;

MyShapes.Hexagon h = new MyShapes.Hexagon();
MyShapes.Circle c = new MyShapes.Circle();
MyShapes.Square s = new MyShapes.Square();
```

通常，没有必要使用完全限定名。不仅需要更多的击键，而且在代码大小或执行速度方面没有任何区别。事实上，在 CIL 代码中，类型始终使用完全限定名定义。在这种情况下，C# using 关键字只是一个节省输入时间的工具。

然而，完全限定名在处理使用多个包含同名类型的命名空间时可能很有帮助（有时是必要的），以避免潜在的名称冲突。假设你有一个名为 My3DShapes 的新命名空间，它定义了以下三个类，能够以惊人的 3D 效果渲染形状：

```
// Another shape-centric namespace.
//Circle.cs
namespace My3DShapes
{
    // 3D Circle class.
    public class Circle { }
}
//Hexagon.cs
namespace My3DShapes
{
    // 3D Hexagon class.
    public class Hexagon { }
}
//Square.cs
namespace My3DShapes
{
    // 3D Square class.
    public class Square { }
}
```

如果你按如下所示更新顶层语句，你会收到多个编译时错误，因为两个命名空间都定义了同名类：

```
// Ambiguities abound!
using System;
using MyShapes;
using My3DShapes;

// Which namespace do I reference?
Hexagon h = new Hexagon(); // Compiler error!
```

```
Circle c = new Circle();  // Compiler error!
Square s = new Square();  // Compiler error!
```

歧义可以通过使用类型的完全限定名来解决，如下所示：

```
// We have now resolved the ambiguity.
My3DShapes.Hexagon h = new My3DShapes.Hexagon();
My3DShapes.Circle c = new My3DShapes.Circle();
MyShapes.Square s = new MyShapes.Square();
```

## 使用别名解决名称冲突

C# using 关键字还允许你为类型的完全限定名创建别名。当你这样做时，你定义了一个在编译时被替换为类型全名的标记。定义别名提供了第二种解决名称冲突的方法。以下是示例：

```
using System;
using MyShapes;
using My3DShapes;
```

```
// Resolve the ambiguity using a custom alias.
using The3DHexagon = My3DShapes.Hexagon;
```

```
// This is really creating a My3DShapes.Hexagon class.
The3DHexagon h2 = new The3DHexagon();
...
```

这种替代的 using 语法还允许你为冗长的命名空间创建别名。较长的基类库命名空间之一是 System.Runtime.Serialization.Formatters.Binary，它包含一个名为 BinaryFormatter 的成员。如果你愿意，可以按如下方式创建 BinaryFormatter 的实例：

```
using bfHome = System.Runtime.Serialization.Formatters.Binary;
bfHome.BinaryFormatter b = new bfHome.BinaryFormatter();
...
```

以及使用传统的 using 指令：

```
using System.Runtime.Serialization.Formatters.Binary;
BinaryFormatter b = new BinaryFormatter();
...
```

在游戏的这个阶段，你无需关心 BinaryFormatter 类的用途（你将在第 20 章检查这个类）。现在，只需记住 C# using 关键字可用于为冗长的完全限定名定义别名，或更常见地，用于解决导入多个定义了同名类型的命名空间时可能出现的名字冲突。

> Note 请注意，过度使用 C# 别名可能导致代码库混乱。如果团队中的其他程序员不知道你的自定义别名，他们可能会假设别名指的是基类库中的类型，并在文档中找不到这些标记时感到非常困惑！

## 创建嵌套命名空间

在组织你的类型时，你可以自由地在其他命名空间内定义命名空间。基类库在许多地方都这样做，以提供更深层次的类型组织。例如，IO 命名空间嵌套在 System 中，形成 System.IO。

.NET Core 项目模板将 Program.cs 中的初始代码添加到以项目命名的命名空间中。此基本命名空间称为根命名空间。在此示例中，.NET Core 模板创建的根命名空间是 CustomNamespaces，如下所示：

```
namespace CustomNamespaces
{
    class Program
    {
    ...
    }
}
```

> Note 当用顶层语句替换 Program/Main() 组合时，你不能为这些顶层语句分配命名空间。

要将 MyShapes 和 My3DShape 命名空间嵌套在根命名空间内，有两个选项。第一个是简单地嵌套 namespace 关键字，如下所示：

```
namespace CustomNamespaces
{
    namespace MyShapes
    {
        // Circle class
        public class Circle
        {
            /* Interesting methods... */
        }
    }
}
```

另一个选项（更常见）是使用命名空间定义中的“点表示法”，如下所示：

```
namespace CustomNamespaces.MyShapes
{
    // Circle class
    public class Circle
    {
        /* Interesting methods... */
    }
}
```

命名空间不必直接包含任何类型。这允许开发人员使用命名空间提供进一步的范围级别。

鉴于你现在已将 My3DShape 命名空间嵌套在 CustomNamespaces 根命名空间内，你需要更新任何现有的 using 指令和类型别名，如下所示（假设你已将所有示例类更新为嵌套在根命名空间下）：

```
using The3DHexagon = CustomNamespaces.My3DShape.Hexagon;
using CustomNamespaces.MyShapes;
```

Guidance 一种常见做法是按目录对文件分组在命名空间中。如图所示，文件在目录结构中的位置对命名空间没有影响。然而，它使命名空间结构对其他开发人员更清晰（和具体）。因此，许多开发人员和代码 linting 工具期望命名空间与文件夹结构匹配。

## 更改 Visual Studio 的根命名空间

如前所述，当你使用 Visual Studio（或 .NET Core CLI）创建新的 C# 项目时，你应用程序的根命名空间名称将与项目名称相同。从现在开始，当你使用 Visual Studio 通过 Project ► Add New Item 菜单选项插入新的代码文件时，类型将自动包装在根命名空间中。如果你想更改根命名空间的名称，只需通过项目属性窗口的 Application 选项卡访问“Default namespace”选项即可（见图 16-1）。

![](img/587021f4d5ba039be70a11107130377b_643_0.png)

Figure 16-1. Configuring the default namespace

