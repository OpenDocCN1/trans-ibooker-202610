

12. 实现另一个带有两个参数的 tryLock() 方法：一个名为 time 的 long 参数和一个名为 unit 的 TimeUnit 参数。调用 sync 对象的 tryAcquireNanos() 方法：

```
@Override
public boolean tryLock(long time, TimeUnit unit) throws InterruptedException {
    return sync.tryAcquireNanos(1, TimeUnit.NANOSECONDS
        .convert(time, unit));
}
```

13. 实现 unlock() 方法。调用 sync 对象的 release() 方法：

```
@Override
public void unlock() {
    sync.release(1);
}
```

14. 实现 newCondition() 方法。创建一个 sync 对象的内部类的新对象，即 ConditionObject：

```
@Override
public Condition newCondition() {
    return sync.newCondition();
}
```

15. 创建一个名为 Task 的类并指定其实现 Runnable 接口：

```
public class Task implements Runnable {
```

16. 声明一个名为 lock 的私有 MyLock 属性：

```
private final MyLock lock;
```

17. 声明一个名为 name 的私有 String 属性：

```
private final String name;
```

18. 实现类的构造函数以初始化其属性：

```
public Task(String name, MyLock lock){
    this.lock=lock;
    this.name=name;
}
```

19. 实现类的 run() 方法。获取锁，将线程休眠 2 秒，然后释放锁对象：

```
@Override
public void run() {
    lock.lock();
    System.out.printf("Task: %s: Take the lock\n",name);
    try {
        TimeUnit.SECONDS.sleep(2);
        System.out.printf("Task: %s: Free the lock\n",name);
    } catch (InterruptedException e) {
        e.printStackTrace();
    } finally {
        lock.unlock();
    }
}
```

20. 通过创建名为 Main 的类并包含 main() 方法来实现示例的主类：

```
public class Main {
    public static void main(String[] args) {
```

21. 创建一个名为 lock 的 MyLock 对象：

```
MyLock lock=new MyLock();
```

22. 创建并执行 10 个 Task 任务：

```
for (int i=0; i<10; i++){
    Task task=new Task("Task-"+i,lock);
    Thread thread=new Thread(task);
    thread.start();
}
```

23. 使用 tryLock() 方法尝试获取锁。等待一秒，如果未获取到锁，则写入一条消息并重试：

```
boolean value;
do {
    try {
        value=lock.tryLock(1,TimeUnit.SECONDS);
        if (!value) {
            System.out.printf("Main: Trying to get the Lock\n");
        }
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
    value=false;
} while (!value);
```

24. 写入一条获取到锁的消息并释放它：

```
System.out.printf("Main: Got the lock\n");
lock.unlock();
```

25. 写入一条表示程序结束的消息：

```
System.out.printf("Main: End of the program\n");
```

## 它是如何工作的...

Java 并发 API 提供了一个类，可用于实现具有锁或信号量功能的同步机制。它被称为 AbstractQueuedSynchronizer，顾名思义，它是一个抽象类。它提供操作来控制对临界区的访问并管理等待访问该区的被阻塞线程队列。这些操作基于两个抽象方法：

- tryAcquire()：此方法用于尝试获取对临界区的访问。如果调用它的线程可以访问临界区，则该方法返回 true 值。否则，它返回 false 值。
- tryRelease()：此方法用于尝试释放对临界区的访问。如果调用它的线程可以释放访问，则该方法返回 true 值。否则，它返回 false 值。

在这些方法中，你必须实现用于控制对临界区访问的机制。在这种情况下，你实现了扩展 AbstractQueuedSynchronizer 类的 MyAbstractQueuedSynchronizer 类，并使用 AtomicInteger 变量实现抽象方法来控制对临界区的访问。此变量将在锁空闲时存储值 0，因此线程可以访问临界区，并在锁被占用时存储值 1，因此线程无法访问临界区。

你使用了 AtomicInteger 类提供的 compareAndSet() 方法，该方法尝试将你作为第一个参数指定的值更改为你作为第二个参数指定的值。为了实现 tryAcquire() 方法，你尝试将原子变量的值从零更改为一。同样，为了实现 tryRelease() 方法，你尝试将原子变量的值从一更改为零。

你必须实现 `AtomicInteger` 类，因为 `AbstractQueuedSynchronizer` 类的其他实现（例如 `ReentrantLock` 使用的实现）是作为内部私有类实现的。这是在使用它的类中完成的，因此你无法访问它。

然后，你实现了 `MyLock` 类。此类实现 `Lock` 接口并具有 `MyQueuedSynchronizer` 对象作为属性。为了实现 `Lock` 接口的所有方法，你使用了 `MyQueuedSynchronizer` 对象的方法。

最后，你实现了实现 `Runnable` 接口的 `Task` 类，并使用 `MyLock` 对象获取对临界区的访问。此临界区使线程休眠 2 秒。`main` 类创建一个 `MyLock` 对象并运行 10 个共享锁的 `Task` 对象。`main` 类还使用 `tryLock()` 方法尝试获取对锁的访问。

当你执行示例时，可以看到只有一条线程可以访问临界区，并且当该线程完成后，另一条线程会获得访问权限。

你可以使用自己的 `Lock` 接口来记录其使用情况的日志消息，控制其被锁定的时间，或实现高级同步机制来控制例如对资源的访问，使其仅在特定时间可用。

## 还有更多...

`AbstractQueuedSynchronizer` 类提供了两个可用于管理锁状态的方法。它们是 `getState()` 和 `setState()` 方法。这些方法接收并返回一个整数值，表示锁的状态。你本可以使用它们代替 `AtomicInteger` 属性来存储锁的状态。

Java 并发 API 提供了另一个用于实现同步机制的类。它是 `AbstractQueuedLongSynchronizer` 类，它等同于 `AbstractQueuedSynchronizer`，但使用 long 属性来存储线程的状态。

## 另请参阅

- `Chapter 2`，*Basic Thread Synchronization* 中 *Synchronizing a block of code with locks* 食谱

## 基于优先级的传输队列实现

Java 9 API 提供了多种用于并发应用程序的数据结构。其中，我们想重点介绍以下两种数据结构：

- LinkedTransferQueue：此数据结构被认为用于具有生产者/消费者结构的程序中。在此类应用程序中，你有一个或多个数据生产者和一个或多个数据消费者，并且一个数据结构被所有它们共享。生产者将数据放入数据结构，消费者从中取出。如果数据结构为空，消费者将被阻塞，直到有数据可消费。如果它已满，生产者将被阻塞，直到有空间放置数据。
- PriorityBlockingQueue：在此数据结构中，元素以有序方式存储。它们必须实现 Comparable 接口的 compareTo() 方法。当你将元素插入结构时，它会与结构中的元素进行比较，直到找到其位置。

LinkedTransferQueue 的元素按到达顺序存储，因此先到达的元素先被消费。在你想要开发生产者/消费者程序时，数据按某种优先级而不是到达时间被消费，这可能是这种情况。在本食谱中，你将学习如何实现一个可用于生产者/消费者问题的数据结构，其元素按优先级排序；优先级更高的元素将先被消费。

## 准备工作

本食谱的示例已使用 Eclipse IDE 实现。如果你使用 Eclipse 或其他 IDE，例如 NetBeans，请打开它并创建一个新的 Java 项目。

## 如何操作...

按照以下步骤实现示例：

1. 创建一个名为 MyPriorityTransferQueue 的类，使其扩展 PriorityBlockingQueue 类并实现 TransferQueue 接口：

```
public class MyPriorityTransferQueue<E> extends
    PriorityBlockingQueue<E> implements TransferQueue<E> {
```

