

var choice = int.TryParse(langChoice.ToString(), out int c) ? c : langChoice;

switch (choice)
{
    case int i when i == 2:
    case string s when s.Equals("VB", StringComparison.OrdinalIgnoreCase):
        Console.WriteLine("VB: OOP, multithreading, and more!");
        break;
    case int i when i == 1:
    case string s when s.Equals("C#", StringComparison.OrdinalIgnoreCase):
        Console.WriteLine("Good choice, C# is a fine language.");
        break;
    default:
        Console.WriteLine("Well...good luck with that!");
        break;
}
Console.WriteLine();
}

这为 switch 语句增添了一个新的维度，因为 case 语句的顺序现在变得重要了。使用常量模式时，每个 case 语句必须是唯一的。使用类型模式时则不再如此。例如，以下代码将在第一个 case 语句中匹配每一个整数，并且永远不会执行第二个或第三个（实际上，以下代码将无法编译）：

```
switch (choice)
{
    case int i:
        //do something
        break;
    case int i when i == 0:
        //do something
        break;
    case int i when i == -1:
        // do something
        break;
}
```

在 C# 7 首次发布时，使用泛型类型进行模式匹配时存在一个小问题。这已在 C# 7.1 中得到解决。泛型类型将在第 10 章中介绍。

注意 C# 9.0 中之前演示过的所有模式匹配改进也可在 switch 语句中使用。

## 使用 switch 表达式（新于 8.0）

C# 8 中新增了 switch 表达式，允许在简洁的语句中为变量赋值。考虑一下该方法在 C# 7 中的版本，该方法接收一个颜色并返回该颜色名称的十六进制值：

```
static string FromRainbowClassic(string colorBand)
{
    switch (colorBand)
    {
        case "Red":
            return "#FF0000";
        case "Orange":
            return "#FF7F00";
        case "Yellow":
            return "#FFFF00";
        case "Green":
            return "#00FF00";
        case "Blue":
            return "#0000FF";
        case "Indigo":
            return "#4B0082";
        case "Violet":
            return "#9400D3";
        default:
            return "#FFFFFF";
    };
}
```

使用 C# 8 中的新 switch 表达式，前述方法可以写成如下形式，更加简洁：

```
static string FromRainbow(string colorBand)
{
    return colorBand switch
    {
        "Red" => "#FF0000",
        "Orange" => "#FF7F00",
        "Yellow" => "#FFFF00",
        "Green" => "#00FF00",
        "Blue" => "#0000FF",
        "Indigo" => "#4B0082",
        "Violet" => "#9400D3",
        _ => "#FFFFFF",
    };
}
```

这个示例有很多内容需要解析，从 lambda（=>）语句到丢弃（_）。这些内容将在后面的章节中全部介绍，该示例也将在后续章节中更详细地介绍。

在结束 switch 表达式主题之前还有一个示例，它涉及元组。元组在第 4 章中有详细介绍，因此现在请将元组视为一个简单的构造，它保存一个以上的值并用括号定义，像这样保存一个字符串和一个 int 的元组：

```
(string, int)
```

在下面的示例中，传递给 RockPaperScissors 方法的两个值被转换为一个元组，然后 switch 表达式在单个表达式中评估这两个值。这种模式允许在 switch 语句期间比较一个以上的值。

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

要调用此方法，请在 Main() 方法中添加以下代码行：

```
Console.WriteLine(RockPaperScissors("paper", "rock"));
Console.WriteLine(RockPaperScissors("scissors", "rock"));
```

当在第 4 章介绍元组时，此示例将被重新审视。

## 摘要

本章的目的是让你接触 C# 编程语言的众多核心方面。你检查了在你可能感兴趣的任何应用程序中常见的构造。在检查了应用程序对象的角色之后，你了解到每个 C# 可执行程序必须有一个定义 Main() 方法的类型，要么是显式定义，要么是通过使用顶级语句。这方法充当程序的入口点。

接下来，你深入研究了 C# 内置数据类型的细节，并了解到每个数据类型关键字（例如 int）实际上是 System 命名空间中完整类型的简写表示法（System.Int32）。由此，每个 C# 数据类型都有一些内置成员。同样，你还了解了 widening 和 narrowing 的作用，以及 checked 和 unchecked 关键字的角色。

本章通过介绍使用 var 关键字的隐式类型化来收尾。如讨论所述，隐式类型化的最有用之处在于使用 LINQ 编程模型时。最后，你快速检查了 C# 支持的各种迭代和决策构造。

现在你已经了解了一些基本原理，下一章（第 4 章）将完成你对核心语言特性的检查。在那之后，你将完全准备好从第 5 章开始检查 C# 的面向对象特性。

## 第 4 章

## 核心 C# 编程构造，第 2 部分

本章在第 3 章结束的地方继续，并完成了你对 C# 编程语言核心方面的调查。你将首先调查使用 C# 语法操作数组的细节，并熟悉相关的 System.Array 类类型中包含的功能。

接下来，你将检查 C# 方法构造的各个细节，探索 out、ref 和 params 关键字。在此过程中，你还将检查可选参数和命名参数的角色。我以方法重载的介绍来结束对方法的讨论。

接下来，本章讨论枚举类型和结构类型的构造，包括对值类型和引用类型之间区别的详细检查。本章通过检查可空数据类型的角色和相关的运算符来收尾。

完成本章后，你将处于完美的位置开始学习 C# 的面向对象功能，从第 5 章开始。

## 了解 C# 数组

正如我猜你已经知道的那样，数组是一组使用数字索引访问的数据项。更具体地说，数组是同一类型的一组连续数据点（int 的数组、string 的数组、SportsCars 的数组等）。使用 C# 声明、填充和访问数组都非常简单。为了说明这一点，创建一个名为 FunWithArrays 的新控制台应用程序项目，其中包含一个名为 SimpleArrays() 的辅助方法；如下所示：

```
Console.WriteLine("***** Fun with Arrays *****");
SimpleArrays();
Console.ReadLine();
```

```
static void SimpleArrays()
{
    Console.WriteLine("=> Simple Array Creation.");
    // Create and fill an array of 3 integers
    int[] myInts = new int[3];
    // Create a 100 item string array, indexed 0 - 99
    string[] booksOnDotNet = new string[100];
    Console.WriteLine();
}
```

仔细查看之前的代码注释。当使用此语法声明 C# 数组时，数组声明中使用的数字表示项目总数，而不是上界。还请注意，数组的下界始终从 0 开始。因此，当你写 int[] myInts = new int[3] 时，你最终得到一个包含三个元素的数组，索引位置为 0、1 和 2。

在定义数组变量后，你就可以按索引逐个填充元素，如下所示更新的 SimpleArrays() 方法所示：

```
static void SimpleArrays()
{
    Console.WriteLine("=> Simple Array Creation.");
    // Create and fill an array of 3 Integers
    int[] myInts = new int[3];
    myInts[0] = 100;
    myInts[1] = 200;
    myInts[2] = 300;

    // Now print each value.
    foreach(int i in myInts)
    {
        Console.WriteLine(i);
    }
    Console.WriteLine();
}
```

注意 请注意，如果你声明了一个数组但没有显式填充每个索引，每个项目将被设置为数据类型的默认值（例如，bool 的数组将被设置为 false 或 int 的数组将被设置为 0）。

## 查看 C# 数组初始化语法

除了逐个填充数组元素外，你还可以使用 C# 数组初始化语法填充数组的项目。为此，请在花括号（{}）范围内指定每个数组项目。当你创建已知大小的数组并希望快速指定初始值时，此语法会很有帮助。例如，考虑以下替代数组声明：

```
static void ArrayInitialization()
{
    Console.WriteLine("=> Array Initialization.");

    // Array initialization syntax using the new keyword.
    string[] stringArray = new string[]
    { "one", "two", "three" };
    Console.WriteLine("stringArray has {0} elements", stringArray.Length);

    // Array initialization syntax without using the new keyword.
    bool[] boolArray = { false, false, true };
    Console.WriteLine("boolArray has {0} elements", boolArray.Length);

    // Array initialization with new keyword and size.
    int[] intArray = new int[4] { 20, 22, 23, 0 };
}
```
