

## 分组线程并在线程组中处理未受控异常

Java 并发 API 提供的一个有趣功能是能够对线程进行分组。这使我们能够将线程组中的线程视为一个整体，并访问属于该组的线程对象以对其进行操作。例如，你有一些执行相同任务的线程，并且想要控制它们。你可以，例如，通过一次调用中断整个线程组中的所有线程。

Java 提供了 ThreadGroup 类来处理一组线程。ThreadGroup 对象可以由线程对象和其他 ThreadGroup 对象组成，形成线程的树形结构。

在《控制线程中断》配方中，你学习了如何使用通用方法处理线程对象中抛出的所有未捕获异常。在《处理线程中的未受控异常》配方中，我们编写了一个处理器来处理线程抛出的未捕获异常。我们可以使用类似的机制来处理线程或线程组抛出的未捕获异常。

在本配方中，我们将学习如何使用 ThreadGroup 对象以及如何实现并设置处理器来处理线程组中的未捕获异常。我们将通过一个示例来完成此操作。

### 准备工作

本配方的示例已使用 Eclipse IDE 实现。如果你使用 Eclipse 或其他 IDE，例如 NetBeans，请打开它并创建一个新的 Java 项目。

### 如何操作...

按照以下步骤实现示例：

- 1. 首先，通过创建一个名为 MyThreadGroup 的类来扩展 ThreadGroup 类，该类将继承自 ThreadGroup。你必须声明一个带有一个参数的构造函数，因为 ThreadGroup 类没有无参构造函数。扩展 ThreadGroup 类以重写 uncaughtException() 方法，以便处理组中线程抛出的异常：

```
public class MyThreadGroup extends ThreadGroup {
    public MyThreadGroup(String name) {
        super(name);
    }
}
```

- 2. 重写 uncaughtException() 方法。当 ThreadGroup 类中的一个线程抛出异常时，会调用此方法。在这种情况下，该方法将写入有关异常和抛出它的线程的信息；它将在控制台中呈现此信息。此外，请注意，此方法将中断 ThreadGroup 类中的其余线程：

```
@Override
public void uncaughtException(Thread t, Throwable e) {
    System.out.printf("The thread %s has thrown an Exception\n", t.getId());
    e.printStackTrace(System.out);
    System.out.printf("Terminating the rest of the Threads\n");
    interrupt();
}
```

- 3. 创建一个名为 Task 的类并指定它实现 Runnable 接口：

```
public class Task implements Runnable {
```

- 4. 实现 run() 方法。在这种情况下，我们将引发 ArithmeticException 异常。为此，我们将用随机数除 1,000，直到随机生成器生成零以抛出异常：

```
@Override
public void run() {
    int result;
    Random random=new Random(Thread.currentThread().getId());
    while (true) {
        result=1000/((int)(random.nextDouble()*1000000000));
        if (Thread.currentThread().isInterrupted()) {
            System.out.printf("%d : Interrupted\n", Thread.currentThread().getId());
            return;
        }
    }
}
```

- 5. 现在，通过创建一个名为 Main 的类并实现 main() 方法来实现示例的主类：

```
public class Main {
    public static void main(String[] args) {
```

- 6. 首先，计算你要启动的线程数。我们使用 Runtime 类的 availableProcessors() 方法（我们通过该类的静态方法 getRuntime() 获取与当前 Java 应用程序关联的运行时对象）。此方法返回 JVM 可用的处理器数量，通常等于运行应用程序的计算机的核心数：

```
int numberOfThreads = 2 * Runtime.getRuntime()
    .availableProcessors();
```

- 7. 创建 MyThreadGroup 类的对象：

```
MyThreadGroup threadGroup=new MyThreadGroup("MyThreadGroup");
```

- 8. 创建 Task 类的对象：

```
Task task=new Task();
```

- 9. 使用此 Task 类创建计算数量的 Thread 对象并启动它们：

```
for (int i = 0; i < numberOfThreads; i++) {
    Thread t = new Thread(threadGroup, task);
    t.start();
}
```

- 10. 然后，在控制台中写入有关 ThreadGroup 的信息：

```
System.out.printf("Number of Threads: %d\n", threadGroup.activeCount());
System.out.printf("Information about the Thread Group\n");
threadGroup.list();
```

- 11. 最后，写入构成该组的线程的状态：

```
Thread[] threads = new Thread[threadGroup.activeCount()];
threadGroup.enumerate(threads);
for (int i = 0; i < threadGroup.activeCount(); i++) {
    System.out.printf("Thread %s: %s\n", threads[i].getName(),
        threads[i].getState());
}
}
```

- 12. 运行示例并查看结果。

### 工作原理...

在下面的屏幕截图中，你可以看到 `ThreadGroup` 类的 `list()` 方法的输出以及我们在写入每个 Thread 对象的状态时生成的输出：

![](img/cc06c59cd21f558f43e9475d7f212170_67_0.png)

`ThreadGroup` 类存储与其关联的线程对象和其他 `ThreadGroup` 对象，以便可以访问它们的所有信息（例如状态）并对所有成员执行操作（例如中断）。

查看其中一个线程对象抛出了中断其他对象的异常的方式：

```
The thread 18 has thrown an Exception
java.lang.ArithmeticException: / by zero
    at com.packtpub.java9.concurrency.cookbook.chapter01.recipe09.task.Task.run(Task.java:18)
    at java.lang.Thread.run(java.base@9-ea/Thread.java:843)
Terminating the rest of the Threads
19 : Interrupted
14 : Interrupted
15 : Interrupted
13 : Interrupted
20 : Interrupted
17 : Interrupted
16 : Interrupted
```

当在 Thread 对象中抛出未捕获异常时，JVM 会查找此异常的三个可能的处理器。

首先，它会查找线程的未捕获异常处理器，如《处理线程中的未受控异常》配方中所述。如果此处理器不存在，则 JVM 会查找线程的 ThreadGroup 类的未捕获异常处理器，如本配方中所学。如果此方法不存在，JVM 会查找默认的未捕获异常处理器，如《处理线程中的未受控异常》配方中所述。

如果没有任何处理器存在，JVM 会在控制台中写入异常的堆栈跟踪并结束抛出异常的线程的执行。

### 另请参阅

- 《处理线程中的未受控异常》配方

## 通过工厂创建线程

工厂模式是面向对象编程世界中最常用的设计模式之一。它是一种创建型模式，其目标是开发一个对象的使命应该是：创建一类或多类对象。有了它，如果你想创建一个这些类之一的对象，你就可以直接使用工厂而不是使用 new 操作符。

```
stats=new ArrayList<String>();
}
```

- 3. 实现 newThread() 方法。此方法将接收一个 Runnable 接口并返回该 Runnable 接口的线程对象。在我们的例子中，我们生成线程对象的名称，创建新的线程对象，并保存统计信息：

```
@Override
public Thread newThread(Runnable r) {
    Thread t=new Thread(r,name+"-Thread_"+counter);
    counter++;
    stats.add(String.format("Created thread %d with name %s on %s\n", t.getId(),t.getName(),new Date()));
    return t;
}
```

- 4. 实现 getStatistics() 方法；它返回包含所有创建的线程对象的统计数据的 String 对象：

```
public String getStats(){
    StringBuffer buffer=new StringBuffer();
    Iterator<String> it=stats.iterator();
    
    while (it.hasNext()) {
        buffer.append(it.next());
        buffer.append("\n");
    }
    
    return buffer.toString();
}
```

- 5. 创建一个名为 Task 的类并指定它实现 Runnable 接口。在本示例中，这些任务除了休眠 1 秒外什么都不做：

```
public class Task implements Runnable {
    @Override
    public void run() {
        try {
            TimeUnit.SECONDS.sleep(1);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

