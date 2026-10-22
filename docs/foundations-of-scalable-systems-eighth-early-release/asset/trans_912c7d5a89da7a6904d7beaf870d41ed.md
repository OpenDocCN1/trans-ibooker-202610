

如图 4-3 所示，在机器级别，这三个操作是独立的，并未被当作单个 *原子* 操作处理。 所谓原子，我们指的是一个不能被中断的操作，因此一旦开始就会执行到完成。

由于自增操作在机器级别不是原子的，一个线程可以从内存中将计数器的值加载到 CPU 寄存器中，但在它写回自增后的值之前，调度器会抢占该线程并允许另一个线程开始。 该线程从内存中加载计数器的旧值并写回自增后的值。 最终原始线程再次执行，并写回其自增后的值，该值恰好与内存中已有的值相同。

这意味着我们丢失了一次更新。 从上面计数器代码的 10 次测试来看，我们发现平均在 50000 次自增中会发生约 5 次。 因此此类事件很少见，但即使每 1000 万次发生 1 次，结果仍然是错误的。

| Thread 1 | Thread 2 |
|---|---|
| Reads (x) into register | |
| Register value + 6 | |
| Writes register value to (x) | |
| | Reads (x) into register |
| | Register value + 1 |
| | Writes register value to (x) |

| Thread 1 | Thread 2 |
|---|---|
| Reads (x) into register | |
| Register value + 6 | |
| | Reads (x) into register |
| | Register value + 1 |
| | Writes register value to (x) |
| Writes register value to (x) | |

![](img/936a72197720a5158b16541313af61c9_98_0.png)

![](img/936a72197720a5158b16541313af61c9_98_1.png)

*Figure 4-3. Increments are not atomic at the machine level*

当我们以这种方式丢失更新时，就称为竞争条件。 每当多个线程对某些共享状态进行更改时，就会发生竞争条件，在此例中是一个简单的计数器。 本质上，线程的不同交错会导致不同的结果。

竞争条件是阴险的、邪恶的错误，因为它们的发生通常很罕见，而且很难检测，因为大多数时候答案是正确的。 尝试使用 1000 个线程而不是 50000 运行多线程计数器代码示例，你就会亲眼看到这一点。 我十次中有九次得到了正确答案。

因此，这个情况可以概括为“相同代码，偶尔不同结果”。 就像我说的——竞争条件很邪恶！ 好在如果你采取一些预防措施，根除它们是相当直接的。

关键是识别并保护 *临界区*。 临界区是更新共享数据结构的代码段，因此如果被多个线程访问，必须以原子方式执行。 自增共享计数器的例子就是临界区的例子。 另一个例子是从列表中删除一个项目。 我们需要删除列表的头节点，并将对列表头的引用从被删除的节点移动到列表中的下一个节点。 这两个操作必须以原子方式执行以维护列表的完整性。 这是一个临界区。

在 Java 中，`synchronized` 关键字定义了临界区。 如果用它来修饰一个方法，那么当多个线程尝试在同一个共享对象上调用该方法时，只允许一个线程进入临界区。 所有其他线程都会阻塞，直到线程退出 synchronized 方法，届时调度器会选择下一个线程来执行临界区。 我们说临界区的执行是串行的，因为一次只有一个线程可以执行其中的代码。

要修复计数器示例，你只需将 `inc()` 方法识别为临界区并使其成为 synchronized 方法，即：

```
synchronized public void inc() {
    count++;
}
```

尽情测试吧。 你总是会得到正确答案。 稍微正式地说，这意味着调度器抛给我们的线程的任何交错都会始终产生正确的结果。

synchronized 关键字也可以应用于方法内的语句块。 例如，我们可以将上面的例子重写为：

```
public void inc() {
synchronized(this){
    count++;
}
}
```

在底层，每个 Java 对象都有一个 *监视器锁*，有时也称为内部锁，作为其运行时表示的一部分。 监视器就像长途巴士上的卫生间——一次只允许（也应该！）一个人进入，使用时门锁会阻止其他人进入。

在我们这个完全卫生的 Java 运行时环境中，线程必须获取监视器锁才能进入 synchronized 方法或 synchronized 语句块。 任何时候只有一个线程可以拥有该锁，因此执行是串行的。 这就是 Java 和类似语言实现临界区的基本方式。

作为经验法则，你应该尽可能保持临界区尽可能小，以使串行化代码最小化。 这对性能和可扩展性都有积极影响。 我稍后会再次讨论这个话题，但我其实又在谈论阿姆达尔定律，如第 2 章中介绍的那样。 Synchronized 块是阿姆达尔所描述的系统的串行部分，而它们执行的时间越长，我们对系统可扩展性的潜力就越小。

## 死锁

为了确保多线程代码的正确结果，我解释说我们必须限制固有的非确定性以串行化对临界区的访问。 这可以避免竞争条件。 然而，如果我们不小心，我们可能会编写出限制非确定性太多以至于程序停止的代码。 而且永远不会继续。 这正式被称为死锁。

死锁发生在两个或多个线程永远阻塞且都无法继续时。 这发生在线程需要对共享资源集进行独占访问，并且线程以不同顺序获取锁时。 这在下面的示例中有所说明，其中两个线程需要对临界区 A 和 B 进行独占访问。 线程 1 获取临界区 A 的锁，线程 2 获取临界区 B 的锁。 然后两者都永远阻塞，因为它们无法获取继续所需的锁。

*Two threads sharing access to two shared variables via synchronized blocks*

- 1. thread 1: enters critical section A
- 2. thread 2: enters critical section B
- 3. thread 1: blocks on entry to critical section B
- 4. thread 2: blocks on entry to critical section A
- 5. Both threads wait forever

死锁，也称为致命拥抱，会导致程序停止。 不需要丰富的想象力就能意识到这会造成各种不良后果。 我正愉快地发着短信，而我的自动驾驶汽车正载着我去酒吧。 突然间，车辆代码死锁了。 结果不会好。

死锁发生在比上面简单示例更微妙的情况下。 经典的例子是哲学家进餐问题。 故事是这样的。

五位哲学家围坐在共享的桌子旁。 作为哲学家，他们花了很多时间深入思考。 在深思熟虑之间，他们通过吃面前盘子里的食物来补充大脑功能。 因此，哲学家要么在吃饭，要么在思考，要么在两者之间转换。

此外，哲学家们必须身体非常接近、极其灵巧并且接种了 Covid19 疫苗，因为他们共用筷子吃饭。 桌上只有五双筷子，放在每位哲学家之间。 当一位哲学家想吃饭时，他们会遵循先拿起左筷子，然后拿起右筷子的协议。 一旦准备好再次思考，他们会先放下右筷子，然后放下左筷子。

## Figure 4-4. The Dining Philosophers Problem

图 4-4 描绘了我们的哲学家，每人都有唯一的编号。 由于每个人都在并发地进食或思考，我们可以将每个哲学家建模为一个线程。 代码如示例 4-7 所示。 共享的筷子由 Java Object 类的实例表示。 由于一次只有一个对象可以持有对象的监视器锁，因此它们被用作哲学家获取进食所需筷子的临界区的进入条件。 吃完后，筷子会被放回桌上，并在每个筷子上释放锁，以便邻近的哲学家在准备好时进食。

## Example 4-7. The Philosopher Thread

```
1  public class Philosopher implements Runnable {
2
3      private final Object leftChopStick;
4      private final Object rightChopStick;
5
6      Philosopher(Object leftChopStick, Object rightChopStick) {
7          this.leftChopStick = leftChopStick;
8          this.rightChopStick = rightChopStick;
9      }
10     private void LogEvent(String event) throws
InterruptedException {
11       System.out.println(Thread.currentThread()
12                             .getName() + " " + event);
13       Thread.sleep(1000);
14     }
15
16     public void run() {
17         try {
18             while (true) {
19                 LogEvent(": Thinking deeply");
20                 synchronized (leftChopStick) {
21                     LogEvent(" : Picked up left chop stick");
22                     synchronized (rightChopStick) {
23                         LogEvent(": Picked up right chopstick - eating");
24                         LogEvent(": Put down right chopstick");
25                     }
26                     LogEvent(": Put down left chopstick. Ate too much");
27                 }
28             } // end while
```

