
```csharp
static void ChangeDynamicDataType()
{
    // Declare a single dynamic data point
    // named "t".
    dynamic t = "Hello!";
    Console.WriteLine("t is of type: {0}", t.GetType());

    t = false;
    Console.WriteLine("t is of type: {0}", t.GetType());

    t = new List<int>();
    Console.WriteLine("t is of type: {0}", t.GetType());
}
```

```
t is of type: System.String
t is of type: System.Boolean
t is of type: System.Collections.Generic.List`1[System.Int32]
```

在目前的研究中，请注意，如果你将变量 `t` 声明为 `System.Object` 类型，前面的代码也会以完全相同的方式编译和执行。然而，你很快就会看到，`dynamic` 关键字提供了许多额外的功能。

## 在动态声明的数据上调用成员

鉴于动态变量可以随时采用任何类型的身份（就像 `System.Object` 类型的变量一样），你接下来可能会想到的问题是：如何在动态变量上调用成员（属性、方法、索引器和事件等）。嗯，从语法上讲，它看起来也没有什么不同。只需将点运算符应用于动态数据变量，指定一个公共成员，并提供任何参数（如果需要）。

然而（这是一个非常重要的“然而”），编译器不会检查你所指定的成员是否有效！请记住，与定义为 `System.Object` 的变量不同，动态数据不是静态类型的。直到运行时，你才会知道你调用的动态数据是否支持指定的成员、是否传入了正确的参数、是否正确地拼写了成员，等等。因此，尽管看起来可能很奇怪，但下面的方法完全可以编译通过：

```csharp
static void InvokeMembersOnDynamicData()
{
    dynamic textData1 = "Hello";
    Console.WriteLine(textData1.ToUpper());

    // You would expect compiler errors here!
    // But they compile just fine.
    Console.WriteLine(textData1.toupper());
    Console.WriteLine(textData1.Foo(10, "ee", DateTime.Now));
}
```

注意第二次调用 `WriteLine()` 试图在动态数据点上调用名为 `toupper()` 的方法（请注意这里的大小写错误——应该是 `ToUpper()`）。如你所见，`textData1` 是 `string` 类型，因此，你知道它没有这个全小写形式的名字的方法。此外，`string` 当然也没有一个名为 `Foo()` 的方法，能接受 `int`、`string` 和 `DateTime` 对象！

尽管如此，C# 编译器仍然感到满意。然而，如果你从 `Main()` 中调用此方法，你会得到如下输出所示的运行时错误：

```
Unhandled Exception: Microsoft.CSharp.RuntimeBinder.RuntimeBinderException:
'string' does not contain a definition for 'toupper'
```

在动态数据和强类型数据上调用成员之间的另一个明显区别是，当你对一段动态数据应用点运算符时，你不会看到预期的 Visual Studio IntelliSense。IDE 将允许你输入你能想到的任何成员名称。

动态数据无法使用 IntelliSense，这是有道理的。但是，请记住，这意味着你在处理此类数据点时输入 C# 代码需要格外小心。任何拼写错误或成员大小写不正确都会引发运行时错误，具体来说是 `RuntimeBinderException` 类的实例。

`RuntimeBinderException` 表示当你试图在动态数据类型上调用一个实际上不存在的成员时（如 `toupper()` 和 `Foo()` 方法的情况）将引发的错误。如果你向确实存在的成员指定了错误的参数数据，也会引发同样的错误。

由于动态数据非常不稳定，每当你对使用 C# `dynamic` 关键字声明的变量调用成员时，你可以将调用包装在适当的 `try/catch` 块中，并以优雅的方式处理错误，如下所示：

```csharp
static void InvokeMembersOnDynamicData()
{
    dynamic textData1 = "Hello";

    try
    {
        Console.WriteLine(textData1.ToUpper());
        Console.WriteLine(textData1.toupper());
        Console.WriteLine(textData1.Foo(10, "ee", DateTime.Now));
    }
    catch (Microsoft.CSharp.RuntimeBinder.RuntimeBinderException ex)
    {
        Console.WriteLine(ex.Message);
    }
}
```

如果你再次调用此方法，你会发现对 `ToUpper()`（注意大写 T 和 U）的调用可以正常工作；但是，随后你会发现错误数据显示在控制台上。

```
HELLO
'string' does not contain a definition for 'toupper'
```

当然，将所有动态方法调用包装在 `try/catch` 块中的过程相当繁琐。如果你注意拼写和参数传递，就不需要这样做。然而，当你可能无法预先知道目标类型上是否存在某个成员时，捕获异常会很方便。

## `dynamic` 关键字的适用范围

回想一下，隐式类型数据（使用 `var` 关键字声明）只能用于成员作用域中的局部变量。`var` 关键字永远不能用作返回值、参数或类/结构的成员。然而，`dynamic` 关键字并非如此。考虑以下类定义：

```csharp
namespace DynamicKeyword
{
    class VeryDynamicClass
    {
        // A dynamic field.
        private static dynamic _myDynamicField;

        // A dynamic property.
        public dynamic DynamicProperty { get; set; }

        // A dynamic return type and a dynamic parameter type.
        public dynamic DynamicMethod(dynamic dynamicParam)
        {
            // A dynamic local variable.
            dynamic dynamicLocalVar = "Local variable";

            int myInt = 10;

            if (dynamicParam is int)
            {
                return dynamicLocalVar;
            }
            else
            {
                return myInt;
            }
        }
    }
}
```

现在你可以按预期调用公共成员；然而，由于你操作的是动态方法和属性，你无法完全确定数据类型将是什么！确切地说，`VeryDynamicClass` 定义在现实世界的应用程序中可能没什么用处，但它确实说明了可以在哪些范围内应用这个 C# 关键字。

## `dynamic` 关键字的限制

虽然可以使用 `dynamic` 关键字定义许多事物，但它的使用存在一些限制。虽然这些限制不是致命的障碍，但要知道，动态数据项在调用方法时不能使用 lambda 表达式或 C# 匿名方法。例如，即使目标方法确实接受一个委托参数，该委托接受一个字符串值并返回 `void`，以下代码也总是会导致错误：

```csharp
dynamic a = GetDynamicObject();

// Error! Methods on dynamic data can't use lambdas!
a.Method(arg => Console.WriteLine(arg));
```

要规避此限制，你需要直接使用底层委托，采用第 12 章中描述的技术。另一个限制是，动态数据点无法理解任何扩展方法（见第 11 章）。不幸的是，这还将包括来自 LINQ API 的任何扩展方法。因此，使用 `dynamic` 关键字声明的变量在 LINQ to Objects 和其他 LINQ 技术中的使用受到限制。

```csharp
dynamic a = GetDynamicObject();
// Error! Dynamic data can't find the Select() extension method!
var data = from d in a select d;
```

考虑到动态数据不是强类型、在编译时不检查、无法触发 IntelliSense，并且不能作为 LINQ 查询的目标，你完全有理由认为仅仅为了使用而使用 `dynamic` 关键字是一种糟糕的编程实践。

然而，在少数情况下，`dynamic` 关键字可以大幅减少你需要手工编写的代码量。具体来说，如果你正在构建一个大量使用后期绑定（通过反射）的 .NET Core 应用程序，`dynamic` 关键字可以为你节省输入时间。同样，如果你正在构建一个需要与旧版 COM 库（如 Microsoft Office 产品）通信的 .NET Core 应用程序，你可以通过 `dynamic` 关键字大大简化你的代码库。最后举个例子，使用 ASP.NET Core 构建的 Web 应用程序经常使用 `ViewBag` 类型，也可以通过 `dynamic` 关键字以简化的方式访问它。

> 注意：COM 交互严格来说是 Windows 的范式，会移除应用程序的跨平台能力。

就像任何“捷径”一样，你需要权衡利弊。使用 `dynamic` 关键字是在代码简洁性和类型安全之间进行权衡。虽然 C# 本质上是一种强类型语言，但你可以在每次调用的基础上选择启用（或禁用）动态行为。永远记住，你并不需要使用 `dynamic` 关键字。你总是可以通过手工编写替代代码来达到相同的最终结果（而且通常会多得多）。

## 动态语言运行时的作用

既然你现在更好地理解了“动态数据”是怎么回事，让我们来了解它是如何被处理的。自 .NET 4.0 发布以来，公共语言运行时（CLR）增加了一个互补的运行时环境，称为动态语言运行时。“动态运行时”的概念当然并不新鲜。事实上，许多编程语言如 JavaScript、LISP、Ruby 和 Python 已经使用它多年了。简而言之，动态运行时允许动态语言在运行时完全发现类型，而无需编译时检查。

> 注意：虽然 DLR 的大部分内容已被移植到 .NET Core（从 3.0 开始），但 .NET Core 5 中的 DLR 与 .NET 4.8 中的 DLR 之间尚未实现功能对等。
