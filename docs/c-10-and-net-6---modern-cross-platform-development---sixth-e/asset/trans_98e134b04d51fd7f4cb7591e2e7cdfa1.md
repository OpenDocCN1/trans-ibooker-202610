
无法仅使用查询表达式语法编写跳过并获取的查询，因此我们可以使用全部扩展方法来编写查询，如以下代码所示：

```
var query = names
    .Where(name => name.Length > 4)
    .Skip(80)
    .Take(10);
```

或者可以将查询表达式语法用括号括起来，然后切换到使用扩展方法，如以下代码所示：

```
var query = (from name in names
    where name.Length > 4
    select name)
    .Skip(80)
    .Take(10);
```

> **良好实践：** 学习使用 lambda 表达式的扩展方法以及查询表达式语法编写 LINQ 查询的两种方式，因为你很可能会需要维护同时使用这两种方式的代码。

## 使用并行 LINQ 进行多线程

默认情况下，执行 LINQ 查询仅使用一个线程。**并行 LINQ (PLINQ)** 是一种轻松启用多个线程执行 LINQ 查询的方法。

> **良好实践：** 不要假设使用并行线程会提高应用程序的性能。始终测量实际的运行时间和资源使用情况。

## 创建受益于多线程的应用程序

为了实际演示，我们将从一段仅使用单线程为 45 个整数计算斐波那契数的代码开始。我们将使用 Stopwatch 类型来衡量性能变化。

我们将使用操作系统工具来监控 CPU 和 CPU 核心的使用情况。如果您没有多个 CPU 或至少多个核心，那么这个练习不会显示太多内容！

- 1. 使用您喜欢的代码编辑器，在 Chapter11 解决方案/工作区中添加一个名为 LinqInParallel 的新控制台应用程序。
- 2. 在 Visual Studio Code 中，将 LinqInParallel 选择为活动的 OmniSharp 项目。
- 3. 在 Program.cs 中，删除现有语句，然后导入 System.Diagnostics 命名空间以便我们可以使用 Stopwatch 类型，并静态导入 System.Console 类型。
- 4. 添加语句以创建秒表记录时间，在启动计时器前等待按键，创建 45 个整数，计算每个整数的最后一个斐波那契数，停止计时器并显示经过的毫秒数，如以下代码所示：
- 5. 运行代码，但不要立即按 Enter 启动秒表，因为我们需要确保监控工具正在显示处理器活动。

```
Stopwatch watch = new();
Write("Press ENTER to start. ");
ReadLine();
watch.Start();

int max = 45;

IEnumerable<int> numbers = Enumerable.Range(start: 1, count: max);

WriteLine($"Calculating Fibonacci sequence up to {max}. Please wait...");

int[] fibonacciNumbers = numbers
    .Select(number => Fibonacci(number)).ToArray();

watch.Stop();
WriteLine("{0:#,##0} elapsed milliseconds.",
    arg0: watch.ElapsedMilliseconds);

Write("Results:");
foreach (int number in fibonacciNumbers)
{
    Write($" {number}");
}

static int Fibonacci(int term) =>
    term switch
    {
        1 => 0,
        2 => 1,
        _ => Fibonacci(term - 1) + Fibonacci(term - 2)
    };
```

## 使用 Windows

- 1. 如果您使用的是 Windows，则右键单击 Windows 开始按钮或按 Ctrl + Alt + Delete，然后单击任务管理器。
- 2. 在任务管理器窗口底部，单击“显示更多详细信息”。
- 3. 在任务管理器窗口顶部，单击“性能”选项卡。
- 4. 右键单击 CPU 利用率图表，选择“更改图表为”，然后选择“逻辑处理器”。

## 使用 macOS

- 1. 如果您使用的是 macOS，则启动“活动监视器”。
- 2. 导航至 视图 | 更新频率 非常频繁（1 秒）。
- 3. 要查看 CPU 图表，请导航至 窗口 | CPU 历史记录。

## 适用于所有操作系统

- 1. 重新排列您的监控工具和代码编辑器，使其并排显示。
- 2. 等待 CPU 稳定后，然后按 Enter 启动秒表并运行查询。结果应为经过的毫秒数，如以下输出所示：
- 3. 在 Program.cs 中，修改查询以调用 AsParallel 扩展方法并对结果序列进行排序，因为并行处理时结果可能会乱序，如以下代码高亮所示：

```
Press ENTER to start.
Calculating Fibonacci sequence up to 45. Please wait...
17,624 elapsed milliseconds.
Results: 0 1 1 2 3 5 8 13 21 34 55 89 144 233 377 610 987 1597 2584 4181
6765 10946 17711 28657 46368 75025 121393 196418 317811 514229 832040
1346269 2178309 3524578 5702887 9227465 14930352 24157817 39088169
63245986 102334155 165580141 267914296 433494437 701408733
```

监控工具很可能会显示一两个 CPU 使用最多，并且会随时间交替。其他任务也可能同时执行后台任务，例如垃圾回收器，因此其他 CPU 或核心不会完全平稳，但工作显然没有在所有可能的 CPU 或核心之间均匀分配。此外，请注意某些逻辑处理器已达到 100% 的峰值。

```
int[] fibonacciNumbers = numbers.AsParallel()
    .Select(number => Fibonacci(number))
    .OrderBy(number => number)
    .ToArray();
```

> **良好实践：** 切勿在查询末尾调用 AsParallel。这没有任何作用。您必须在调用 AsParallel 之后至少执行一个操作，以便该操作能够并行化。.NET 6 引入了一个代码分析器，它会警告这种类型的误用。

- 4. 运行代码，等待监控工具中的 CPU 图表稳定，然后按 *Enter* 启动秒表并运行查询。这次，应用程序应该在更短的时间内完成（尽管可能没有您希望的那么少——管理这些多个线程需要额外的工作！）：

```
Press ENTER to start.
Calculating Fibonacci sequence up to 45. Please wait...
9,028 elapsed milliseconds.
Results: 0 1 1 2 3 5 8 13 21 34 55 89 144 233 377 610 987 1597 2584 4181
6765 10946 17711 28657 46368 75025 121393 196418 317811 514229 832040
1346269 2178309 3524578 5702887 9227465 14930352 24157817 39088169
63245986 102334155 165580141 267914296 433494437 701408733
```

- 5. 监控工具应显示所有 CPU 都被平等地用于执行 LINQ 查询，并且请注意没有任何逻辑处理器达到 100% 的峰值，因为工作分配得更均匀。

您将在第 12 章 *使用多任务提高性能和可扩展性* 中了解更多关于管理多个线程的内容。

## 创建自己的 LINQ 扩展方法

在第 6 章 *实现接口和继承类* 中，您学习了如何创建自己的扩展方法。要创建 LINQ 扩展方法，您所需要做的就是扩展 IEnumerable<T> 类型。

> **良好实践：** 将您自己的扩展方法放在单独的类库中，以便可以轻松地作为自己的程序集或 NuGet 包部署。

我们将以 Average 扩展方法为例进行改进。一个受过良好教育的孩子会告诉你，*平均值* 可能有三种含义：

- **均值：** 将数字求和并除以计数。
- **众数：** 最常见的数字。
- **中位数：** 排序后位于数字中间的数字。

Microsoft 对 Average 扩展方法的实现计算的是均值。我们可能想要为 Mode 和 Median 定义自己的扩展方法：

- 1. 在 LinqWithEFCore 项目中，添加一个名为 MyLinqExtensions.cs 的新类文件。
- 2. 修改该类，如以下代码所示：

```
namespace System.Linq; // extend Microsoft's namespace

public static class MyLinqExtensions
{
    // this is a chainable LINQ extension method
    public static IEnumerable<T> ProcessSequence<T>(
        this IEnumerable<T> sequence)
    {
        // you could do some processing here
        return sequence;
    }

    public static IQueryable<T> ProcessSequence<T>(
        this IQueryable<T> sequence)
    {
        // you could do some processing here
        return sequence;
    }

    // these are scalar LINQ extension methods
    public static int? Median(
        this IEnumerable<int?> sequence)
    {
        var ordered = sequence.OrderBy(item => item);
        int middlePosition = ordered.Count() / 2;
        return ordered.ElementAt(middlePosition);
    }

    public static int? Median<T>(
        this IEnumerable<T> sequence, Func<T, int?> selector)
    {
        return sequence.Select(selector).Median();
    }

    public static decimal? Median(
        this IEnumerable<decimal?> sequence)
    {
        var ordered = sequence.OrderBy(item => item);
        int middlePosition = ordered.Count() / 2;
        return ordered.ElementAt(middlePosition);
    }
}
```
