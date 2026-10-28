

14. 实现类的构造函数以初始化两个属性：

```
public ReportProcessor (CompletionService<String> service){
    this.service=service;
    end=false;
}
```

15. 实现 run() 方法。在 end 属性为 false 时，调用 CompletionService 接口的 poll() 方法来获取下一个由完成服务执行的已完成任务的 Future 对象：

```
@Override
public void run() {
    while (!end){
        try {
            Future<String> result=service.poll(20, TimeUnit.SECONDS);
```

16. 然后，使用 Future 对象的 get() 方法获取任务的结果并将结果写入控制台：

```
if (result!=null) {
    String report=result.get();
    System.out.printf("ReportReceiver: Report Received: %s\n", report);
}
} catch (InterruptedException | ExecutionException e) {
    e.printStackTrace();
}
}
System.out.printf("ReportSender: End\n");
}
```

17. 实现 stopProcessing() 方法以修改 end 属性的值：

```
public void stopProcessing() {
    this.end = true;
}
```

18. 通过创建一个名为 Main 的类并向其添加 main() 方法来实现示例的主类：

```
public class Main {
    public static void main(String[] args) {
```

25. 使用 `shutdown()` 方法完成 executor，并使用 `awaitTermination()` 方法等待任务的最终化：

```
System.out.printf("Main: Shutting down the executor.\n");
executor.shutdown();
try {
    executor.awaitTermination(1, TimeUnit.DAYS);
} catch (InterruptedException e) {
    e.printStackTrace();
}
```

26. 通过将 `ReportSender` 对象的 `end` 属性值设置为 true 来完成其执行：

```
processor.stopProcessing();
System.out.println("Main: Ends");
```

## 如何工作...

在示例的主类中，你使用 `Executors` 类的 `newCachedThreadPool()` 方法创建了 `ThreadPoolExecutor`。然后，你使用该 `Executor` 对象初始化一个 `CompletionService` 对象，因为完成服务使用 executor 来执行其任务。要使用完成服务执行任务，请使用 `submit()` 方法，如 `ReportRequest` 类中所示。

当其中一个任务在完成服务完成其执行时被执行时，服务会将用于控制其执行的 `Future` 对象存储在队列中。`poll()` 方法访问此队列以检查是否有任何任务已完成其执行；如果有，它返回队列的第一个元素，即已完成执行的任务的 `Future` 对象。当 `poll()` 方法返回一个 `Future` 对象时，它会将其从队列中删除。在这种情况下，你向该方法传递了两个属性，以指示你希望等待任务最终化的时间，以防包含已完成任务结果的队列为空。

一旦创建了 `CompletionService` 对象，你就创建了两个 `ReportRequest` 对象，它们执行 `ReportGenerator` 任务，使用之前创建并作为 `ReportRequest` 对象构造函数的参数传递的 `CompletionService` 对象执行 `ReportGenerator` 任务

## 还有更多...

`CompletionService` 类可以执行 `Callable` 或 `Runnable` 任务。在本示例中，你使用了 `Callable`，但你也可以发送 `Runnable` 对象。由于 `Runnable` 对象不产生结果，因此在这种情况下 `CompletionService` 类的理念不适用。

该类还提供了另外两种方法来获取已完成任务的 `Future` 对象。这些方法如下：

- `poll()`：不带参数的 `poll()` 方法版本检查队列中是否有任何 `Future` 对象。如果队列为空，它会立即返回 null。否则，它返回其第一个元素并将其从队列中移除。
- `take()`：此方法不带参数，检查队列中是否有任何 `Future` 对象。如果为空，它会阻塞线程直到队列有元素。如果队列有元素，它会返回并删除其第一个元素。

在我们的例子中，我们使用了带超时的 `poll()` 方法来控制我们希望结束 `ReportProcessor` 任务执行的时机。

## 另请参阅

- 本章中 *Executing tasks in an executor that returns a result* 配方

## 5
Fork/Join Framework

在本章中，我们将介绍：

- 创建 fork/join 池
- 加入任务的结果
- 异步运行任务
- 在任务中抛出异常
- 取消任务

### 引言

通常，当你实现一个简单的并发 Java 应用程序时，你会实现一些 `Runnable` 对象，然后是相应的 `Thread` 对象。你在程序中控制这些线程的创建、执行和状态。Java 5 引入了改进，使用了 `Executor` 和 `ExecutorService` 接口以及实现它们的类（例如 `ThreadPoolExecutor` 类）。

**Executor** 框架将任务创建与其执行分离。使用它时，你只需要实现 `Runnable` 对象并使用一个 `Executor` 对象。你将 `Runnable` 任务发送给 executor，它会创建、管理和完成执行这些任务所需的线程。

Java 7 更进一步，包含了一个面向特定类型问题的 `ExecutorService` 接口的额外实现。它是 **fork/join framework**。

该框架旨在解决可使用分而治之技术分解为更小任务的问题。在一个任务内部，你检查要解决的问题的大小，如果它大于既定大小，你就将其分解为使用框架执行的更小任务。如果问题的大小小于既定大小，你就在任务中直接解决问题，然后，可选地返回结果。以下图表总结了这一概念：

![](img/cc06c59cd21f558f43e9475d7f212170_222_0.png)

没有公式可以确定问题的参考大小，以确定任务是否应被细分，这取决于其特征。你可以使用任务中要处理的元素的数量和执行时间的估计来确定参考大小。测试不同的参考大小以选择最适合你的问题的大小。你可以将 `ForkJoinPool` 视为一种特殊的 `Executor`。

该框架基于以下两个操作：

- **Fork 操作：** 当你将任务分解为更小的任务并使用框架执行它们时。
- **Join 操作：** 当一个任务等待其创建的任务完成时。它用于合并这些任务的结果。

fork/join 与 Executor 框架之间的主要区别是 **work-stealing** 算法。与 Executor 框架不同，当任务使用 join 操作等待其创建的子任务完成时，执行该任务的线程（称为 **worker thread**）会寻找尚未执行的其他任务并开始执行它们。这样，线程就能充分利用其运行时间，从而提高应用程序的性能。

为了实现这一目标，由 fork/join 框架执行的任务具有以下限制：

- 任务只能使用 `fork()` 和 `join()` 操作作为同步机制。如果它们使用其他同步机制，worker 线程在同步操作中时就不能执行其他任务。例如，如果你在 fork/join 框架中使任务休眠，执行该任务的 worker 线程在休眠期间不会执行另一个任务。
- 任务不应执行 I/O 操作，例如在文件中读写数据。
- 任务不能抛出受检异常。它们必须包含处理这些异常所需的代码。

fork/join 框架的核心由以下两个类组成：

- `ForkJoinPool`：此类实现了 `ExecutorService` 接口和 work-stealing 算法。它管理 worker 线程并提供有关任务状态及其执行的信息。
- `ForkJoinTask`：这是将在 `ForkJoinPool` 中执行的任务的基类。它提供了在任务内部执行 `fork()` 和 `join()` 操作的机制以及控制任务状态的方法。通常，为了实现你的 fork/join 任务，你将实现此类三个子类之一：`RecursiveAction` 用于没有返回结果的任务，`RecursiveTask` 用于返回一个结果的任务，以及 `CountedCompleter` 用于在所有子任务完成后启动完成操作的任务。

该框架的大多数功能已包含在 Java 7 中，但 Java 8 在其中加入了次要功能。它包含了一个默认的 `ForkJoinPool` 对象。你可以使用 `ForkJoinPool` 类的静态方法 `commonPool()` 获取它。此默认 fork/join executor 默认将使用由计算机可用处理器决定的线程数。你可以通过更改系统属性 `java.util.concurrent.ForkJoinPool.common.parallelism` 的值来更改此默认行为。此默认池在并发 API 的其他类内部使用。例如，**Parallel Streams** 使用它。Java 8 还包含了前面提到的 `CountedCompleter` 类。

本章介绍了五个配方，展示了如何有效地使用 fork/join 框架。

