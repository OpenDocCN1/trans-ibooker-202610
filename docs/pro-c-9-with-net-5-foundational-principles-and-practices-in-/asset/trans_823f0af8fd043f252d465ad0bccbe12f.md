

# 使用 System.Type.GetType() 获取类型引用

要以更灵活的方式获取类型信息，可以调用 System.Type 类的静态 GetType() 成员，并指定你感兴趣的类型完全限定字符串名称。使用这种方法，你不需要在编译时就知道你要提取元数据的类型，因为 Type.GetType() 接受一个无处不在的 System.String 实例。

> 注意 当我说调用 Type.GetType() 时你不需要编译时知识时，我指的是该方法可以接受任何字符串值（而不是强类型变量）。当然，你仍然需要以“字符串化”格式知道类型的名称！

Type.GetType() 方法已被重载以允许你指定两个布尔参数，其中一个控制如果找不到类型是否应抛出异常，另一个建立字符串的大小写敏感性。为了说明，请思考以下内容：

```
// 使用静态 Type.GetType() 方法获取类型信息
// （如果找不到 SportsCar 则不抛出异常且忽略大小写）。
Type t = Type.GetType("CarLibrary.SportsCar", false, true);
```

在前面的示例中，注意你传递给 GetType() 的字符串中没有提到包含该类型的程序集。在这种情况下，假设该类型是在当前执行的程序集中定义的。然而，当你想要获取外部程序集中类型的元数据时，字符串参数的格式使用类型的完全限定名称，后跟一个逗号，后跟包含该类型的程序集的友好名称（程序集名称不含任何版本信息），如下所示：

```
// 获取外部程序集中类型的类型信息。
Type t = Type.GetType("CarLibrary.SportsCar, CarLibrary");
```

此外，请注意传递给 Type.GetType() 的字符串可以指定加号标记（+）来表示嵌套类型。假设你想要获取嵌套在名为 JamesBondCar 的类中的枚举（SpyOptions）的类型信息。为此，你将编写以下内容：

```
// 获取当前程序集中嵌套枚举的类型信息
Type t = Type.GetType("CarLibrary.JamesBondCar+SpyOptions");
```

## 构建自定义元数据查看器

为了说明反射的基本过程（以及 System.Type 的有用性），让我们创建一个名为 MyTypeViewer 的控制台应用程序项目。此程序将显示 System.Runtime.dll 中任何类型的详细信息的方法、属性、字段和支持的接口（以及其他一些感兴趣的点），或 MyTypeViewer 本身中的类型（回想一下所有 .NET Core 应用程序都自动访问此核心框架类库）。一旦应用程序创建完毕，请确保导入 System、System.Reflection 和 System.Linq 命名空间。

```
// 需要导入此命名空间来进行任何反射！
using System;
using System.Linq;
using System.Reflection;
```

## 反射方法

将向 Program 类添加几个静态方法，每个方法都接受单个 System.Type 参数并返回 void。首先是 ListMethods()，它（正如你可能猜到的）打印传入类型定义的方法的名称。注意 Type.GetMethods() 返回一个 System.Reflection.MethodInfo 对象数组，可以用标准的 foreach 循环进行枚举，如下所示：

```
// 显示类型的名称。
static void ListMethods(Type t)
{
    Console.WriteLine("***** Methods *****");
    MethodInfo[] mi = t.GetMethods();
    foreach(MethodInfo m in mi)
    {
        Console.WriteLine("-> {0}", m.Name);
    }
    Console.WriteLine();
}
```

这里，你只是使用 MethodInfo.Name 属性打印方法的名称。正如你可能猜到的，MethodInfo 还有许多其他成员，允许你确定方法是否为静态、虚拟、泛型或抽象。此外，MethodInfo 类型允许你获取方法的返回值和参数集。你将在稍后改进 ListMethods() 的实现。

如果你愿意，你也可以构建一个合适的 LINQ 查询来枚举每个方法的名称。回想一下第 13 章，LINQ to Objects 允许你构建可应用于内存对象集合的强类型查询。作为一个经验法则，每当你发现循环或决策编程逻辑块时，你都可以使用相关的 LINQ 查询。例如，你可以用 LINQ 重写之前的方法，如下所示：

```
using System.Linq;
static void ListMethods(Type t)
{
    Console.WriteLine("***** Methods *****");
    var methodNames = from n in t.GetMethods() select n.Name;
    foreach (var name in methodNames)
    {
        Console.WriteLine("-> {0}", name);
    }
    Console.WriteLine();
}
```

## 反射字段和属性

ListFields() 的实现类似。唯一值得注意的区别是调用 Type.GetFields() 和由此产生的 FieldInfo 数组。同样，为了保持简单，你只使用 LINQ 查询打印每个字段的名称。

```
// 显示类型的字段名称。
static void ListFields(Type t)
{
    Console.WriteLine("***** Fields *****");
    var fieldNames = from f in t.GetFields() select f.Name;
    foreach (var name in fieldNames)
    {
        Console.WriteLine("->{0}", name);
    }
    Console.WriteLine();
}
```

显示类型的属性的逻辑类似。

```
// 显示类型的属性名称。
static void ListProps(Type t)
{
    Console.WriteLine("**** Properties ****");
    var propNames = from p in t.GetProperties() select p.Name;
    foreach (var name in propNames)
    {
        Console.WriteLine("->{0}", name);
    }
    Console.WriteLine();
}
```

## 反射已实现的接口

接下来，你将编写一个名为 ListInterfaces() 的方法，它将打印传入类型上支持的任何接口的名称。这里唯一需要注意的是，对 GetInterfaces() 的调用返回一个 System.Types 数组！这是有道理的，因为接口确实是类型。

```
// 显示已实现的接口。
static void ListInterfaces(Type t)
{
    Console.WriteLine("**** Interfaces ****");
    var ifaces = from i in t.GetInterfaces() select i;
    foreach(Type i in ifaces)
    {
        Console.WriteLine("->{0}", i.Name);
    }
}
```

> 注意 请注意 System.Type 的大多数“get”方法（GetMethods()、GetInterfaces() 等）已被重载以允许你指定 BindingFlags 枚举的值。这提供了对应该搜索什么的更大控制（例如，仅静态成员，仅公共成员，包含私有成员等）。请查阅文档了解详细信息。

## 显示各种零碎信息

最后但并非最不重要的一点是，你有一个最终的辅助方法，它将简单地显示有关传入类型的各种统计信息（指示类型是否为泛型、基类是什么、类型是否被密封等）。

```
// 仅为好措施。
static void ListVariousStats(Type t)
{
    Console.WriteLine("***** Various Statistics *****");
    Console.WriteLine("Base class is: {0}", t.BaseType);
    Console.WriteLine("Is type abstract? {0}", t.IsAbstract);
    Console.WriteLine("Is type sealed? {0}", t.IsSealed);
    Console.WriteLine("Is type generic? {0}", t.IsGenericTypeDefinition);
    Console.WriteLine("Is type a class type? {0}", t.IsClass);
    Console.WriteLine();
}
```

## 添加顶层语句

Program.cs 文件的顶层语句提示用户输入类型的完全限定名称。一旦你获得此字符串数据，你将其传递给 Type.GetType() 方法并将提取的 System.Type 发送到每个辅助方法。此过程重复进行，直到用户按 Q 终止应用程序。

```
Console.WriteLine("***** Welcome to MyTypeViewer *****");
string typeName = "";

do
{
    Console.WriteLine("\nEnter a type name to evaluate");
    Console.Write("or enter Q to quit: ");

    // Get name of type.
    typeName = Console.ReadLine();

    // Does user want to quit?
    if (typeName.Equals("Q",StringComparison.OrdinalIgnoreCase))
    {
        break;
    }

    // Try to display type.
    try
    {
        Type t = Type.GetType(typeName);
        Console.WriteLine("");
        ListVariousStats(t);
        ListFields(t);
        ListProps(t);
        ListMethods(t);
        ListInterfaces(t);
    }
    catch
    {
        Console.WriteLine("Sorry, can't find type");
    }
} while (true);
```

