

6. 实现 `thread3()` 方法。它将实现生产者的逻辑。首先等待两个消费者在 `take()` 方法中被阻塞；使用 `waitForTick()` 方法两次阻塞此方法。然后，调用队列的 `put()` 方法在队列中插入两个字符串：

```
public void thread3() {
    waitForTick(1);
    waitForTick(2);
    queue.put("Event 1");
    queue.put("Event 2");
    System.out.printf("Thread 3: Inserted two elements\n");
}
```

7. 最后，实现 `finish()` 方法。在控制台写入一条消息以指示测试已完成执行。使用 `assertEquals()` 方法检查两个事件已被消费（因此队列的大小为 0）：

```
public void finish() {
    super.finish();
    System.out.printf("Test: End\n");
    assertEquals(true, queue.size()==0);
    System.out.printf("Test: Result: The queue is empty\n");
}
```

8. 接下来，通过创建一个名为 `Main` 的类并包含 `main()` 方法来实现示例的主要类：

```
public class Main {
    public static void main(String[] args) throws Throwable {
```

9. 创建一个名为 `test` 的 `ProducerConsumerTest` 对象：

```
ProducerConsumerTest test=new ProducerConsumerTest();
```

10. 使用 `TestFramework` 类的 `runOnce()` 方法执行测试：

```
System.out.printf("Main: Starting the test\n");
TestFramework.runOnce(test);
System.out.printf("Main: The test has finished\n");
```

## 它是如何工作的...

在本教程中，你使用 `MultithreadedTC` 库为 `LinkedTransferQueue` 类实现了一个测试。你可以使用该库及其节拍器为任何并发应用程序或类实现测试。在示例中，你实现了经典的生产者/消费者问题，包含两个消费者和一个生产者。你想要测试的是，缓冲区中引入的第一个 `String` 对象被到达缓冲区的第一个消费者消费，而缓冲区中引入的第二个 `String` 对象被到达缓冲区的第二个消费者消费。

`MultithreadedTC` 库基于 JUnit 库，后者是用于在 Java 中实现单元测试的最常用的库。要使用 `MultithreadedTC` 库实现一个基本测试，你必须扩展 `MultithreadedTestCase` 类。此类扩展了包含检查测试结果的所有方法的 `junit.framework.AssertJUnit` 类。它不扩展 `junit.framework.TestCase` 类，因此你无法将 MultithreadedTC 测试与其他 JUnit 测试集成。

然后，你可以实现以下方法：

- `initialize()`：此方法的实现是可选的。它在你启动测试时执行，因此你可以使用它来初始化在测试中使用的对象。
- `finish()`：此方法的实现是可选的。它在测试完成后执行。你可以使用它来关闭或释放测试期间使用的资源，或检查测试结果。
- 实现测试的方法：这些方法包含你实现的测试的主要逻辑。它们必须以 `thread` 关键字开头，后跟一个字符串，例如 `thread1()`。

为了控制线程执行的顺序，你使用了 `waitForTick()` 方法。此方法接收一个整数值作为参数，并使执行该方法的线程休眠，直到在测试中运行的所有线程都被阻塞。当它们被阻塞时，`MultithreadedTC` 库会恢复由对 `waitForTick()` 方法的调用阻塞的线程。

你作为 `waitForTick()` 方法参数传递的整数用于控制执行顺序。`MultithreadedTC` 库的节拍器有一个内部计数器。当所有线程都被阻塞时，库会将此计数器递增到 `waitForTick()` 调用中指定的下一个数字。

内部地，当 MultithreadedTC 库必须执行测试时，首先它执行 `initialize()` 方法。然后它为以 `thread` 关键字开头的方法创建线程（在你的示例中，方法为 `thread1()`、`thread2()` 和 `thread3()`）。当所有线程完成执行后，它执行 `finish()` 方法。要执行测试，你使用了 `TestFramework` 类的 `runOnce()` 方法。

## 还有更多...

如果 MultithreadedTC 库检测到测试的所有线程都被阻塞，除了在 `waitForTick()` 方法中，测试将被声明为死锁状态并抛出 `java.lang.IllegalStateException` 异常。

## 另请参阅

- 本章中的 Analyzing concurrent code with FindBugs 教程

## 使用 JConsole 进行监控

JConsole 是一个遵循 JMX 规范的监控工具，允许你获取有关应用程序执行的 information，例如线程数、内存使用或类加载。它随 JDK 一起提供，可用于监控本地或远程应用程序。在本教程中，你将学习如何使用此工具监控一个简单的并发应用程序。

## 准备工作

本教程的示例使用 Eclipse IDE 实现。如果使用 Eclipse 或其他 IDE，例如 NetBeans，打开它并创建一个新的 Java 项目。

## 如何操作...

按照以下步骤实现示例：

- 1. 创建一个名为 `Task` 的类并指定 `Runnable` 接口。实现 `run()` 方法以在 100 秒内在控制台写入消息：
- 2. 实现包含 `main()` 方法的 `Main` 类。创建 10 个 `Task` 对象以创建 10 个线程。启动它们并使用 `join()` 方法等待它们的完成：

```
public class Task implements Runnable {

    @Override
    public void run() {

        Date start, end;
        start = new Date();
        do {
            System.out.printf("%s: tick\n",
                    Thread.currentThread().getName());
            end = new Date();
        } while (end.getTime() - start.getTime() < 100000);
    }
}
```

```
public class Main {
    public static void main(String[] args) {

        Thread[] threads = new Thread[10];

        for (int i=0; i<10; i++) {
            Task task=new Task();
            threads[i]=new Thread(task);
            threads[i].start();
        }

        for (int i=0; i<10; i++) {
            try {
                threads[i].join();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}
```

3. 打开一个控制台窗口并执行 JConsole 应用程序。它包含在 JDK-9 安装的 bin 目录中：

## 它是如何工作的...

在本教程中，我们实现了一个非常简单的示例：运行 10 个线程 100 秒。这些线程在控制台写入消息。

当你执行 JConsole 时，你会看到一个窗口，显示系统中运行的所有 Java 应用程序。你可以选择想要监控的应用程序。窗口将类似于以下内容：

![](img/cc06c59cd21f558f43e9475d7f212170_502_0.png)

在这种情况下，我们选择我们的示例应用并点击 **Connect** 按钮。然后，你将被要求与应用程序建立不安全连接，显示类似以下对话框：

![](img/cc06c59cd21f558f43e9475d7f212170_503_0.png)

点击 **Insecure connection** 按钮。JConsole 将使用六个标签向你显示有关应用程序的信息：

- **Overview** 标签提供内存使用情况、应用程序中运行的线程数、创建的对象数以及应用程序的 CPU 使用情况的概览。
- **Memory** 标签显示应用程序使用的内存量。它有一个组合框，你可以在其中选择要监控的内存类型（堆、非堆或池）。
- **Threads** 标签显示有关应用程序中线程数的信息以及每个线程的详细信息。
- **Classes** 标签显示有关应用程序中加载的对象数的信息。
- **VM Summary** 标签提供运行应用程序的 JVM 的摘要。
- **MBeans** 标签显示有关应用程序的管理 bean 的信息。

