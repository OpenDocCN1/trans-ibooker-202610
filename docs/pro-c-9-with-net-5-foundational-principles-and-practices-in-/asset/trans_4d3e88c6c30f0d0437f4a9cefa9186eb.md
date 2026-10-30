

如果要运行此项目，执行所有任务可能需要花费大量时间，这取决于机器的 CPU 数量和整体处理器速度。最终，你应该会看到此处显示的输出：

```
Downloading book...
Download complete.
Ten Most Common Words are:
Defarge
himself
Manette
through
nothing
business
another
looking
prisoner
Cruncher
Longest word is: undistinguishable
```

你可以通过并行调用 FindTenMostCommon() 和 FindLongestWord() 方法来确保你的应用程序使用主机上所有可用的 CPU。为此，按如下方式修改你的 GetStats() 方法：

```
void GetStats()
{
    // Get the words from the ebook.
    string[] words = _theEBook.Split(
        new char[] { ' ', '\u000A', ',', '.', ';', ':', '-', '?', '/' },
        StringSplitOptions.RemoveEmptyEntries);
    string[] tenMostCommon = null;
    string longestWord = string.Empty;

    Parallel.Invoke(
        () =>
        {
            // Now, find the ten most common words.
            tenMostCommon = FindTenMostCommon(words);
        },
        () =>
        {
            // Get the longest word.
            longestWord = FindLongestWord(words);
        });

    // Now that all tasks are complete, build a string to show all stats.
    ...
}
```

Parallel.Invoke() 方法期望一个 Action<> 委托的参数数组，你已通过 lambda 表达式间接提供了它们。同样，虽然输出是相同的，但好处是 TPL 现在将使用机器上所有可能的处理器在可能的情况下并行调用每个方法。

## Parallel LINQ Queries (PLINQ)

为了结束你对 TPL 的了解，请注意，你还可以将并行任务合并到你的 .NET Core 应用程序中的另一种方式。如果你愿意，你可以使用一组扩展方法来构建一个 LINQ 查询，该查询将以并行方式执行其工作负载（如果可能）。恰当地说，设计为并行运行的 LINQ 查询被称为 PLINQ 查询。

与使用 Parallel 类编写的并行代码一样，PLINQ 也有在必要时忽略你并行处理集合的请求的选择。PLINQ 框架已在许多方面进行了优化，其中包括确定查询是否实际上会在同步方式下运行得更快。

在运行时，PLINQ 分析查询的整体结构，如果查询可能从并行化中受益，它将并发运行。但是，如果并行化查询会损害性能，PLINQ 只是按顺序运行查询。如果 PLINQ 在可能昂贵的并行算法和廉价的顺序算法之间做出选择，它默认选择顺序算法。

必要的扩展方法位于 System.Linq 命名空间的 ParallelEnumerable 类中。表 15-5 记录了一些有用的 PLINQ 扩展。

### Table 15-5. Select Members of the ParallelEnumerable Class

| Member | Meaning in Life |
|---|---|
| AsParallel() | Specifies that the rest of the query should be parallelized, if possible |
| WithCancellation() | Specifies that PLINQ should periodically monitor the state of the provided cancellation token and cancel execution if it is requested |
| WithDegreeOfParallelism() | Specifies the maximum number of processors that PLINQ should use to parallelize the query |
| ForAll() | Enables results to be processed in parallel without first merging back to the consumer thread, as would be the case when enumerating a LINQ result using the foreach keyword |

要查看 PLINQ 的实际效果，请创建一个名为 PLINQDataProcessingWithCancellation 的控制台应用程序并导入 System.Linq、System.Threading 和 System.Threading.Tasks 命名空间（如果尚未导入）。处理开始时，程序将启动一个新的 Task，该 Task 执行一个 LINQ 查询，调查一个大型整数数组，查找仅满足 x % 3 == 0 为 true 的项。以下是该查询的非并行版本：

```
using System;
using System.Linq;
using System.Threading;
using System.Threading.Tasks;

Console.WriteLine("Start any key to start processing");
Console.ReadKey();

Console.WriteLine("Processing");
Task.Factory.StartNew(ProcessIntData);
Console.ReadLine();

void ProcessIntData()
{
    // Get a very large array of integers.
    int[] source = Enumerable.Range(1, 10_000_000).ToArray();

    // Find the numbers where num % 3 == 0 is true, returned
    // in descending order.
    int[] modThreeIsZero = (
        from num in source
        where num % 3 == 0
        orderby num descending
        select num).ToArray();
    Console.WriteLine($"Found { modThreeIsZero.Count()} numbers that match query!");
}
```

### Opting in to a PLINQ Query

如果你想通知 TPL 以并行方式（如果可能）执行此查询，你将希望使用 AsParallel() 扩展方法，如下所示：

```
int[] modThreeIsZero = (
    from num in source.AsParallel()
    where num % 3 == 0
    orderby num descending select num).ToArray();
```

请注意 LINQ 查询的整体格式与你在前几章中看到的相同。然而，通过包含对 AsParallel() 的调用，TPL 将尝试将工作负载传递给任何可用的 CPU。

### Cancelling a PLINQ Query

也可以使用 CancellationTokenSource 对象在正确条件下（通常是由于用户干预）通知 PLINQ 查询停止处理。声明一个名为 _cancelToken 的类级 CancellationTokenSource 对象并更新顶级语句方法以从用户获取输入。以下是相关的代码更新：

```
CancellationTokenSource _cancelToken =
    new CancellationTokenSource();

do
{
    Console.WriteLine("Start any key to start processing");
    Console.ReadKey();
    Console.WriteLine("Processing");
    Task.Factory.StartNew(ProcessIntData);
    Console.Write("Enter Q to quit: ");
    string answer = Console.ReadLine();

    // Does user want to quit?
    if (answer.Equals("Q",
        StringComparison.OrdinalIgnoreCase))
    {
        _cancelToken.Cancel();
        break;
    }
}
while (true);

Console.ReadLine();
```

现在，通过链接 WithCancellation() 扩展方法并传入令牌，通知 PLINQ 查询它应该留意传入的取消请求。此外，你将希望将此 PLINQ 查询包装在适当的 try/catch 范围内并处理可能的异常。以下是 ProcessIntData() 方法的最终版本：

```
void ProcessIntData()
{
    // Get a very large array of integers.
    int[] source = Enumerable.Range(1, 10_000_000).ToArray();

    // Find the numbers where num % 3 == 0 is true, returned
    // in descending order.
    int[] modThreeIsZero = null;
    try
    {
        modThreeIsZero = (from num in source.AsParallel().WithCancellation(_cancelToken.Token)
                          where num % 3 == 0
                          orderby num descending
                          select num).ToArray();
        Console.WriteLine();
        Console.WriteLine($"Found {modThreeIsZero.Count()} numbers that match query!");
    }
    catch (OperationCanceledException ex)
    {
        Console.WriteLine(ex.Message);
    }
}
```

运行此程序时，你需要快速按 Q 并输入以查看来自取消令牌的消息。在我的开发机器上，我大约有一秒钟的时间在它自行完成之前退出。

## Async Calls with the async/await

我已在本章（相当冗长）中涵盖了大量材料。毫无疑问，在任何框架中构建、调试和理解复杂的 multithreaded 应用程序都是具有挑战性的。虽然 TPL、PLINQ 和委托类型可以在一定程度上简化问题（尤其是与其它平台和语言相比），但开发人员仍需要了解各种高级技术的内部细节。

自 .NET 4.5 发布以来，C# 编程语言已更新了两个新关键字，进一步简化了编写异步代码的过程。与本章中的所有示例相反，当你使用新的 async 和 await 关键字时，编译器将代表你生成大量线程代码，使用 System.Threading 和 System.Threading.Tasks 命名空间的众多成员。

### A First Look at the C# async and await Keywords (Updated 7.1, 9.0)

C# 的 async 关键字用于限定方法、lambda 表达式或匿名方法应以异步方式自动调用。是的，这是真的。只需标记一个方法

