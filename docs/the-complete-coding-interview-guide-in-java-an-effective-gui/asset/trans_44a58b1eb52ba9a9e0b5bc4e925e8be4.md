

## 编程挑战 14 – ThreadLocal

**问题：** Java `ThreadLocal` 是什么？

**解答：** Java 线程共享相同的内存。然而，有时我们需要为每个线程提供专用的内存。Java 提供了 `ThreadLocal` 作为为每个线程单独存储和检索值的一种手段。单个 `ThreadLocal` 实例可以存储和检索多个线程的值。如果线程 A 在同一个 `ThreadLocal` 实例中存储 x 值，而线程 B 存储 y 值，那么之后，线程 A 检索 x 值，线程 B 检索 y 值。Java `ThreadLocal` 通常用于以下两种场景：

- 1. 提供每个线程的实例（线程安全和内存效率）
- 2. 提供每个线程的上下文

## 编程挑战 15 – submit() 与 execute()

**问题：** 解释 `ExecutorService#submit()` 和 `Executor#execute()` 方法之间主要的区别。

**解答：** 虽然这两种方法都用于提交 `Runnable` 任务以供执行，但它们并不相同。主要区别只需查看它们的签名即可观察到。请注意，`submit()` 返回一个结果（即表示该任务的 `Future` 对象），而 `execute()` 返回 `void`。返回的 `Future` 对象可用于稍后以编程方式取消正在运行的线程（提前）。此外，通过使用 `Future#get()` 方法，我们可以等待任务完成。如果我们提交一个 `Callable`，那么 `Future#get()` 方法将返回调用 `Callable#call()` 方法的结果。

## 编程挑战 16 – interrupted() 和 isInterrupted()

**问题：** 解释 `interrupted()` 和 `isInterrupted()` 方法之间主要的区别。

**解答：** Java 多线程中断技术使用一个内部标志，称为 *中断状态*。`Thread.interrupt()` 方法中断当前线程并将此标志设置为 `true`。

`interrupted()` 和 `isInterrupted()` 方法之间主要的区别在于 `interrupted()` 方法会清除中断状态，而 `isInterrupted()` 不会。

如果线程被中断，则 Thread.interrupted() 将返回 true。然而，除了测试之外，如果当前线程被中断，Thread.interrupted() 会清除线程的中断状态（即将其设置为 false）。

非静态的 isInterrupted() 方法不会更改中断状态标志。

作为经验法则，在捕获 InterruptedException 之后，不要忘记通过调用 Thread.currentThread().interrupt() 来恢复中断。这样，我们代码的调用者就会意识到中断。

## 编程挑战 17 – 取消线程

**问题：** 我们如何停止或取消线程？

**解答：** Java 不提供预先停止线程的方法。因此，为了取消任务，常见做法是依赖于使用标志条件的循环。任务的职责是定期检查此标志，并在发现标志被设置时尽快停止。请注意，此标志通常声明为 volatile（也称为轻量级同步机制）。作为 volatile 标志，它不会被线程缓存，并且对其的操作不会在内存中重新排序；因此，线程无法看到旧值。读取 volatile 字段的任何线程都将看到最近写入的值。这正是我们为了将取消操作传达给所有对该操作感兴趣的正在运行的线程所需要的。下面的图表说明了这一点：

![](img/6709a71136a6b327c831c6c15aff2f6a_707_0.png)

请注意，volatile 变量不适合读-改-写场景。对于此类场景，我们将依赖原子变量（例如 AtomicBoolean、AtomicInteger 和 AtomicReference）。

在本图书的代码包中，你可以找到取消线程的示例。该应用程序名为 CancelThread。

## 编程挑战 18 – 线程间共享数据

**问题：** 我们如何在这两个线程之间共享数据？

**解答：** 在两个（或更多）线程之间共享数据可以通过线程安全的共享对象或数据结构来完成。Java 带有内置的一组线程安全数据结构，例如 BlockingQueue、LinkedBlockingQueue 和 ConcurrentLinkedDeque。依靠这些数据结构在线程之间共享数据非常方便，因为你不必担心线程安全和线程间通信。

## 编程挑战 19 – ReadWriteLock

**问题：** 解释 Java 中 ReadWriteLock 是什么。

**解答：** ReadWriteLock 的主要目的是在并发环境中维持读写操作的效率和线程安全。它通过锁分段概念实现这一目标。换句话说，ReadWriteLock 使用读写锁。更准确地说，ReadWriteLock 保留一对锁：一个用于只读操作，一个用于写入操作。只要没有写入者线程，多个读取者线程可以同时持有读锁（共享悲观锁）。一次只能有一个写入者进行写入（独占/悲观锁定）。因此，ReadWriteLock 可以显著提高应用程序的性能。

除了 ReadWriteLock，Java 还提供 ReentrantReadWriteLock 和 StampedLock。ReentrantReadWriteLock 类将可重入锁定概念（参见编程挑战 4）添加到 ReadWriteLock。另一方面，StampedLock 的性能优于 ReentrantReadWriteLock 并支持乐观读。但它不可重入；因此，容易发生死锁。

## 编程挑战 20 – 生产者-消费者

**问题：** 为著名的生产者-消费者问题提供一个实现。

> 注意
> 这是任何 Java 多线程面试中的一个热门问题！

**解答：** 生产者-消费者问题是一种设计模式，可以表示如下：

![](img/6709a71136a6b327c831c6c15aff2f6a_709_0.png)

16.4 – 生产者-消费者设计模式

最常见的是，在此模式中，生产者线程和消费者线程通过队列进行通信（生产者将数据入队，消费者将数据出队）以及针对所建模业务的特定规则集。此队列称为数据缓冲区。当然，根据流程设计，其他数据结构也可以充当数据缓冲区的角色。

现在，让我们假设以下场景（规则集）：

- - 如果数据缓冲区为空，则生产者生产一个产品（通过将其添加到数据缓冲区）。
- - 如果数据缓冲区不为空，则消费者消费一个产品（通过将其从数据缓冲区中移除）。
- - 只要数据缓冲区不为空，生产者就等待。
- - 只要数据缓冲区为空，消费者就等待。

接下来，让我们通过两种常见方法解决此场景。我们将从基于 wait() 和 notify() 方法的解决方案开始。

### 通过 wait() 和 notify() 实现生产者-消费者

一些面试官可能会要求你使用 wait() 和 notify() 方法实现生产者-消费者应用程序。换句话说，他们不允许你使用内置的线程安全队列，例如 BlockingQueue。

例如，让我们考虑数据缓冲区（队列）由 LinkedList 表示，即非线程安全数据结构。为了确保此共享 LinkedList 能以线程安全的方式被生产者和消费者访问，我们依赖 synchronized 关键字。

### 生产者

如果队列不为空，则生产者等待消费者完成。为此，生产者依赖 wait() 方法，如下所示：

```
synchronized (queue) {
    while (!queue.isEmpty()) {
        logger.info("Queue is not empty ...");
        queue.wait();
    }
}
```

另一方面，如果队列为空，则生产者入队一个产品并通过 notify() 通知消费者线程，如下所示：

```
synchronized (queue) {
    String product = "product-" + rnd.nextInt(1000);

    // simulate the production time
    Thread.sleep(rnd.nextInt(MAX_PROD_TIME_MS));

    queue.add(product);
    logger.info(() -> "Produced: " + product);

    queue.notify();
}
```

在将产品添加到队列后，消费者应该准备好消费它。

### 消费者

如果队列为空，则消费者等待生产者完成。为此，生产者依赖 wait() 方法，如下所示：

```
synchronized (queue) {
    while (queue.isEmpty()) {
        logger.info("Queue is empty ...");
        queue.wait();
    }
}
```

