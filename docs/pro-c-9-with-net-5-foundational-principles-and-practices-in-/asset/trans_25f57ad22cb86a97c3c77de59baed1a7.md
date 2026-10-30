

这些是元组的两个重要考虑因素：

- 字段不会被验证。
- 你无法定义自己的方法。

它们被设计为仅作为轻量级的数据传输机制。

## 开始使用元组

理论够了。让我们写一些代码！要创建元组，只需将要分配给元组的值用括号括起来，如下所示：

```
("a", 5, "c")
```

请注意，它们不一定都是相同的数据类型。圆括号结构也用于将元组分配给变量（或者你可以使用 var 关键字，编译器会为你分配数据类型）。要将前面的示例分配给变量，以下两行实现了相同的效果。values 变量将是一个元组，其中包含两个字符串属性和一个夹在中间的 int 属性。

```
(string, int, string) values = ("a", 5, "c");
var values = ("a", 5, "c");
```

默认情况下，编译器为每个属性分配名称 ItemX，其中 X 表示元组中基于一的位置。对于前面的示例，属性名称为 Item1、Item2 和 Item3。访问它们的方式如下：

```
Console.WriteLine($"First item: {values.Item1}");
Console.WriteLine($"Second item: {values.Item2}");
Console.WriteLine($"Third item: {values.Item3}");
```

也可以在语句的右侧或左侧为元组中的每个属性添加特定名称。虽然在语句的两侧同时指定名称不会导致编译错误，但如果这样做，右侧的名称将被忽略，只使用左侧的名称。以下两行代码展示了在左侧和右侧设置名称以达到相同目的：

```
(string FirstLetter, int TheNumber, string SecondLetter) valuesWithNames = ("a", 5, "c");
var valuesWithNames2 = (FirstLetter: "a", TheNumber: 5, SecondLetter: "c");
```

现在可以使用字段名称以及 ItemX 记法来访问元组上的属性，如以下代码所示：

```
Console.WriteLine($"First item: {valuesWithNames.FirstLetter}");
Console.WriteLine($"Second item: {valuesWithNames.TheNumber}");
Console.WriteLine($"Third item: {valuesWithNames.SecondLetter}");
//Using the item notation still works!
Console.WriteLine($"First item: {valuesWithNames.Item1}");
Console.WriteLine($"Second item: {valuesWithNames.Item2}");
Console.WriteLine($"Third item: {valuesWithNames.Item3}");
```

请注意，当在右侧设置名称时，你必须使用关键字 var 来声明变量。显式设置数据类型（即使没有自定义名称）会触发编译器使用左侧，分配属性使用 ItemX 记法，并忽略右侧设置的任何自定义名称。以下两个示例忽略了 Custom1 和 Custom2 名称：

```
(int, int) example = (Custom1:5, Custom2:7);
(int Field1, int Field2) example = (Custom1:5, Custom2:7);
```

还需要注意的是，自定义字段名称仅在编译时存在，在使用反射检查元组时在运行时不可用（反射在第 17 章中介绍）。

元组也可以嵌套为元组中的元组。由于元组中的每个属性都是数据类型，而元组本身也是数据类型，因此以下代码完全合法：

```
Console.WriteLine("=> Nested Tuples");
var nt = (5, 4, ("a", "b"));
```

## 使用推断变量名（更新 7.1）

C# 7.1 对元组的更新是 C# 能够推断元组的变量名，如下所示：

```
Console.WriteLine("=> Inferred Tuple Names");
var foo = new {Prop1 = "first", Prop2 = "second"};
var bar = (foo.Prop1, foo.Prop2);
Console.WriteLine($"{bar.Prop1};{bar.Prop2}");
```

## 理解元组相等/不相等（新增 7.3）

C# 7.1 中新增的功能是元组相等（==）和不相等（!=）。在测试不相等时，比较运算符会对元组中的数据类型执行隐式转换，包括比较可空和不可空的元组和/或属性。这意味着以下测试完全有效，尽管 int/long 之间存在差异：

```
Console.WriteLine("=> Tuples Equality/Inequality");
// lifted conversions
var left = (a: 5, b: 10);
(int? a, int? b) nullableMembers = (5, 10);
Console.WriteLine(left == nullableMembers); // Also true
// converted type of left is (long, long)
(long a, long b) longTuple = (5, 10);
Console.WriteLine(left == longTuple); // Also true
// comparisons performed on (long, long) tuples
(long a, int b) longFirst = (5, 10);
(int a, long b) longSecond = (5, 10);
Console.WriteLine(longFirst == longSecond); // Also true
```

包含元组的元组也可以进行比较，但前提是它们具有相同的形状。你不能将三个 int 属性的元组与两个 int 和一个元组的元组进行比较。

## 理解元组作为方法返回值

本章前面使用 out 参数从方法调用返回多个值。还有其他方法可以做到这一点，例如专门创建类或结构来返回这些值。但如果这个类或结构仅用于作为一个方法的数据传输，那就是额外的工作和不需要开发的额外代码。元组非常适合此任务，轻量级，易于声明和使用。

这是来自 out 参数部分的示例之一。它返回三个值，但需要将三个参数作为传输机制传递给调用代码。

```
static void FillTheseValues(out int a, out string b, out bool c)
{
    a = 9;
    b = "Enjoy your string.";
    c = true;
}
```

使用元组，你可以去掉参数并仍然获得这三个值。

```
static (int a,string b,bool c) FillTheseValues()
{
    return (9,"Enjoy your string.",true);
}
```

调用此方法与调用任何其他方法一样简单。

```
var samples = FillTheseValues();
Console.WriteLine($"Int is: {samples.a}");
Console.WriteLine($"String is: {samples.b}");
Console.WriteLine($"Boolean is: {samples.c}");
```

也许更好的例子是将全名分解为其各个部分（名、中间名、姓）。以下代码接收全名并返回包含不同部分的元组：

```
static (string first, string middle, string last) SplitNames(string fullName)
{
    //do what is needed to split the name apart
    return ("Philip", "F", "Japikse");
}
```

## 理解元组中的丢弃

延续 SplitNames() 示例，假设你只知道需要名字和姓氏，而不关心中间名。通过为你想要返回的值提供变量名，并用下划线（_）占位符填充不需要的值，可以像这样细化返回值：

```
var (first, _, last) = SplitNames("Philip F Japikse");
Console.WriteLine($"{first}:{last}");
```

元组的中间名值被丢弃。

## 理解元组模式匹配 switch 表达式（新增 8.0）

现在你对元组有了透彻的理解，是时候重新审视第 3 章中的元组 switch 表达式了。以下是示例再次：

```
//Switch expression with Tuples
static string RockPaperScissors(string first, string second)
{
    return (first, second) switch
    {
        ("rock", "paper") => "Paper wins.",
        ("rock", "scissors") => "Rock wins.",
        ("paper", "rock") => "Paper wins.",
        ("paper", "scissors") => "Scissors wins.",
        ("scissors", "rock") => "Rock wins.",
        ("scissors", "paper") => "Scissors wins.",
        (_, _) => "Tie.",
    };
}
```

在此示例中，两个参数在传递到 switch 表达式时被转换为元组。相关值在 switch 表达式中表示，任何其他情况都由最终元组处理，该元组由两个丢弃组成。

RockPaperScissors() 方法签名也可以写成接收元组，如下所示：

```
static string RockPaperScissors(
    (string first, string second) value)
{
    return value switch
    {
        //omitted for brevity
    };
}
```

## 解构元组

解构是指将元组的属性分离出来单独使用这一术语。FillTheseValues() 就是这样做的。但这种模式还有另一种用途可能有所帮助，那就是解构自定义类型。

取本章前面使用过的 Point 结构的简短版本。添加了一个名为 Deconstruct() 的新方法，用于将 Point 实例的各个属性作为具有 XPos 和 YPos 属性的元组返回。

```
struct Point
{
    // Fields of the structure.
    public int X;
    public int Y;
```

