

### 表 15-1.（续）

| 类型 | 在生活中的含义 |
|---|---|
| ThreadPriority | 此枚举表示线程的优先级级别（Highest、Normal 等）。 |
| ThreadStart | 此委托用于指定给定线程要调用的方法。与 ParameterizedThreadStart 委托不同，ThreadStart 的目标必须始终具有相同的原型。 |
| ThreadState | 此枚举指定线程可能采用的有效状态（Running、Aborted 等）。 |
| Timer | 此类型提供按指定间隔执行方法的机制。 |
| TimerCallback | 此委托类型与 Timer 类型一起使用。 |

## System.Threading.Thread 类

System.Threading 命名空间中所有类型中最原始的是 Thread。此类代表 AppDomain 内给定执行路径的面向对象包装器。此类型还定义了多个方法（静态和实例级别），允许您在当前 AppDomain 中创建新线程，以及挂起、停止和销毁线程。请考虑表 15-2 中关键静态成员的列表。

### 表 15-2. Thread 类的关键静态成员

| 静态成员 | 在生活中的含义 |
|---|---|
| ExecutionContext | 此只读属性返回与逻辑执行线程相关信息，包括安全、调用、同步、本地化和事务上下文。 |
| CurrentThread | 此只读属性返回对当前运行线程的引用。 |
| Sleep() | 此方法使当前线程暂停指定的时间。 |

Thread 类还支持多个实例级成员，其中一些显示在表 15-3 中。

### 表 15-3. Thread 类的选定实例级成员

| 实例级成员 | 在生活中的含义 |
|---|---|
| IsAlive | 返回一个布尔值，指示此线程是否已启动（尚未终止或中止）。 |
| IsBackground | 获取或设置一个值，指示此线程是否为“后台线程”（稍后详细介绍）。 |
| Name | 允许您为线程建立一个友好的文本名称。 |
| Priority | 获取或设置线程的优先级，该优先级可以从 ThreadPriority 枚举中分配值。 |
| ThreadState | 获取此线程的状态，该状态可以从 ThreadState 枚举中分配值。 |
| Abort() | 指示 .NET Core 运行时尽快终止线程。 |
| Interrupt() | 中断（例如唤醒）当前线程使其从合适的等待期中退出。 |
| Join() | 阻塞调用线程，直到指定的线程（在其上调用 Join() 的线程）退出。 |
| Resume() | 恢复先前已挂起的线程。 |
| Start() | 指示 .NET Core 运行时尽快执行线程。 |
| Suspend() | 挂起线程。如果线程已挂起，则调用 Suspend() 无效。 |

> 注意 中止或挂起活动线程通常被认为是不好的主意。当您这样做时，存在（尽管很小）线程可能在被干扰或终止时“泄漏”其工作量的风险。

## 获取有关当前执行线程的统计信息

请记住，可执行程序集的入口点（即顶级语句或 Main() 方法）在主执行线程上运行。为了说明 Thread 类型的基本用法，假设您有一个名为 ThreadStats 的新控制台应用程序项目。如您所知，静态 Thread.CurrentThread 属性检索表示当前执行线程的 Thread 对象。一旦您获得了当前线程，您就可以打印出各种统计信息，如下所示：

```csharp
// Be sure to import the System.Threading namespace.
using System;
using System.Threading;
Console.WriteLine("***** Primary Thread stats ****\n");

// Obtain and name the current thread.
Thread primaryThread = Thread.CurrentThread;
primaryThread.Name = "ThePrimaryThread";

// Print out some stats about this thread.
Console.WriteLine("ID of current thread: {0}",
    primaryThread.ManagedThreadId);
Console.WriteLine("Thread Name: {0}",
    primaryThread.Name);
Console.WriteLine("Has thread started?: {0}",
    primaryThread.IsAlive);
Console.WriteLine("Priority Level: {0}",
    primaryThread.Priority);
Console.WriteLine("Thread State: {0}",
    primaryThread.ThreadState);
Console.ReadLine();
```

以下是当前输出：

```text
***** Primary Thread stats *****
ID of current thread: 1
Thread Name: ThePrimaryThread
Has thread started?: True
Priority Level: Normal
Thread State: Running
```

### Name 属性

请注意，Thread 类支持一个名为 Name 的属性。如果您不设置此值，Name 将返回一个空字符串。但是，一旦您为给定的 Thread 对象分配一个友好的字符串名称，您就可以大大简化调试工作。如果您使用 Visual Studio，您可以在调试会话期间访问线程窗口（程序运行时选择 Debug ► Windows ► Threads）。如您从图 15-1 所见，您可以快速识别要诊断的线程。

![](img/587021f4d5ba039be70a11107130377b_596_0.png)

图 15-1. 使用 Visual Studio 调试线程

### Priority 属性

接下来，请注意 Thread 类型定义了一个名为 Priority 的属性。默认情况下，所有线程的优先级级别均为 Normal。但是，您可以使用 Priority 属性和相关的 System.Threading.ThreadPriority 枚举在线程生命周期的任何时间更改它，如下所示：

```csharp
public enum ThreadPriority
{
    Lowest,
    BelowNormal,
    Normal, // Default value.
    AboveNormal,
    Highest
}
```

如果您将线程的优先级级别分配为默认值（ThreadPriority.Normal）以外的其他值，请理解您无法直接控制线程调度程序在线程之间切换的时间。线程的优先级级别向 .NET Core 运行时提供有关线程活动重要性的提示。因此，具有 ThreadPriority.Highest 值的线程不一定保证获得最高优先级。

同样，如果线程调度程序忙于特定任务（例如同步对象、切换线程或移动线程），优先级级别很可能会相应更改。但是，在所有条件相同的情况下，.NET Core 运行时将读取这些值并指示线程调度程序如何最好地分配时间片。具有相同线程优先级的线程应各自获得相同的时间来执行其工作。

在大多数情况下，您很少（如果有的话）需要直接更改线程的优先级级别。理论上，可以提高一组线程的优先级级别，从而阻止低优先级线程在其所需级别执行（因此请谨慎使用）。

## 手动创建辅助线程

当您想要以编程方式创建额外的线程来执行某些工作单元时，使用 System.Threading 命名空间的类型时请遵循以下可预测的过程：

1. 创建一个方法作为新线程的入口点。
2. 创建一个新的 ParameterizedThreadStart（或 ThreadStart）委托，将步骤 1 中定义的方法的地址传递给构造函数。
3. 创建一个 Thread 对象，将 ParameterizedThreadStart/ThreadStart 委托作为构造函数参数传递。
4. 建立任何初始线程特征（名称、优先级等）。
5. 调用 Thread.Start() 方法。这会在可能的情况下尽快在步骤 2 中创建的委托引用的方法处启动线程。

如步骤 2 所述，您可以使用两种不同的委托类型来“指向”辅助线程将执行的 方法。ThreadStart 委托可以指向任何不带参数且不返回任何内容的方法。当方法设计为在后台简单运行而无需进一步交互时，此委托可能很有帮助。

ThreadStart 的限制是您无法传入参数进行处理。但是，ParameterizedThreadStart 委托类型允许单个 System.Object 类型的参数。鉴于任何东西都可以表示为 System.Object，您可以通过自定义类或结构传入任意数量的参数。请注意，ThreadStart 和 ParameterizedThreadStart 委托只能指向返回 void 的方法。

### 使用 ThreadStart 委托工作

为了说明构建多线程应用程序的过程（以及展示这样做的有用性），假设您有一个名为 SimpleMultiThreadApp 的控制台应用程序项目，允许最终用户选择应用程序是否将使用单个主线程执行其职责，或者是否将使用两个单独的执行线程拆分其工作负载。

假设您已导入 System.Threading 命名空间，您首先要定义一个方法来执行（可能的）辅助线程的工作。为了专注于构建多线程程序的机制，此方法将简单地将一系列数字打印到控制台窗口，每次循环暂停大约两秒钟。以下是 Printer 类的完整定义：

```csharp
using System;
using System.Threading;

namespace SimpleMultiThreadApp
{
    public class Printer
    {
        public void PrintNumbers()
```

