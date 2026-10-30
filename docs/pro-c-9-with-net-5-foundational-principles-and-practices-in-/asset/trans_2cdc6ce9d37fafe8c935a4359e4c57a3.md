
使用 async 修饰符时，.NET Core 运行时将会创建一个新的执行线程来处理手头的任务。此外，当您调用异步方法时，await 关键字会自动暂停当前线程的任何进一步活动，直到任务完成，从而使调用线程可以继续执行。

为了说明这一点，创建一个名为 FunWithCSharpAsync 的控制台应用程序，并将 System.Threading、System.Threading.Tasks 和 System.Collections.Generic 命名空间导入 Program.cs。添加一个名为 DoWork() 的方法，它强制调用线程等待五秒钟。到目前为止的情况如下：

```
using System;
using System.Collections.Generic;
using System.Threading;
using System.Threading.Tasks;

Console.WriteLine(" Fun With Async ==>");
Console.WriteLine(DoWork());
Console.WriteLine("Completed");
Console.ReadLine();

static string DoWork()
{
    Thread.Sleep(5_000);
    return "Done with work!";
}
```

现在，根据你在本章中学习的内容，你知道如果运行该程序，你需要等待五秒才能进行其他任何操作。如果这是一个图形应用程序，整个屏幕将一直锁定，直到工作完成。

如果你使用本章前面展示的任何技术来让程序响应更快，那么你还有大量工作要做。然而，自 .NET 4.5 起，你可以编写以下 C# 代码：

```
...
string message = await DoWorkAsync();
Console.WriteLine(message);
...

static string DoWork()
{
    Thread.Sleep(5_000);
    return "Done with work!";
}
static async Task<string> DoWorkAsync()
{
    return await Task.Run(() =>
    {
        Thread.Sleep(5_000);
        return "Done with work!";
    });
}
```

如果你使用 Main() 方法作为入口点（而不是顶级语句），则需要将该方法标记为 async，这是 C# 7.1 中引入的。

```
static async Task Main(string[] args)
{
...
string message = await DoWorkAsync();
Console.WriteLine(message);
...
}
```

> 注意 从 C# 7.1 开始，可以使用 async 装饰 Main() 方法。在 C# 9.0 中，顶级语句隐式地支持 async。

注意在调用异步方法时，await 关键字位于方法名前。这一点很重要：如果你用 async 关键字修饰了一个方法，但其中至少没有一处以 await 为核心的方法调用，那么你本质上构建的是一个同步方法调用（实际上，编译器会给出一个关于此效果的警告）。

现在，注意你需要使用 System.Threading.Tasks 命名空间中的 Task 类来重构你的 Main()（如果你使用 Main() 的话）和 DoWork() 方法（后者被添加为 DoWorkAsync()）。基本上，你不是直接返回一个具体的返回值（在当前示例中是一个字符串对象），而是返回一个 Task<T> 对象，其中泛型类型参数 T 是底层的实际返回值（到现在为止还跟得上吗？）。如果方法没有返回值（如 Main() 方法），那么你只使用 Task 而不是 Task<T>。

DoWorkAsync() 的实现现在直接返回一个 Task<T> 对象，即 Task.Run() 的返回值。Run() 方法接受一个 Func<> 或 Action<> 委托，而你在本书的这一点上应该知道，可以使用 lambda 表达式来简化生活。基本上，你的新版本 DoWorkAsync() 本质上是在表达以下内容：

> 当你调用我时，我会运行一个新任务。这个任务会让调用线程休眠五秒，当它完成时，会给我一个字符串返回值。我会把这个字符串放入一个新的 Task<string> 对象中，并返回给调用者。

将这个新版本的 DoWorkAsync() 实现转换成更自然（富有诗意）的语言后，你会对 await 标记的真正作用有所了解。这个关键字总是修饰返回 Task 对象的方法。当逻辑流到达 await 标记时，调用线程会在此方法中挂起，直到调用完成。如果你运行这个版本的应用程序，你会发现“Completed”消息先于“Done with work!”消息出现。如果这是一个图形应用程序，用户可以在 DoWorkAsync() 方法执行期间继续使用 UI。

## SynchronizationContext 与 async/await

SynchronizationContext 的官方定义是一个提供无同步自由线程上下文的基类。虽然这个最初的定义不是很有描述性，但官方文档接着说明：

> 该类实现的同步模型的目的是允许公共语言运行时的内部异步/同步操作在不同同步模型下正确运行。

这句话，加上你对多线程的了解，阐明了这个问题。回想一下，GUI 应用程序（WinForms、WPF）不允许辅助线程直接访问控件，而必须委托这种访问。我们已经在 WPF 示例中看到了 Dispatcher 对象。对于不使用 WPF 的控制台应用程序，这种限制并不存在。这些就是所提到的不同同步模型。考虑到这一点，让我们更深入地探究 SynchronizationContext。

SynchronizationContext 是一种提供虚方法 Post 的类型，该方法接受一个要异步执行的委托。这为框架提供了一种模式，使其能够适当地处理异步请求（对于 WPF/WinForms 进行分派，对于非 GUI 应用程序直接执行等）。它提供了一种将工作单元排队到上下文中的方式，并跟踪未完成的异步操作的数量。

正如我们之前讨论的，当委托被排队异步运行时，它会被调度到单独的线程上运行。这个细节由 .NET Core 运行时处理。这通常使用 .NET Core 运行时的托管线程池来完成，但也可以通过自定义实现来覆盖。

虽然这些管道工作可以通过代码手动管理，但 async/await 模式完成了大部分繁重的工作。当异步方法被等待时，它会利用目标框架的 SynchronizationContext 和 TaskScheduler 实现。例如，如果你在 WPF 应用程序中使用 async/await，WPF 框架会管理委托的分派，并在等待的任务完成时回调到状态机中，以便安全地更新控件。

## ConfigureAwait 的作用

既然你对 SynchronizationContext 有了更好的理解，是时候介绍 ConfigureAwait() 方法的作用了。默认情况下，等待一个 Task 会利用同步上下文。在开发 GUI 应用程序（WinForms、WPF）时，这正是你想要的行为。然而，如果你编写的是非 GUI 应用程序代码，在不需要时排队原始上下文的开销可能会导致应用程序出现性能问题。

为了实际看到这一点，请将你的顶级语句更新为以下内容：

```
Console.WriteLine(" Fun With Async ==>");
//Console.WriteLine(DoWork());
string message = await DoWorkAsync();
Console.WriteLine(message);
```

```
string message1 = await DoWorkAsync().ConfigureAwait(false);
Console.WriteLine(message1);
```

原始代码块使用框架（在本例中为 .NET Core 运行时）提供的 SynchronizationContext。这等效于调用 ConfigureAwait(true)。第二个示例忽略了当前上下文和调度器。

.NET Core 团队的指导建议是，当开发应用程序代码（WinForms、WPF 等）时，请保持默认行为不变。如果你编写的是非应用程序代码（例如库代码），则使用 ConfigureAwait(false)。唯一的例外是 ASP.NET Core（在第 9 部分中介绍）。ASP.NET Core 不会创建自定义 SynchronizationContext；因此，ConfigureAwait(false) 并不会带来它在其他框架中所提供的好处。

## 异步方法的命名约定

当然，你注意到了名称从 DoWork() 变为 DoWorkAsync()，但为什么要这样改呢？假设新版本的方法仍然命名为 DoWork()；然而，调用代码却这样实现：

```
//Oops! No await keyword here!
string message = DoWork();
```

