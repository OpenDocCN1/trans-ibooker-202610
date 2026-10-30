

// 通用属性。
public T X
{
    get => _xPos;
    set => _xPos = value;
}

public T Y
{
    get => _yPos;
    set => _yPos = value;
}

public override string ToString() => $"[{_xPos}, {_yPos}]"
}
}

如你所见，Point<T> 在字段数据、构造函数参数和属性定义的定义中利用了其类型参数。

## 使用泛型的默认值表达式

随着泛型的引入，C# 的 default 关键字获得了双重身份。除了在 switch 结构中使用之外，它还可以用于将类型参数设置为其默认值。这很有帮助，因为泛型类型在前期并不知道实际的占位符，这意味着它无法安全地假设默认值会是什么。类型参数的默认值如下：

- 数值类型默认为 0。
- 引用类型默认为 null。
- 结构体的字段设置为 0（对于值类型）或 null（对于引用类型）。

要重置 Point<T> 的实例，你可以直接将 X 和 Y 值设置为 0。这假设调用者只会提供数值数据。那字符串版本呢？这就是 default(T) 语法派上用场的地方。default 关键字会将变量重置为变量数据类型的默认值。添加一个名为 ResetPoint() 的方法如下：

// 将字段重置为类型参数的默认值。
// C# 中的 "default" 关键字是重载的。
// 当与泛型一起使用时，它表示类型参数的
// 默认值。
public void ResetPoint()
{
    _xPos = default(T);
    _yPos = default(T);
}
```

现在你已经有了 ResetPoint() 方法，可以充分使用 Point<T> 结构的方法了。

```
using System;
using GenericPoint;

Console.WriteLine("***** Fun with Generic Structures *****");
// 使用 int 的 Point。
Point<int> p = new Point<int>(10, 10);
Console.WriteLine("p.ToString()={0}", p.ToString());
p.ResetPoint();
Console.WriteLine("p.ToString()={0}", p.ToString());
Console.WriteLine();

// 使用 double 的 Point。
Point<double> p2 = new Point<double>(5.4, 3.3);
Console.WriteLine("p2.ToString()={0}", p2.ToString());
p2.ResetPoint();
Console.WriteLine("p2.ToString()={0}", p2.ToString());
Console.WriteLine();

// 使用字符串的 Point。
Point<string> p3 = new Point<string>("i", "3i");
Console.WriteLine("p3.ToString()={0}", p3.ToString());
p3.ResetPoint();
Console.WriteLine("p3.ToString()={0}", p3.ToString());
Console.ReadLine();
```

以下是输出结果：

```
***** Fun with Generic Structures *****
p.ToString()=[10, 10]
p.ToString()=[0, 0]

p2.ToString()=[5.4, 3.3]
p2.ToString()=[0, 0]

p3.ToString()=[i, 3i]
p3.ToString()=[, ]
```

## 默认字面量表达式（新特性 7.1）

除了设置属性的默认值之外，C# 7.1 还引入了默认字面量表达式。这消除了在 default 语句中指定变量类型的需求。将 ResetPoint() 方法更新为如下：

```
public void ResetPoint()
{
    _xPos = default;
    _yPos = default;
}
```

default 表达式不仅限于简单变量，还可以应用于复杂类型。例如，要创建并初始化 Point 结构，你可以编写如下代码：

```
Point<string> p4 = default;
Console.WriteLine("p4.ToString()={0}", p4.ToString());
Console.WriteLine();
Point<int> p5 = default;
Console.WriteLine("p5.ToString()={0}", p5.ToString());
```

## 使用泛型的模式匹配（新特性 7.1）

C# 7.1 中的另一个更新是能够对泛型进行模式匹配。以下方法检查 Point 实例基于的数据类型（或许不完整，但足以展示概念）：

```
static void PatternMatching<T>(Point<T> p)
{
    switch (p)
    {
        case Point<string> pString:
            Console.WriteLine("Point is based on strings");
            return;
        case Point<int> pInt:
            Console.WriteLine("Point is based on ints");
            return;
    }
}
```

要使用模式匹配代码，请将顶层语句更新为如下：

```
Point<string> p4 = default;
Point<int> p5 = default;
PatternMatching(p4);
PatternMatching(p5);
```

## 约束类型参数

正如本章所述，任何泛型项至少有一个类型参数，你需要在与泛型类型或成员交互时指定它。这本身就允许你构建一些类型安全的代码；然而，你也可以使用 where 关键字对给定类型参数必须是什么样子的要求变得极其具体。

使用此关键字，你可以为给定类型参数添加一组约束，C# 编译器将在编译时检查这些约束。具体而言，你可以按表 10-8 所描述的方式约束类型参数。

## 表 10-8. 泛型类型参数的可能约束

| 泛型约束 | 实际含义 |
|---|---|
| where T : struct | 类型参数 <T> 必须在其继承链中具有 System.ValueType（即 <T> 必须是结构体）。 |
| where T : class | 类型参数 <T> 必须不在其继承链中具有 System.ValueType（即 <T> 必须是引用类型）。 |
| where T : new() | 类型参数 <T> 必须具有默认构造函数。这在你的泛型类型必须创建类型参数的实例时很有帮助，因为你无法假设自定义构造函数的格式。注意，此约束必须列在多重约束类型的最后。 |
| where T : NameOfBaseClass | 类型参数 <T> 必须派生自 NameOfBaseClass 指定的类。 |
| where T : NameOfInterface | 类型参数 <T> 必须实现 NameOfInterface 指定的接口。你可以用逗号分隔的列表指定多个接口。 |

除非你需要构建一些极其类型安全的自定义集合，否则你可能永远不需要在 C# 项目中使用 where 关键字。无论如何，以下几个（部分）代码示例说明了如何使用 where 关键字。

## 使用 where 关键字的示例

首先假设你创建了一个自定义泛型类，并且你希望确保类型参数具有默认构造函数。这在自定义泛型类需要创建 T 的实例时很有用，因为默认构造函数是所有类型都可能共有的唯一构造函数。此外，以这种方式约束 T 让你可以进行编译时检查；如果 T 是引用类型，程序员记得在类定义中重新定义默认值（你可能还记得，当你定义自己的构造函数时，默认构造函数会在类中被移除）。

```
// MyGenericClass 派生自 object，而
// 包含的项必须具有默认 ctor。
public class MyGenericClass<T> where T : new()
{
    ...
}
```

注意 where 子句指定了正在被约束的类型参数，后跟冒号运算符。在冒号运算符之后，你列出每个可能的约束（在本例中为默认构造函数）。这里是另一个示例：

// MyGenericClass 派生自 object，而
// 包含的项必须是实现 IDrawable 的类
// 并且必须支持默认 ctor。
public class MyGenericClass<T> where T : class, IDrawable, new()
{
    ...
}
```

在本例中，T 有三个要求。它必须是引用类型（不是结构体），用 class 标记表示。其次，T 必须实现 IDrawable 接口。第三，它还必须具有默认构造函数。多个约束以逗号分隔的列表列出；然而，你应该注意 new() 约束必须始终列在最后！因此，以下代码将无法编译：

// 错误！new() 约束必须列在最后！
public class MyGenericClass<T> where T : new(), class, IDrawable
{
    ...
}
```

如果你曾经创建自定义泛型集合类并指定多个类型参数，你可以使用单独的 where 子句为每个参数指定唯一的一组约束。

```
// <K> 必须扩展 SomeBaseClass 并具有默认 ctor，
// 而 <T> 必须是结构体并实现
// 泛型 IComparable 接口。
public class MyGenericClass<K, T> where K : SomeBaseClass, new()
    where T : struct, IComparable<T>
{
    ...
}
```

你很少会遇到需要构建完整的自定义泛型集合类的情况；然而，你也可以在泛型方法上使用 where 关键字。例如，如果你想指定你的泛型 Swap<T>() 方法只能在结构体上操作，你将像这样更新该方法：

// 此方法将交换任何结构体，但不交换类。
static void Swap<T>(ref T a, ref T b) where T : struct
{
    ...
}
```

注意，如果你以这种方式约束 Swap() 方法，你将不再能够交换字符串对象（如示例代码所示），因为 string 是引用类型。

## 操作符约束的缺失

我想在本章结束时再做一个关于泛型方法和约束的评论。当创建泛型方法时，如果你在类型参数上应用任何 C# 运算符（+、-、*、== 等），你会收到编译器错误。例如，想象一个可以对泛型类型进行加、减、乘、除的类的用处。

```
// 编译器错误！不能对类型参数应用
// 运算符！
public class BasicMath<T>
{
    public T Add(T arg1, T arg2)
    { return arg1 + arg2; }
    public T Subtract(T arg1, T arg2)
    { return arg1 - arg2; }
}
```
