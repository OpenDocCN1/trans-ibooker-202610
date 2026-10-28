

## 附加信息

1. 首先，执行 `Runnable` 对象（在本例中为 `FileSearch` 对象）。

2. 其次，返回此任务生成的结果。为此，你重写了 `Task` 类的 `set()` 方法。内部地，`FutureTask` 类控制其需要执行的任务何时完成。在那一刻，它会调用 `set()` 方法来建立任务的返回值。当你执行一个 `Callable` 对象时，此调用会使用 `call()` 方法返回的值进行调用，但当你执行一个 `Runnable` 对象时，此调用会使用 null 值进行调用。你已将此 null 值更改为 `FileSearch` 对象生成的结果列表。`set()` 方法仅在第一次被调用时才有效。当第一次调用时，它会将任务标记为已完成，后续的调用将不会修改任务的返回值。

在 `Main` 类中，而不是将 `Callable` 或 `Runnable` 对象发送给执行器，你可以发送 `FutureTask` 对象。主要区别在于你使用 `FutureTask` 对象来获取任务的结果，而不是 `submit()` 方法返回的 `Future` 对象。

在这种情况下，你仍然可以使用 `submit()` 方法返回的 `Future` 对象来控制任务的状态，但请记住，由于此任务执行的是 `Runnable` 对象（你已使用实现 `Runnable` 接口的 `FileSearch` 对象初始化了 `FutureTask` 对象），如果你在 `Future` 对象上调用 `get()` 方法，你将得到 null 值。

### 还有更多...

`FutureTask` 类提供了一个未包含在 `Future` 接口中的方法。它是 `setException()` 方法。此方法接收一个 `Throwable` 对象作为参数，并且当调用 `get()` 方法时，将抛出 `ExecutionException`。此调用仅在尚未调用 `FutureTask` 对象的 `set()` 方法时才有效。

### 另请参阅

- *Chapter 4, Thread Executors* 中 *Executing tasks in an executor that returns a result* 食谱
- *Chapter 1, Thread Management* 中 *Creating, running, and setting the characteristics of a thread* 食谱

## 在 ForkJoinPool 类中处理不受控制的异常

fork/join 框架使你能够为 `ForkJoinPool` 类的工作线程抛出的异常设置处理程序。当你使用 `ForkJoinPool` 类时，你应该理解任务和工作线程之间的区别。

要使用 fork/join 框架，你需要实现一个扩展 `ForkJoinTask` 类的任务，或者通常是 `RecursiveAction` 或 `RecursiveTask` 类。任务实现了你希望使用框架并发执行的动作。它们由 `ForkJoinPool` 类中的工作线程执行。一个工作线程将执行各种任务。在 `ForkJoinPool` 类实现的工作窃取算法中，当工作线程正在执行的任务完成执行或它正在等待另一个任务完成时，工作线程会寻找新任务。

在本食谱中，你将学习如何处理工作线程抛出的异常。为使其按以下各项所述工作，你必须实现两个额外的元素：

- 第一个元素是 `ForkJoinWorkerThread` 类的扩展类。此类实现了 `ForkJoinPool` 类的工作线程。你将实现一个基本子类，该子类会抛出异常。
- 第二个元素是用于创建你自己的自定义类型工作线程的工厂。`ForkJoinPool` 类使用工厂来创建其工作线程。你必须实现一个实现 `ForkJoinWorkerThreadFactory` 接口的类，并在 `ForkJoinPool` 类的构造函数中使用该类的对象。创建的 `ForkJoinPool` 对象将使用该工厂来创建工作线程。

### 如何做...

执行以下步骤来实现示例：

1. 首先，实现你自己的工作线程类。创建一个名为 `AlwaysThrowsExceptionWorkerThread` 的类，该类扩展 `ForkJoinWorkerThread` 类：

```java
public class AlwaysThrowsExceptionWorkerThread extends
    ForkJoinWorkerThread {
```

2. 实现该类的构造函数。它接收一个 `ForkJoinPool` 类作为参数并调用其父类的构造函数：

```java
protected AlwaysThrowsExceptionWorkerThread(ForkJoinPool pool) {
    super(pool);
}
```

3. 实现 `onStart()` 方法。这是 `ForkJoinWorkerThread` 类的一个方法，并在工作线程开始执行时执行。实现将在被调用时抛出 `RuntimeException`：

```java
protected void onStart() {
    super.onStart();
    throw new RuntimeException("Exception from worker thread");
}
```

4. 现在，实现创建工作线程所需的工厂。创建一个名为 `AlwaysThrowsExceptionWorkerThreadFactory` 的类，该类实现 `ForkJoinWorkerThreadFactory` 接口：

```java
public class AlwaysThrowsExceptionWorkerThreadFactory implements ForkJoinWorkerThreadFactory {
```

5. 实现 `newThread()` 方法。它接收一个 `ForkJoinPool` 对象作为参数并返回一个 `ForkJoinWorkerThread` 对象。创建一个 `AlwaysThrowsExceptionWorkerThread` 对象并返回它：

```java
@Override
public ForkJoinWorkerThread newThread(ForkJoinPool pool) {
    return new AlwaysThrowsExceptionWorkerThread(pool);
}
```

6. 实现一个将管理工作线程抛出的异常的类。实现一个名为 `Handler` 的类，该类实现 `UncaughtExceptionHandler` 接口：

```java
public class Handler implements UncaughtExceptionHandler {
```

7. 实现 `uncaughtException()` 方法。它接收一个 `Thread` 对象和一个 `Throwable` 对象作为参数，并在工作线程每次抛出异常时由 `ForkJoinPool` 类调用。向控制台写入消息并退出程序：

```java
@Override
public void uncaughtException(Thread t, Throwable e) {
    System.out.printf("Handler: Thread %s has thrown an Exception.\n", t.getName());
    System.out.printf("%s\n", e);
    System.exit(-1);
}
```

8. 现在，实现一个将在 `ForkJoinPool` 执行器中执行的任务。创建一个名为 `OneSecondLongTask` 的类，该类扩展 `RecursiveAction` 类：

```java
public class OneSecondLongTask extends RecursiveAction {
```

9. 实现 `compute()` 方法。它只是让线程在 1 秒后休眠：

```java
@Override
protected void compute() {
    System.out.printf("Task: Starting.\n");
    try {
        TimeUnit.SECONDS.sleep(1);
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
    System.out.printf("Task: Finish.\n");
}
```

10. 现在，实现示例的主类。创建一个名为 `Main` 的类，其中包含 `main()` 方法：

```java
public class Main {
    public static void main(String[] args) {
```

11. 创建一个新的 `OneSecondLongTask` 对象：

```java
OneSecondLongTask task = new OneSecondLongTask();
```

12. 创建一个新的 `Handler` 对象：

```java
Handler handler = new Handler();
```

13. 创建一个新的 `AlwaysThrowsExceptionWorkerThreadFactory` 对象：

```java
AlwaysThrowsExceptionWorkerThreadFactory factory = new AlwaysThrowsExceptionWorkerThreadFactory();
```

14. 创建一个新的 `ForkJoinPool` 对象。将值 2、工厂对象、处理程序对象和值 false 作为参数传递：

```java
ForkJoinPool pool = new ForkJoinPool(2, factory, handler, false);
```

15. 使用 `execute()` 方法在池中执行任务：

```java
pool.execute(task);
```

16. 使用 `shutdown()` 方法关闭池：

```java
pool.shutdown();
```

17. 使用 `awaitTermination()` 方法等待任务完成：

```java
try {
    pool.awaitTermination(1, TimeUnit.DAYS);
} catch (InterruptedException e) {
    e.printStackTrace();
}
```

18. 写入一条指示程序结束的消息：

```java
System.out.printf("Task: Finish.\n");
```

### 它是如何工作的...

在本食谱中，你实现了以下元素：

- **你自己的工作线程类**：你实现了 `AlwaysThrowsExceptionWorkerThread` 类，它扩展了 `ForkJoinWorkerThread` 类，后者实现了 fork/join 池的工作线程。你重写了 `onStart()` 方法。此方法在工作线程开始执行时执行。它只是在被调用时抛出 `RuntimeException`。

