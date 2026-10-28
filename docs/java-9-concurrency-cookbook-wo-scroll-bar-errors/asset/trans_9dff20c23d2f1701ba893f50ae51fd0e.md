

9. 通过创建名为 Main 的类并向其添加 main() 方法来实现示例的主类：

```
public class Main {
    public static void main(String[] args) {
```

10. 声明一个名为 THREADS 的常量并将其值赋为 100。创建一个包含 1,000 个元素的 AtomicIntegerArray 对象：

```
final int THREADS=100;
AtomicIntegerArray vector=new AtomicIntegerArray(1000);
```

11. 创建一个 Incrementer 任务来操作前面创建的原子数组：

```
Incrementer incrementer=new Incrementer(vector);
```

12. 创建一个 Decrementer 任务来操作前面创建的原子数组：

```
Decrementer decrementer=new Decrementer(vector);
```

13. 创建两个数组来存储 100 个 Thread 对象：

```
Thread threadIncrementer[]=new Thread[THREADS];
Thread threadDecrementer[]=new Thread[THREADS];
```

14. 创建并启动 100 个线程来执行 Incrementer 任务，再创建并启动另外 100 个线程来执行 Decrementer 任务。将线程存储在前面创建的数组中：

```
for (int i=0; i<THREADS; i++) {
    threadIncrementer[i]=new Thread(incrementer);
    threadDecrementer[i]=new Thread(decrementer);

    threadIncrementer[i].start();
    threadDecrementer[i].start();
}
```

15. 使用 join() 方法等待线程的结束：

```
for (int i=0; i<100; i++) {
    try {
        threadIncrementer[i].join();
        threadDecrementer[i].join();
```
```
} catch (InterruptedException e) {
    e.printStackTrace();
}
}
```

16. 在控制台中写入与零不同的原子数组元素。使用 get() 方法获取原子数组的元素：

```
int errors=0;
for (int i=0; i<vector.length(); i++) {
    if (vector.get(i)!=0) {
        System.out.println("Vector["+i+"] : "+vector.get(i));
        errors++;
    }
}
if (errors==0) {
    System.out.printf("No errors found\n");
}
```

17. 在控制台中写入一条消息以指示示例的结束：

```
System.out.println("Main: End of the example");
```

## How it works...

在本示例中，你实现了两个不同的任务来操作一个 AtomicIntegerArray 对象：

- Incrementer：此类使用 getAndIncrement() 方法递增数组的所有元素
- Decrementer：此类使用 getAndDecrement() 方法递减数组的所有元素

在 Main 类中，你创建了包含 1,000 个元素的 AtomicIntegerArray，然后执行了 100 个递增器和 100 个递减器任务。在这些任务结束时，如果没有不一致错误，数组的所有元素都必须具有值 0。如果你执行程序，你会看到程序仅向控制台写入最终消息，因为所有元素都为零。

## There's more...

如今，Java 提供了另一个原子数组类。它被称为 `AtomicLongArray` 类，它提供了与 `AtomicIntegerArray` 类相同的 方法。

这些类提供的其他有趣方法有：

- `get(int i)`: 返回参数指定的数组位置的值
- `set(int i, int newValue)`: 设置参数指定的数组位置的值。

## See also

- 本章中的 *Using atomic variables* 配方

## Using the volatile keyword

几乎每个应用程序都会读取和写入计算机的主内存中的数据。出于性能原因，这些操作并非直接在内存中执行。CPU 具有缓存内存系统，因此应用程序将数据写入缓存，然后数据从缓存移动到主内存。

在多线程应用程序中，并发线程在不同的 CPU 或 CPU 内的不同核心中运行。当线程修改存储在内存中的变量时，修改是在其运行的缓存或 CPU 或核心中进行的。但是，没有保证该修改何时到达主内存。如果另一个线程想要读取数据的值，则可能不会读取修改后的值，因为它不在计算机的主内存中。

为了解决这个问题（还有其他解决方案，例如 `synchronized` 关键字），Java 语言包含 `volatile` 关键字。它是一个修饰符，允许你指定变量必须始终从主内存读取并存储到主内存中，而不是 CPU 的缓存中。当其他线程对变量的实际值可见性很重要时，而对该变量的访问顺序并不重要时，你应该使用 volatile 关键字。在这种情况下，`volatile` 关键字会给你更好的性能，因为它不需要获取任何监视器或锁来访问变量。相反，如果对变量的访问顺序很重要，你必须使用另一种同步机制。

在本配方中，你将学习如何使用 volatile 关键字及其使用效果。

## Getting ready

本配方的示例已使用 Eclipse IDE 实现。如果你使用 Eclipse 或其他 IDE，例如 NetBeans，请打开它并创建一个新的 Java 项目。

## How to do it...

按照以下步骤实现示例：

1. 创建一个名为 Flag 的类，其中包含一个公共 Boolean 属性名为 flag，初始化为 true 值：
```
public class Flag {
    public boolean flag=true;
}
```
2. 创建一个名为 VolatileFlag 的类，其中包含一个公共 Boolean 属性名为 flag，初始化为 true 值。我们在该属性的声明中添加 volatile 修饰符：
```
public class VolatileFlag {
    public volatile boolean flag=true;
}
```
3. 创建一个名为 Task 的类并指定其实现 Runnable 接口。它有一个私有 Flag 属性和一个用于初始化它的构造函数：
```
public class Task implements Runnable {
    private Flag flag;
    public Task(Flag flag) {
        this.flag = flag;
    }
}
```
4. 实现此任务的 run() 方法。它将在 flag 属性的值为 true 时递增一个 int 变量。然后，写入变量的最终值：
```
@Override
public void run() {
    int i = 0;
```
```
while (flag.flag) {
    i++;
}
System.out.printf("VolatileTask: Stopped %d - %s\n", i,
        new Date());
}
```

5. 创建一个名为 VolatileTask 的类并指定其实现 Runnable 接口。它有一个私有 VolatileFlag 属性和一个用于初始化它的构造函数：

```
public class VolatileTask implements Runnable {

    private VolatileFlag flag;
    public VolatileTask(VolatileFlag flag) {
        this.flag = flag;
    }
}
```

6. 实现此任务的 run() 方法。它与 Task 类中的方法相同，因此此处不包含：

7. 使用 main() 方法实现 Main 类。首先，创建 VolatileFlag、Flag、VolatileTask 和 Task 类的四个对象：

```
public class Main {

    public static void main(String[] args) {
        VolatileFlag volatileFlag=new VolatileFlag();
        Flag flag=new Flag();

        VolatileTask vt=new VolatileTask(volatileFlag);
        Task t=new Task(flag);
```

8. 然后，创建两个线程来执行任务，启动它们，并让主线程休眠一秒：

```
Thread thread=new Thread(vt);
thread.start();
thread=new Thread(t);
thread.start();

try {
    TimeUnit.SECONDS.sleep(1);
} catch (InterruptedException e) {
    e.printStackTrace();
}
```

9. 然后，将 volatileFlag 变量的值更改为停止 volatileTask 的执行，并让主线程休眠一秒：

```
System.out.printf("Main: Going to stop volatile task: %s\n", new Date());
volatileFlag.flag=false;
System.out.printf("Main: Volatile task stopped: %s\n", new Date());

try {
    TimeUnit.SECONDS.sleep(1);
} catch (InterruptedException e) {
    e.printStackTrace();
}
```

10. 最后，更改 task 对象的值以停止任务的执行，并让主线程休眠一秒：

```
System.out.printf("Main: Going to stop task: %s\n", new Date());
flag.flag=false;
System.out.printf("Main: Volatile stop flag changed: %s\n", new Date());

try {
    TimeUnit.SECONDS.sleep(1);
} catch (InterruptedException e) {
    e.printStackTrace();
}
```

## How it works...

以下屏幕截图显示了示例的输出：

![](img/cc06c59cd21f558f43e9475d7f212170_368_0.png)

