## 并发编程设计

在本章中，我们将涵盖以下主题：

- 在可能的情况下使用不可变对象
- 通过排序锁避免死锁
- 使用原子变量代替同步
- 尽可能缩短持有锁的时间
- 将线程管理委托给执行器
- 使用并发数据结构而不是自行编程
- 使用延迟初始化时采取预防措施
- 使用 fork/join 框架代替执行器
- 避免在锁内使用阻塞操作
- 避免使用已弃用的方法
- 使用执行器代替线程组
- 使用流处理大数据集
- 其他技巧和窍门

## 引言

实现并发应用程序是一项困难的任务。执行时存在多个线程，它们共享资源，例如文件、内存、对象等。您必须非常注意所做的设计决策。一个糟糕的决定可能会以导致性能低下或简单地引发数据不一致情况的方式影响您的程序。

在本章中，我包含了一些建议来帮助您做出正确的设计决策，从而使您的并发应用程序变得更好。

## 在可能的情况下使用不可变对象

当您使用面向对象编程在 Java 中开发应用程序时，您会创建由属性和方法组成的类。类的 方法决定了您可以用该类执行的操作。属性存储定义对象的数据。通常，在每个类中，您都会实现一些方法来建立属性的值。此外，对象在应用程序运行时会发生变化，您使用这些方法来更改其属性的值。

当您开发并发应用程序时，您必须特别注意被多个线程共享的对象。您必须使用同步机制来保护对此类对象的访问。如果您不使用它，您的应用程序可能会出现数据不一致问题。

在处理并发应用程序时，您可以实现特殊类型的对象。它们称为不可变对象；其主要特征是创建后无法修改。如果您需要更改不可变对象，则必须创建新对象而不是更改对象属性的值。

当您在并发应用程序中使用它们时，这种机制具有以下优点：

- 一旦创建，这些对象就不能被任何线程修改，因此您无需使用任何同步机制来保护对其属性的访问。
- 您不会有任何数据不一致问题。因为这些对象的属性不能被修改，所以您将始终访问到数据的一致副本。

这种方法的唯一缺点是开销：创建新对象而不是修改现有对象。

Java 提供了一些不可变类，例如 String 类。当您有一个 String 对象并尝试为其分配新值时，您是在创建新的 String 对象而不是修改对象旧值。例如，查看以下代码：

```
String var = "hello";
var = "new";
```





```
public String getFirstName() {
    return firstName;
}

public void setFirstName(String firstName) {
    this.firstName = firstName;
}

public String getLastName() {
    return lastName;
}

public void setLastName(String lastName) {
    this.lastName = lastName;
}
public Date getBirthDate() {
    return birthDate;
}

public void setBirthDate(Date birthDate) {
    this.birthDate = birthDate;
}
}
```

你可以通过遵循前面解释的规则将此类转换为不可变类。
结果如下：

```
public final class PersonImmutable {

    final private String firstName;
    final private String lastName;
    final private Date birthDate;

    public PersonImmutable (String firstName, String lastName,
                        String address, Date birthDate) {
        this.firstName=firstName;
        this.lastName=lastName;
        this.birthDate=birthDate;
    }

    public String getFirstName() {
        return firstName;
    }

    public String getLastName() {
        return lastName;
    }
```

```
public Date getBirthDate() {
    return new Date(birthDate.getTime());
}
```

本质上，你遵循了不可变类的基本原则，如下所示：

+   - 该类被标记为 `final`。
+   - 属性被标记为 `final` 和 `private`。
+   - 属性的值只能在类的构造函数中建立。其方法返回属性的值，但不会修改它们。
+   - 对于可变属性（我们例子中的 `birthDate` 属性），我们通过创建新对象在 `get()` 方法中返回其防御性拷贝。

## 还有更多...

不可变对象并非总能使用。分析应用程序的每个类，以决定是否可以将其实现为不可变对象。如果无法将类实现为不可变类，并且其对象被多个线程共享，则必须使用同步机制来保护对该类属性的访问。

## 另请参阅

+   - 本章中 *使用原子变量代替同步* 的配方

## 通过排序锁避免死锁

当你需要在应用程序的方法中获取多个锁时，必须非常小心获取锁的顺序。错误的顺序可能导致死锁情况。

在本配方中，你将实现一个死锁情况的示例，然后学习如何解决它。

+   2. 让我们分析前面的代码。如果一个线程调用 `operation1()` 方法，另一个线程调用 `operation2()` 方法，就可能发生死锁。如果 `operation1()` 和 `operation2()` 同时执行各自的第一句，`operation1()` 方法将等待获取 `lock2` 的控制权，而 `operation2()` 方法将等待获取 `lock1` 的控制权。现在你就有了死锁情况。
+   3. 为了解决这种情况，你可以遵循以下规则：

+   - 如果你需要在不同操作中获取多个锁，尝试在所有方法中以相同顺序锁定它们。
+   - 然后，以相反顺序释放它们，并将锁及其解锁封装在单个类中。这样就不会在代码各处分散同步相关代码。

## 它是如何工作的...

使用此规则，你将避免死锁情况。例如，在前面介绍的情况下，你可以更改 `operation2()` 使其先获取 `lock1` 再获取 `lock2`。现在如果 `operation1()` 和 `operation2()` 同时执行各自的第一句，其中一个将被阻塞等待 `lock1`，另一个将获取 `lock1` 和 `lock2` 并执行其操作。之后，被阻塞的线程将获取 `lock1` 和 `lock2` 锁并执行其操作。

## 还有更多...

你可能会遇到一种情况，需求阻止你在所有操作中以相同顺序获取锁。在这种情况下，你可以使用 `Lock` 类的 `tryLock()` 方法。此方法返回一个 `Boolean` 值以指示你是否已控制该锁。你可以使用 `tryLock()` 方法尝试获取执行操作所需的全部锁。如果你无法控制其中一个锁，必须释放可能已拥有的所有锁并重新开始操作。

## 另请参阅

+   - 本章中 *尽可能缩短持有锁的时间* 的配方

## 使用原子变量代替同步

当你需要在多个线程之间共享数据时，必须使用同步机制保护对该数据的访问。你可以在修改数据的声明方法中使用 `synchronized` 关键字，以便一次只有一个线程可以修改数据。另一种可能性是利用 `Lock` 类创建修改数据的指令的临界区。

从版本 5 开始，Java 包含原子变量。当线程对原子变量执行操作时，类的实现包含一种机制来检查操作是否一步完成。基本上，操作获取变量的值，在局部变量中更改值，然后尝试用新值更改旧值。如果旧值仍然相同，则进行更改。如果不是，则方法重新开始操作。Java 提供了以下类型的原子变量：

+   - `AtomicBoolean`
+   - `AtomicInteger`
+   - `AtomicLong`
+   - `AtomicReference`

在某些情况下，Java 的原子变量比基于同步机制的解决方案提供更好的性能（特别是在我们关心每个单独变量的原子性时）。`java.util.concurrent` 包的一些类使用原子变量而不是同步。在本配方中，你将开发一个示例，展示原子属性如何提供比同步更好的性能。

## 准备工作

本配方的示例已使用 Eclipse IDE 实现。如果你使用 Eclipse 或其他 IDE，例如 NetBeans，请打开它并创建一个新的 Java 项目。

## 如何操作...

按照以下步骤实现示例：

+   1. 创建一个名为 TaskAtomic 的类并指定其实现 Runnable 接口：
+   ```
+   public class TaskAtomic implements Runnable {
+   ```
+   2. 声明一个名为 number 的私有 AtomicInteger 属性：
+   ```
+   private final AtomicInteger number;
+   ```
+   3. 实现类的构造函数以初始化其属性：
+   ```
+   public TaskAtomic () {
+       this.number=new AtomicInteger();
+   }
+   ```
+   4. 实现 run() 方法。在 1,000,000 步的循环中，使用 set() 方法将步数分配给原子属性作为值：
+   ```
+   @Override
+   public void run() {
+       for (int i=0; i<1000000; i++) {
+           number.set(i);
+       }
+   }
+   ```
+   5. 创建一个名为 TaskLock 的类并指定其实现 Runnable 接口：
+   ```
+   public class TaskLock implements Runnable {
+   ```
+   6. 声明一个名为 number 的私有 int 属性和一个名为 lock 的私有 Lock 属性：
+   ```
+   private Lock lock;
+   private int number;
+   ```
+   7. 实现类的构造函数以初始化其属性：
+   ```
+   public TaskLock() {
+       this.lock=new ReentrantLock();
+   }
+   ```

8. 实现 run() 方法。在 1,000,000 步的循环中，将步数分配给整数属性。你必须在赋值前获取锁，并在赋值后释放它：

```
@Override
public void run() {
    for (int i=0; i<1000000; i++) {
        lock.lock();
        number=i;
        lock.unlock();
    }
}
```

9. 通过创建一个名为 Main 的类并向其添加 main() 方法来实现示例的主类：

```
public class Main {
    public static void main(String[] args) {
```

10. 创建一个名为 atomicTask 的 TaskAtomic 对象：

```
TaskAtomic atomicTask=new TaskAtomic();
```

11. 创建一个名为 lockTask 的 TaskLock 对象：

```
TaskLock lockTask=new TaskLock();
```

12. 声明线程数并创建一个 Thread 对象数组来存储线程：

```
int numberThreads=50;
Thread threads[]=new Thread[numberThreads];
Date begin, end;
```

13. 启动指定数量的线程以执行 TaskLock 对象。计算并在控制台中写入其执行时间：

```
begin=new Date();
for (int i=0; i<numberThreads; i++) {
    threads[i]=new Thread(lockTask);
    threads[i].start();
}
```





```
for (int i=0; i<numberThreads; i++) {
    try {
        threads[i].join();
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
}
end=new Date();

System.out.printf("Main: Lock results: %d\n",
(end.getTime()-begin.getTime()));
```

14. 启动指定数量的线程来执行 TaskAtomic 对象。计算并在控制台中输出其执行时间：

```
begin=new Date();
for (int i=0; i<numberThreads; i++) {
    threads[i]=new Thread(atomicTask);
    threads[i].start();
}

for (int i=0; i<numberThreads; i++) {
    try {
        threads[i].join();
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
}
end=new Date();

System.out.printf("Main: Atomic results: %d\n",
(end.getTime()-begin.getTime()));
```

## How it works...

当你执行示例时，你会看到使用原子变量的 TaskAtomic 任务的执行时间始终优于使用锁的 TaskLock 任务。如果你使用 `synchronized` 关键字代替锁，也会得到类似的结果。

本菜谱的结论是，使用原子变量会比其他同步方法带来更好的性能。如果没有适合你需求的原子类型，也许你可以尝试实现自己的原子类型。

## See also

- `Chapter 8` 中 *Customizing Concurrency Classes* 的 *Implementing your own atomic object* 菜谱

## Holding locks for as short time as possible

锁与其他同步机制一样，允许定义一个临界区，一次只有一个线程可以执行。你必须非常小心地定义临界区。它只能包含真正需要互斥的指令。如果临界区包含长时间的操作尤其如此。如果临界区包含不使用共享资源的冗长操作，应用程序的性能将比可能的情况更差。

在本菜谱中，你将实现一个示例来查看临界区内有长时间操作任务与临界区外有长时间操作任务的性能差异。

### Getting ready

本菜谱的示例使用 Eclipse IDE 实现。如果你使用 Eclipse 或其他 IDE，例如 NetBeans，打开它并创建一个新的 Java 项目。

### How to do it...

按照以下步骤实现示例：

1. 创建一个名为 `Operations` 的类：

```
public class Operations {
```

2. 实现一个名为 `readData()` 的 `public static` 方法。它使当前线程休眠 500 毫秒：

```
public static void readData() {
    try {
        TimeUnit.MILLISECONDS.sleep(500);
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
}
```

3. 实现一个名为 `writeData()` 的 public static 方法。它使当前线程休眠 500 毫秒：

```
public static void writeData(){
    try {
        TimeUnit.MILLISECONDS.sleep(500);
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
}
```

4. 实现一个名为 `processData()` 的 public static 方法。它使当前线程休眠 2,000 毫秒：

```
public static void processData(){
    try {
        TimeUnit.SECONDS.sleep(2);
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
}
```

5. 实现一个名为 `Task1` 的类并指定它实现 `Runnable` 接口：

```
public class Task1 implements Runnable {
```

6. 声明一个名为 `lock` 的私有 `Lock` 属性：

```
private final Lock lock;
```

7. 实现类的构造函数以初始化其属性：

```
public Task1 (Lock lock) {
    this.lock=lock;
}
```

8. 实现 `run()` 方法。获取锁，调用 `Operations` 类的三个操作，然后释放锁：

```
@Override
public void run() {
    lock.lock();
    Operations.readData();
    Operations.processData();
    Operations.writeData();
    lock.unlock();
}
```

9. 实现一个名为 `Task2` 的类并指定它实现 `Runnable` 接口：

```
public class Task2 implements Runnable {
```

10. 声明一个名为 `lock` 的私有 `Lock` 属性：

```
private final Lock lock;
```

11. 实现类的构造函数以初始化其属性：

```
public Task2 (Lock lock) {
    this.lock=lock;
}
```

12. 实现 `run()` 方法。获取锁，调用 `readData()` 操作，然后释放锁。接着，调用 `processData()` 方法，获取锁，调用 `writeData()` 操作，然后释放锁：

```
@Override
public void run() {
    lock.lock();
    Operations.readData();
    lock.unlock();
    Operations.processData();
    lock.lock();
    Operations.writeData();
    lock.unlock();
}
```

13. 通过创建名为 `Main` 的类并向其添加 `main()` 方法来实现示例的主类：

```
public class Main {

    public static void main(String[] args) {
```

14. 创建一个名为 `lock` 的 `Lock` 对象，一个名为 `task1` 的 `Task1` 对象，一个名为 `task2` 的 `Task2` 对象，以及一个包含 10 个线程的数组：

```
Lock lock=new ReentrantLock();
Task1 task1=new Task1(lock);
Task2 task2=new Task2(lock);
Thread threads[]=new Thread[10];
```

15. 启动 10 个线程来执行第一个任务并控制其执行时间：

```
Date begin, end;

begin=new Date();
for (int i=0; i<threads.length; i++) {
    threads[i]=new Thread(task1);
    threads[i].start();
}

for (int i=0; i<threads.length; i++) {
    try {
        threads[i].join();
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
}
end=new Date();
System.out.printf("Main: First Approach: %d\n", (end.getTime()-begin.getTime()));
```

16. 启动 10 个线程来执行第二个任务并控制其执行时间：

```
begin=new Date();
for (int i=0; i<threads.length; i++) {
    threads[i]=new Thread(task2);
    threads[i].start();
}

for (int i=0; i<threads.length; i++) {
    try {
        threads[i].join();
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
}
end=new Date();
System.out.printf("Main: Second Approach: %d\n", (end.getTime()-begin.getTime()));
```

### How it works...

如果执行示例，你会看到两种方法执行时间之间存在很大差异。所有操作都在临界区内的任务比另一个任务花费更长时间。

当你需要实现一个由锁保护的代码块时，仔细分析它，只包含必要的指令。将方法拆分为多个临界区，并在必要时使用多个锁，以获得应用程序的最佳性能。

### See also

- 本章中的 *Avoiding deadlocks by ordering locks* 菜谱

## Delegating the management of threads to executors

在 Java 5 之前，Java Concurrency API 中，当我们想要实现一个并发应用程序时，我们必须自己管理线程。首先我们习惯于实现 `Runnable` 接口或 `Thread` 类的扩展。然后，我们习惯于创建 `thread` 对象并使用其 `start()` 方法启动其执行。我们还必须控制其状态以知道线程是否已完成执行或仍在运行。

在 Java 5 版本中，执行器作为执行线程池的提供者的概念出现了。这种机制由 `Executor` 和 `ExecutorService` 接口以及 `ThreadPoolExecutor` 和 `ScheduledThreadPoolExecutor` 类实现，允许你只专注于任务逻辑的实现。你实现任务并将其发送给执行器。它有一个线程池，正是这个池负责线程的创建、管理和结束。在 Java 7 版本中，fork/join 框架中出现了执行器机制的另一种实现，专门用于可以分解为更小子问题的任务。这种方法具有众多优势，如下所示：

- 我们不必为所有任务创建线程。当我们将任务发送给执行器并由池中的线程执行时，我们节省了创建新线程所用的时间。如果我们的应用程序必须执行大量任务，总节省的时间将是显著的，应用程序的性能将更好。





- 如果我们创建较少的线程，我们的应用程序也会使用更少的内存。这还能让我们的应用获得更好的性能。
- 我们可以通过实现`Runnable`或`Callable`接口来构建在执行器中运行的并发任务。`Callable`接口允许我们实现返回结果的任务，这相比传统任务具有很大的优势。
- 当我们向执行器发送任务时，它会返回一个`Future`对象，使我们能够轻松了解任务的状态、返回结果以及是否已完成执行。
- 我们可以通过`ScheduledThreadPoolExecutor`类实现的特殊执行器来调度任务并重复执行它们。
- 我们可以轻松控制执行器使用的资源。我们可以设置池中的最大线程数，这样我们的执行器在同一时间运行的线程绝不会超过这个数量。

与直接使用线程相比，使用执行器有很多优势。在本节中，你将实现一个示例，展示使用执行器比自行创建线程能获得更好的性能。

## 准备就绪

本节示例使用 Eclipse IDE 实现。如果你使用 Eclipse 或其他 IDE（如 NetBeans），请打开它并创建一个新的 Java 项目。

## 操作步骤...

按照以下步骤实现该示例：

- 1. 创建一个名为`Task`的类，并指定它实现`Runnable`接口：
- 2. 实现`run()`方法。创建一个包含 1,000,000 次循环的循环，并在每一步中对整数变量进行一些数学运算：

```
public class Task implements Runnable {
```

```
@Override
public void run() {
    int r;
    for (int i=0; i<1000000; i++) {
```

```
r=0;
r++;
r++;
r*=r;
}
```

3. 通过创建一个名为`Main`的类并添加`main()`方法来实现示例的主类：

```
public class Main {
    public static void main(String[] args) {
```

4. 创建 1,000 个线程来执行 1,000 个任务对象，并等待它们完成，同时控制总执行时间：

```
Thread threads[]=new Thread[1000];
Date start,end;

start=new Date();
for (int i=0; i<threads.length; i++) {
    Task task=new Task();
    threads[i]=new Thread(task);
    threads[i].start();
}

for (int i=0; i<threads.length; i++) {
    try {
        threads[i].join();
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
}
end=new Date();
System.out.printf("Main: Threads: %d\n", (end.getTime()-start.getTime()));
```

5. 创建一个`Executor`对象，向其中发送 1,000 个`Task`对象，并等待它们完成。测量总执行时间：

```
ThreadPoolExecutor executor=(ThreadPoolExecutor)Executors
    .newCachedThreadPool();

start=new Date();
```

```
for (int i=0; i<threads.length; i++) {
    Task task=new Task();
    executor.execute(task);
}
executor.shutdown();
try {
    executor.awaitTermination(1, TimeUnit.DAYS);
} catch (InterruptedException e) {
    e.printStackTrace();
}
end=new Date();
System.out.printf("Main: Executor: %d\n", (end.getTime()-start.getTime()));
```

## 工作原理...

在整个示例的执行过程中，我们始终发现执行器比直接创建线程花费的执行时间更短。如果你的应用程序需要执行大量任务，最好使用执行器。

## 另请参阅

- 本章中的*使用执行器代替线程组*和*使用 fork/join 框架代替执行器*小节

## 使用并发数据结构而不是自己编写

数据结构是每个程序中必不可少的部分。你总是需要管理存储在数据结构中的数据。数组、列表或树是常见数据结构的例子。Java API 提供了许多现成的数据结构，但在处理并发应用程序时，你必须小心，因为 Java API 提供的并非所有数据结构都是**线程安全**的。如果你选择的数据结构不是线程安全的，你的应用程序中就可能出现数据不一致的情况。

当你希望在并发应用程序中使用数据结构时，必须查看实现该数据结构的类的文档，以确认它支持并发操作。Java 提供了以下两种并发数据结构：

- **非阻塞数据结构：** 这些数据结构提供的所有插入或取出元素的操作，如果当前无法完成（因为数据结构已满或为空），将返回空值。
- **阻塞数据结构：** 这些数据结构提供了与非阻塞数据结构相同的操作。此外，它们还提供插入和取出数据的操作；如果这些操作无法立即完成，将使线程阻塞，直到能够执行这些操作。

以下是 Java API 提供的一些可用于并发应用程序的数据结构：

- `ConcurrentLinkedDeque`：这是一种基于链接节点的非阻塞数据结构，允许你在结构的开头或末尾插入数据。
- `LinkedBlockingDeque`：这是一种基于链接节点的阻塞数据结构。它可以具有固定容量。你可以在结构的开头或末尾插入元素。它提供的操作如果无法立即完成，将阻塞线程，直到你能执行该操作。
- `ConcurrentLinkedQueue`：这是一个非阻塞队列，允许你在队列末尾插入元素，并从队列开头取出元素。
- `ArrayBlockingQueue`：这是一个固定大小的阻塞队列。你在队列末尾插入元素，并从队列开头取出元素。它提供的操作如果因为队列已满或为空而无法完成，将让线程休眠，直到你能执行该操作。
- `LinkedBlockingQueue`：这是一个阻塞队列，允许你在队列末尾插入元素，并从队列开头取出元素。它提供的操作如果因为队列已满或为空而无法完成，将让线程休眠，直到你能执行该操作。
- `DelayQueue`：这是一个具有延迟元素的`LinkedBlockingQueue`队列。插入此队列的每个元素都必须实现`Delayed`接口。在元素的延迟时间变为 0 之前，无法从列表中取出该元素。

## 使用惰性初始化时的注意事项

**惰性初始化**是一种常见的编程技术，它将对象创建延迟到首次需要时。这通常会导致对象的初始化在操作的实现中完成，而不是在类的构造函数中完成。这项技术的主要优点是可以节省内存。因为你只创建应用程序执行所必需的对象。你可能在一个类中声明了许多对象，但并非每次程序执行都会用到每个对象；因此，你的应用程序不会为你在某次程序执行中未使用的对象占用内存。这个优点对于在资源受限环境中运行的应用程序非常有用。

相比之下，这项技术也存在缺点，即它可能在应用程序中造成性能问题，因为对象是在操作中首次使用时才创建的。

如果你在并发应用程序中使用这项技术，它还可能导致问题。由于多个线程可能同时执行某个操作，它们可能同时创建同一个对象，这种情况可能会引发问题。这一点对于**单例**类尤其重要。应用程序中这些类只能有一个对象，而如前所述，并发应用程序可能创建多个对象。请考虑以下代码：

```
public static DBConnection getConnection() {
    if (connection==null) {
        connection=new DBConnection();
    }
    return connection;
}
```

这是单例类中使用惰性初始化获取该类在应用程序中唯一对象引用的典型方法。如果对象尚未创建，则创建该对象。最后，它总是返回该对象。

如果两个或更多线程同时执行第一条语句的比较（`connection == null`），它们都会创建一个 Connection 对象。这不是一种理想的情况。

在本节中，你将实现一个针对惰性初始化问题的优雅解决方案。





## 准备工作

本食谱的示例使用 Eclipse IDE 实现。如果您使用 Eclipse 或其他 IDE，例如 NetBeans，请打开它并创建一个新的 Java 项目。

## 如何做...

按照以下步骤实现示例：

- 1. 创建一个名为 DBConnectionOK 的类：
```
public class DBConnectionOK {
```
- 2. 声明一个私有构造函数。写入执行它的线程的名称：
```
private DBConnectionOK() {
    System.out.printf("%s: Connection created.\n",
        Thread.currentThread().getName());
}
```
- 3. 声明一个名为 LazyDBConnectionOK 的私有静态类。它有一个名为 INSTANCE 的私有静态 final DBConnectionOK 实例：
```
private static class LazyDBConnection {
    private static final DBConnectionOK INSTANCE = new
        DBConnectionOK();
}
```
- 4. 实现 getConnection() 方法。它不接收任何参数并返回一个 DBConnectionOK 对象。它返回 INSTANCE 对象：
```
public static DBConnectionOK getConnection() {
    return LazyDBConnection.INSTANCE;
}
```
- 5. 创建一个名为 Task 的类并指定它实现 Runnable 接口。实现 run() 方法。调用 DBConnectionOK 类的 getConnection() 方法：
```
public class Task implements Runnable {

    @Override
    public void run() {

        System.out.printf("%s: Getting the connection...\n",
                Thread.currentThread().getName());
        DBConnectionOK connection=DBConnectionOK.getConnection();
        System.out.printf("%s: End\n",
                Thread.currentThread().getName());
    }

}
```
- 6. 通过创建一个名为 Main 的类并向其添加 main() 方法来实现示例的主类：
```
public class Main {
    public static void main(String[] args) {
```
- 7. 创建 20 个 Task 对象和 20 个线程来执行它们：
```
for (int i=0; i<20; i++){
    Task task=new Task();
    Thread thread=new Thread(task);
    thread.start();
}
```

## 它是如何工作的...

示例的关键是 getConnection() 方法和私有静态类 LazyDBConnection 实例。当第一个线程调用 getConnection() 方法时，LazyDBConnection 类通过调用 DBConnection 类的构造函数来初始化 INSTANCE 对象。此对象由 getConnection() 方法返回。当其余线程调用 getConnection() 方法时，对象已经创建，因此所有线程都使用仅创建一次的同一对象。

当您运行示例时，您将看到 20 个任务的开始和结束消息，但只有一个创建消息。

## 使用 fork/join 框架代替执行器

执行器允许您避免线程的创建和管理。您通过实现 Runnable 或 Callable 接口来实现任务并将其发送给执行器。它有一个线程池并使用其中一个线程来执行任务。

Java 7 提供了 fork/join 框架中的一种新型执行器。此执行器在 ForkJoinPool 类中实现，专为可使用分而治之技术拆分为更小部分的问题而设计。当您为 fork/join 框架实现任务时，您必须检查要解决的问题的大小。如果它大于预定义的大小，您将问题分为两个或多个子类别，并根据您进行的划分数量创建相同数量的子任务。任务使用 fork() 操作将这些子任务发送到 ForkJoinPool 类，并使用 join() 操作等待其完成。

对于这类问题，fork/join 池比经典执行器获得更好的性能。在本食谱中，您将实现一个示例，您可以在其中检查这一点。

## 准备工作

本食谱的示例使用 Eclipse IDE 实现。如果您使用 Eclipse 或其他 IDE，例如 NetBeans，请打开它并创建一个新的 Java 项目。

## 如何做...

按照以下步骤实现示例：

- 1. 创建一个名为 TaskFJ 的类并指定它扩展 RecursiveAction 类：
- 2. 声明一个名为 array 的私有 int 数组：
- 3. 声明两个名为 start 和 end 的私有 int 属性：
```
public class TaskFJ extends RecursiveAction {
```
```
private final int array[];
```
```
private final int start, end;
```
- 4. 实现类的构造函数以初始化其属性：
```
public TaskFJ(int array[], int start, int end) {
    this.array=array;
    this.start=start;
    this.end=end;
}
```
- 5. 实现 compute() 方法。如果此任务必须处理超过 1,000 个元素的块（由 start 和 end 属性确定），则创建两个 TaskFJ 对象，使用 fork() 方法将它们发送到 ForkJoinPool 类，并使用 join() 方法等待其完成：
```
@Override
protected void compute() {
    if (end-start>1000) {
        int mid=(start+end)/2;
        TaskFJ task1=new TaskFJ(array,start,mid);
        TaskFJ task2=new TaskFJ(array,mid,end);
        task1.fork();
        task2.fork();
        task1.join();
        task2.join();
```
- 6. 否则，增加此任务必须处理的元素。每次递增操作后，让线程休眠 1 毫秒：
```
} else {
    for (int i=start; i<end; i++) {
        array[i]++;
        try {
            TimeUnit.MILLISECONDS.sleep(1);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```
- 7. 创建一个名为 Task 的类并指定它实现 Runnable 接口：
```
public class Task implements Runnable {
```
- 8. 声明一个名为 array 的私有 int 数字数组：
```
private final int array[];
```
- 9. 实现类的构造函数以初始化其属性：
```
public Task(int array[]) {
    this.array=array;
}
```
- 10. 实现 run() 方法。递增数组的所有元素。每次递增操作后，让线程休眠 1 毫秒：
```
@Override
public void run() {
    for (int i=0; i<array.length; i++) {
        array[i]++;
        try {
            TimeUnit.MILLISECONDS.sleep(1);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```
- 11. 通过创建一个名为 Main 的类并向其添加 main() 方法来实现示例的主类：
```
public class Main {

    public static void main(String[] args) {
```
- 12. 创建一个包含 100,000 个元素的 int 数组：
```
int array[]=new int[100000];
```
- 13. 创建一个 Task 对象和一个 ThreadPoolExecutor 对象并执行它们。通过控制任务运行的时间来执行任务：
```
Task task=new Task(array);
ExecutorService executor=Executors.newCachedThreadPool();

Date start,end;
start=new Date();
executor.execute(task);
executor.shutdown();
try {
    executor.awaitTermination(1, TimeUnit.DAYS);
} catch (InterruptedException e) {
    e.printStackTrace();
}
```
```
end=new Date();
System.out.printf("Main: Executor: %d\n", (end.getTime()-start.getTime()));
```
- 14. 创建一个 TaskFJ 对象和一个 ForkJoinPool 对象并执行它们。通过控制任务运行的时间来执行任务：
```
TaskFJ taskFJ=new TaskFJ(array,1,100000);
ForkJoinPool pool=new ForkJoinPool();
start=new Date();
pool.execute(taskFJ);
pool.shutdown();
try {
pool.awaitTermination(1, TimeUnit.DAYS);
} catch (InterruptedException e) {
e.printStackTrace();
}
end=new Date();
System.out.printf("Core: Fork/Join: %d\n", (end.getTime()-start.getTime()));
}
```

## 它是如何工作的...

当您执行示例时，您将看到 ForkJoinPool 和 TaskFJ 类比 ThreadPoolExecutor 和 Task 类获得更好的性能。

如果您必须解决一个可以使用分而治之技术拆分的问题，请使用 ForkJoinPool 类而不是 ThreadPoolExecutor 类。您将获得更好的性能。

## 另请参阅

- 本章的 Delegating the management of threads to executors 食谱





## 避免在锁内使用阻塞操作

**阻塞操作** 是指在事件发生前阻塞当前线程执行的<|message|>操作。典型的阻塞操作是涉及与控制台、文件或网络进行输入输出操作的操作。

如果你在锁的临界区内使用阻塞操作，你会降低应用程序的性能。在线程等待完成阻塞操作所需的事件时，应用程序的其余部分也可能在等待同一个事件；然而，其他线程都无法访问临界区并执行其代码（临界区的代码）。

在本示例中，你将实现这种情况的一个示例。线程在临界区内从控制台读取一行。此指令会使应用程序的其余线程在用户输入该行之前被阻塞。

## 准备工作

本示例使用 Eclipse IDE 实现。如果你使用 Eclipse 或其他 IDE，例如 NetBeans，请打开它并创建一个新的 Java 项目。

## 如何做...

按照以下步骤实现示例：

- 1. 创建一个名为 Task 的类并指定其实现 Runnable 接口：
```
public class Task implements Runnable {
```
- 2. 声明一个名为 lock 的私有 Lock 属性：
```
private final Lock lock;
```
- 3. 实现类的构造函数以初始化其属性：
```
public Task (Lock lock) {
    this.lock=lock;
}
```
- 4. 实现 run() 方法：
```
@Override
public void run() {
    System.out.printf("%s: Starting\n", Thread.currentThread().getName());
```
- 5. 使用 lock() 方法获取锁：
```
lock.lock();
```
- 6. 调用 criticalSection() 方法：
```
try {
    criticalSection();
```
- 7. 从控制台读取一行：
```
System.out.printf("%s: Press a key to continue: \n", Thread.currentThread().getName());
InputStreamReader converter = new InputStreamReader(System.in);
BufferedReader in = new BufferedReader(converter);
String line=in.readLine();
} catch (IOException e) {
    e.printStackTrace();
```
- 8. 在 finally 部分使用 unlock() 方法释放锁：
```
} finally {
    lock.unlock();
}
```
- 9. 实现 criticalSection() 方法。等待一个随机时间段：
```
private void criticalSection() {
    Random random=new Random();
    int wait=random.nextInt(10);
    System.out.printf("%s: Wait for %d seconds\n", Thread.currentThread().getName(),wait);
    try {
        TimeUnit.SECONDS.sleep(wait);
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
}
```
- 10. 通过创建一个名为 Main 的类并向其添加 main() 方法来实现应用程序的主类：
```
public class Main {
    public static void main(String[] args) {
```
- 11. 创建一个新的 ReentrantLock 对象名为 lock。创建 10 个 Task 对象和 10 个线程来执行它们：
```
ReentrantLock lock=new ReentrantLock();
for (int i=0; i<10; i++) {
    Task task=new Task(lock);
    Thread thread=new Thread(task);
    thread.start();
}
```

## 其工作原理...

当你执行此示例时，10 个线程开始执行，但只有一个进入在 run() 方法中实现的临界区。由于每个任务在释放锁之前都会从控制台读取一行，因此所有应用程序都将在你在控制台输入文本之前被阻塞。

## 另请参阅

- The Holding locks for as short a time period as possible recipe

## 避免使用已弃用的方法

Java 并发 API 也包含已弃用的操作。这些是在 API 的第一个版本中包含的操作，但现在你不应该使用它们。它们已被其他操作取代，这些操作比原始操作实现了更好的实践。

## 使用执行器代替线程组

`ThreadGroup` 类提供了一种机制，可将线程分组到层次结构中，以便你只需一次调用即可对属于线程组的线程执行操作。默认情况下，所有线程都属于同一组，但在创建线程时你可以指定不同的组。

无论如何，线程组不提供任何使其使用变得有趣的功能：

- 你必须创建线程并管理其状态
- 控制线程组中所有线程状态的方法已被弃用，其使用不受鼓励

如果你需要将线程分组到一个通用结构下，最好使用 `Executor` 实现，例如 `ThreadPoolExecutor`。它提供了更多功能，如下所示：

- 你不必担心线程的管理。执行器创建并重用它们以节省执行资源。
- 你可以通过实现 `Runnable` 或 `Callable` 接口来实现并发任务。`Callable` 接口允许你实现返回结果的任务，这比传统任务具有很大的优势。
- 当你将任务发送到执行器时，它会返回一个 `Future` 对象，允许你轻松地了解任务的状态以及如果任务已完成执行则返回的结果。
- 你可以使用 `ScheduledThreadPoolExecutor` 类实现的特殊执行器安排任务并重复执行它们。
- 你可以轻松控制执行器使用的资源。你可以建立池中线程的最大数量，以便执行器永远不会同时运行超过该数量的任务。

因此，最好不要使用线程组，而改用执行器。

## 另请参阅

- 本章中 *Delegating the management of threads to executors* recipe

## 使用流处理大数据集

`Stream` 接口是元素的序列，可以对其进行过滤和转换以获得最终结果，顺序或并行。此最终结果可以是原始数据类型（整数、长整型...）、对象或数据结构。以下是最能定义 `Stream` 的特征：

- 流是数据的序列，而不是数据结构。
- 你可以从不同的源创建流，例如集合（列表、数组...）、文件、字符串或提供流元素的类。
- 你无法访问流的单个元素。
- 你无法修改流的源。
- 流定义了两种操作：中间操作，它产生一个新的 `Stream` 接口，允许你转换、过滤、映射或排序流的元素，以及终端操作，它生成操作的最终结果。流管道由零个或多个中间操作和一个最终操作组成。
- 中间操作是惰性的。它们在终端操作开始执行之前不会被执行。如果 Java 检测到中间操作对流的元素或一组元素的操作不会影响操作最终结果，它可以避免执行。

当你需要实现以并发方式处理大数据集的操作时，你可以使用 Java 并发 API 的不同元素来实现它。Java 线程要么使用 fork/join 框架，要么使用 Executor 框架，但我认为并行流是最好的选择。在本示例中，我们将实现一个示例来解释使用并行流提供的优势。

## 准备工作

本示例使用 Eclipse IDE 实现。如果你使用 Eclipse 或其他 IDE，例如 NetBeans，请打开它并创建一个新的 Java 项目。

## 如何做...

按照以下步骤实现示例：

- 1. 创建一个名为 Person 的类。此类将有六个属性来定义一个人的一些基本特征。我们将实现获取() 和设置() 属性值的方法，但它们在此处不包含：
```
public class Person {
    private int id;
    private String firstName;
    private String lastName;
    private Date birthDate;
    private int salary;
    private double coefficient;
}
```
- 2. 现在，实现一个名为 PersonGenerator 的类。此类将只有一个名为 generatedPersonList() 的方法，用于生成具有参数指定的大小的随机值的 Person 对象列表。这是此类的源代码：
```
public class PersonGenerator {
    public static List<Person> generatePersonList (int size) {
        List<Person> ret = new ArrayList<>();

        String firstNames[] = {"Mary","Patricia","Linda",
            "Barbara","Elizabeth","James",
            "John","Robert","Michael","William"};
        String lastNames[] = {"Smith","Jones","Taylor",
            "Williams","Brown","Davies",
            "Evans","Wilson","Thomas","Roberts"};

        Random randomGenerator=new Random();
        for (int i=0; i<size; i++) {
            Person person=new Person();
            person.setId(i);
            person.setFirstName(firstNames[randomGenerator.nextInt(10)]);
            person.setLastName(lastNames[randomGenerator.nextInt(10)]);
            person.setSalary(randomGenerator.nextInt(100000));
            person.setCoefficient(randomGenerator.nextDouble()*10);
            Calendar calendar=Calendar.getInstance();
            calendar.add(Calendar.YEAR, -randomGenerator.nextInt(30));
            Date birthDate=calendar.getTime();
```





```
personList.add(person);
}
return;
}
```

6. 如果 List 包含超过 1,000 个元素，我们将创建两个子任务并将列表的一部分的处理委托给它们：

```
PersonMapTask child1, child2;

child1=new PersonMapTask(persons.subList(0,persons.size()/2), personMap);
child2=new PersonMapTask(persons.subList(persons.size()/2, persons.size()), personMap);

invokeAll(child1,child2);
}
}
```

7. 最后，实现带有 main() 方法的 Main 类。首先，生成一个包含 100,000 个随机 Person 对象的列表：

```
public class Main {

public static void main (String[] args) {
List<Person> persons=PersonGenerator
.generatePersonList(100000);
```

8. 然后，比较两种生成以名称作为键、List 的一部分且 Person 作为值的方法。List 将使用并行 Stream 函数和 collect() 方法，使用 groupingByConcurrent() 收集器：

```
Date start, end;

start = new Date();
Map<String, List<Person>> personsByName = persons.parallelStream()
.collect(Collectors.groupingByConcurrent(p -> p.getFirstName()));
end = new Date();
System.out.printf("Collect: %d - %d\n", personsByName.size(), end.getTime()-start.getTime());
```

## 其他技巧和窍门

在本最终配方中，我们包含了本章其他配方中未包含的其他技巧和窍门：

- 尽可能使用并发设计模式：在软件工程中，设计模式是对常见问题的解决方案。它们在软件开发和并发应用中被广泛使用，并非例外。诸如信号、会合和互斥等模式定义了如何在具体情况下实现并发应用程序，并且已被用于实现并发实用程序。
- 在尽可能高的级别实现并发：丰富的线程 API，例如 Java 并发 API，为您提供了在应用程序中实现并发的不同类。尝试使用提供更高抽象级别的类。这将使您更容易实现算法，并且它们经过优化以提供比直接使用线程更好的性能。因此，性能不会成为问题。
- 考虑可扩展性：在实现并发算法时，主要目标之一是利用计算机的所有资源，特别是处理器或核心的数量。但这个数字可能会随着时间的推移而变化。在设计并发算法时，不要预设应用程序将要执行的核心或处理器数量。动态获取有关系统的信息。例如，在 Java 中，您可以使用 `Runtime.getRuntime().availableProcessors()` 方法获取它，并使您的算法使用此信息来计算它将要执行的任务数量。
- 尽可能优先使用本地线程变量而不是静态和共享变量：线程本地变量是一种特殊的变量类型。每个任务对此变量都有一个独立的值，因此您不需要任何同步机制来保护对其的访问。

## 另请参阅

- 本章中的所有配方
