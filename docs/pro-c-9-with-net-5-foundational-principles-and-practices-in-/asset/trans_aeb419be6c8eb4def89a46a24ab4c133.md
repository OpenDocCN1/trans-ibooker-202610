

虽然 System.Array 并未直接实现 IEnumerable<T> 接口，但它通过静态 System.Linq.Enumerable 类间接获得了该类型所需的功能（以及许多其他以 LINQ 为核心的成员）。

这个实用类定义了许多泛型扩展方法（例如 Aggregate<T>()、First<T>()、Max<T>() 等），System.Array（以及其他类型）会在后台获取这些方法。因此，如果对局部变量 currentVideoGames 使用点运算符，就会发现许多在 System.Array 正式定义中找不到的成员。

## 延迟执行的作用

关于 LINQ 查询表达式，还有一点很重要：当它们返回一个序列时，只有在你遍历该序列时，查询才会真正执行。严格来说，这称为延迟执行。这种方式的好处是，你可以对同一个容器多次应用相同的 LINQ 查询，并确信每次都能获取最新、最准确的结果。请看对 QueryOverInts() 方法所做的以下修改：

```
static void QueryOverInts()
{
    int[] numbers = { 10, 20, 30, 40, 1, 2, 3, 8 };

    // 获取小于十的数字。
    var subset = from i in numbers where i < 10 select i;

    // 在此处执行 LINQ 语句！
    foreach (var i in subset)
    {
        Console.WriteLine("{0} < 10", i);
    }
    Console.WriteLine();
    // 更改数组中的一些数据。
    numbers[0] = 4;

    // 再次执行！
    foreach (var j in subset)
    {
        Console.WriteLine("{0} < 10", j);
    }

    Console.WriteLine();
    ReflectOverQueryResults(subset);
}
```

> 注意：当 LINQ 语句选择单个元素时（使用 First/FirstOrDefault、Single/SingleOrDefault 或任何聚合方法），查询会立即执行。First、FirstOrDefault、Single 和 SingleOrDefault 将在下一节介绍。聚合方法将在本章后面介绍。

如果再次运行该程序，就会得到以下输出。请注意，第二次遍历请求的序列时，会发现一个额外的成员，因为你将数组中的第一个元素设为小于十的值。

```
1 < 10
2 < 10
3 < 10
8 < 10

4 < 10
1 < 10
2 < 10
3 < 10
8 < 10
```

Visual Studio 的一个实用功能是：如果在 LINQ 查询求值之前设置断点，就可以在调试会话中查看其内容。只需将鼠标光标悬停在 LINQ 结果集变量上（图 13-1 中的 subset）。此时，展开“Results View”（结果视图）选项，即可选择立即对查询求值。

![](img/587021f4d5ba039be70a11107130377b_549_0.png)

图 13-1 调试 LINQ 表达式

## 立即执行的作用

如果需要在 foreach 逻辑范围之外对返回序列的 LINQ 表达式求值，可以调用 Enumerable 类型定义的各种扩展方法，例如 ToArray<T>()、ToDictionary<TSource, TKey>() 和 ToList<T>()。这些方法会在调用时立即执行 LINQ 查询，从而获取数据快照。完成此操作后，就可以独立处理该数据快照。

此外，如果只需要单个元素，查询也会立即执行。First() 返回序列中的第一个成员（应始终与 orderby 一起使用）。如果没有可返回的项，例如原始序列为空或 where 子句筛除了所有元素，FirstOrDefault() 会返回列表中元素类型的默认值。Single() 也会返回序列中的第一个成员（依据 orderby 排序；如果没有 orderby 子句，则依据元素顺序）。与名称相似的对应方法一样，如果序列中没有任何项（或 where 子句筛除了所有记录），SingleOrDefault() 会返回元素类型的默认值。FirstOrDefault() 与 SingleOrDefault() 的区别在于：如果查询将返回多个元素，SingleOrDefault() 会引发异常。

```
static void ImmediateExecution()
{
    Console.WriteLine();
    Console.WriteLine("Immediate Execution");
    int[] numbers = { 10, 20, 30, 40, 1, 2, 3, 8 };

    // 获取序列顺序中的第一个元素
    int number = (from i in numbers select i).First();
    Console.WriteLine("First is {0}", number);

    // 获取查询顺序中的第一个元素
    number = (from i in numbers orderby i select i).First();
    Console.WriteLine("First is {0}", number);

    // 获取与查询匹配的那个元素
    number = (from i in numbers where i > 30 select i).Single();
    Console.WriteLine("Single is {0}", number);

    try
    {
        // 如果有多个元素通过查询，则引发异常
        number = (from i in numbers where i > 10 select i).Single();
    }
    catch (Exception ex)
    {
        Console.WriteLine("An exception occurred: {0}", ex.Message);
    }
    // 立即获取数据，类型为 int[]。
    int[] subsetAsIntArray =
        (from i in numbers where i < 10 select i).ToArray<int>();

    // 立即获取数据，类型为 List<int>。
    List<int> subsetAsListOfInts =
        (from i in numbers where i < 10 select i).ToList<int>();
}
```

注意，整个 LINQ 表达式都用括号括起来，以便将其转换为正确的底层类型（无论该类型是什么），从而调用 Enumerable 的扩展方法。

还要回想一下第 10 章的内容：如果 C# 编译器能够明确推断出泛型的类型参数，就无需指定该类型参数。因此，也可以像下面这样调用 ToArray<T>()（或者 ToList<T>()）：

```
int[] subsetAsIntArray =
    (from i in numbers where i < 10 select i).ToArray();
```

当你需要将 LINQ 查询的结果返回给外部调用方时，立即执行的用处显而易见。巧合的是，这正是本章接下来要讨论的主题。

## 返回 LINQ 查询的结果

可以在类（或结构）中定义一个字段，其值为 LINQ 查询的结果。不过，要做到这一点，不能使用隐式类型（因为 var 关键字不能用于字段），并且 LINQ 查询的目标不能是实例级数据，因此必须是静态数据。鉴于这些限制，你很少需要编写如下代码：

```
class LINQBasedFieldsAreClunky
{
    private static string[] currentVideoGames =
        {"Morrowind", "Uncharted 2",
        "Fallout 3", "Daxter", "System Shock 2"};

    // 这里不能使用隐式类型！必须知道 subset 的类型！
    private IEnumerable<string> subset =
        from g in currentVideoGames
        where g.Contains(" ")
        orderby g
        select g;

    public void PrintGames()
    {
        foreach (var item in subset)
        {
            Console.WriteLine(item);
        }
    }
}
```

LINQ 查询通常定义在方法或属性的作用域内。此外，为了简化编程，通常会使用 var 关键字，将结果集存储在隐式类型的局部变量中。回想一下第 3 章的内容：隐式类型变量不能用于定义类或结构的参数、返回值或字段。

鉴于这一点，你可能会想，究竟该如何将查询结果返回给外部调用方？答案是：视情况而定。如果结果集包含强类型数据，例如字符串数组或 Cars 的 List<T>，就可以不使用 var 关键字，而改用适当的 IEnumerable<T> 或 IEnumerable 类型（同样，IEnumerable<T> 扩展了 IEnumerable）。请看下面这个名为 LinqRetValues 的新控制台应用程序示例：

```
using System;
using System.Collections.Generic;
using System.Linq;

Console.WriteLine("***** LINQ 返回值 ****\n");
IEnumerable<string> subset = GetStringSubset();

foreach (string item in subset)
{
    Console.WriteLine(item);
}
```

