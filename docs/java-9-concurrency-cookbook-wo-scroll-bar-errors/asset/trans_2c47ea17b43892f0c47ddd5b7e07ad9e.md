

当你执行程序时，异常被抛出，但程序并未停止。在 Main 类中，你使用原始任务调用了 ForkJoinTask 类的 isCompletedAbnormally() 方法。该方法如果该任务或其子任务之一抛出异常则返回 true。你还使用了同一对象的 getException() 方法来获取其抛出的 Exception 对象。

当你在任务中抛出未检查的异常时，它也会影响其父任务（将任务发送到 ForkJoinPool 类的任务）以及其父任务的父任务，依此类推。如果你查看整个程序的输出，你会发现某些任务的完成输出消息不存在。那些任务的开始消息如下：

```
Task: Starting form 0 to 100
Task: Starting form 0 to 50
Task: Starting form 0 to 25
Task: Starting form 0 to 12
Task: Starting form 0 to 6
```

这些任务是抛出异常的任务及其父任务。它们全部异常完成。在使用 ForkJoinPool 和 ForkJoinTask 对象开发程序时，如果你不希望出现此行为，请考虑这一点。

以下屏幕截图显示了此示例的部分执行：

![](img/cc06c59cd21f558f43e9475d7f212170_253_0.png)

## There's more...

在此示例中，你使用了 `join()` 方法来等待任务完成并获取其结果。你也可以使用以下两种版本的 `get()` 方法来实现此目的：

- `get()`: 此版本的 `get()` 方法如果 `ForkJoinTask` 已完成执行，则返回 `compute()` 方法返回的值，否则等待其完成。
- `get(long timeout, TimeUnit unit)`: 此版本的 `get()` 方法如果任务结果不可用，则等待指定时间。如果指定时间段过去且结果仍不可用，该方法返回 `null` 值。`TimeUnit` 类是一个枚举，具有以下常量：`DAYS`、`HOURS`、`MICROSECONDS`、`MILLISECONDS`、`MINUTES`、`NANOSECONDS` 和 `SECONDS`。

`get()` 和 `join()` 方法之间主要有两个区别：

- `join()` 方法不能被中断。如果你中断调用 `join()` 方法的线程，该方法会抛出 `InterruptedException`。
- 虽然 `get()` 方法如果任务抛出任何未检查的异常会返回 `ExecutionException`，但 `join()` 方法会返回 `RuntimeException`。

如果你不是抛出异常，而是使用 `ForkJoinTask` 类的 `completeExceptionally()` 方法，可以获得与示例中相同的结果。代码如下：

```
Exception e=new Exception("This task throws an Exception: "+
"Task from "+start+" to "+end);
completeExceptionally(e);
```

## See also

- 本章中 *Creating a fork/join pool* 配方

## Canceling a task

当你在 ForkJoinPool 类中执行 ForkJoinTask 对象时，可以在它们开始执行前取消它们。ForkJoinTask 类为此提供了 cancel() 方法。当你想取消任务时，需要注意以下几点：

- ForkJoinPool 类不提供任何取消其正在运行或等待在池中的所有任务的方法
- 当你取消一个任务时，你不会取消该任务已执行的任务

在本配方中，你将实现一个取消 ForkJoinTask 对象的示例。你将在数组中查找一个数字的位置。找到该数字的第一个任务将取消剩余的任务。由于该功能未由 fork/join 框架提供，你将实现一个辅助类来执行此取消操作。

## Getting ready...

本配方的示例使用 Eclipse IDE 实现。如果你使用 Eclipse 或其他 IDE，例如 NetBeans，请打开它并创建一个新的 Java 项目。

## How to do it...

按照以下步骤实现示例：

- 1. 创建一个名为 ArrayGenerator 的类。此类将生成指定大小的随机整数数组。实现一个名为 generateArray() 的方法。它将生成数字数组。它接收数组大小作为参数：

```
public class ArrayGenerator {
    public int[] generateArray(int size) {
        int array[]=new int[size];
        Random random=new Random();
        for (int i=0; i<size; i++){
            array[i]=random.nextInt(10);
        }
        return array;
    }
}
```

- 8. 声明一个名为 numbers 的私有 int 数组：

```
private int numbers[];
```

- 9. 声明两个私有 int 属性，名为 start 和 end。这些属性将确定此任务要处理的数组元素：

```
private int start, end;
```

- 10. 声明一个私有 int 属性，名为 number，用于存储你要查找的数字：

```
private int number;
```

- 11. 声明一个私有 TaskManager 属性，名为 manager。你将使用此对象来取消所有任务：

```
private TaskManager manager;
```

- 12. 声明一个私有 int 常量并将其初始化为 -1。它将在任务未找到数字时作为返回值：

```
private final static int NOT_FOUND=-1;
```

- 13. 实现类的构造函数以初始化其属性：

```
public SearchNumberTask(int numbers[], int start, int end, int number, TaskManager manager){
    this.numbers=numbers;
    this.start=start;
    this.end=end;
    this.number=number;
    this.manager=manager;
}
```

- 14. 实现 compute() 方法。首先通过写入控制台消息指示 start 和 end 属性的值来开始该方法：

```
@Override
protected Integer compute() {
    System.out.println("Task: "+start+":"+end);
```

- 15. 如果 start 和 end 属性之间的差值大于 10（任务要处理超过 10 个数组元素），则调用 launchTasks() 方法将此任务的工作划分为两个子任务：

```
int ret;
if (end-start>10) {
    ret=launchTasks();
```

- 16. 否则，在调用 lookForNumber() 方法的任务要处理的数组块中查找数字：

```
} else {
    ret=lookForNumber();
}
```

- 17. 返回任务的结果：

```
return ret;
```

- 18. 实现 lookForNumber() 方法：

```
private int lookForNumber() {
```

- 19. 对于此任务要处理的元素块中的所有元素，将存储在该元素中的值与你要查找的数字进行比较。如果它们相等，则向控制台写入一条消息，指示在此情况下使用 TaskManager 对象的 cancelTasks() 方法来取消所有任务，并返回找到数字的元素位置：

```
for (int i=start; i<end; i++){
    if (numbers[i]==number) {
        System.out.printf("Task: Number %d found in position %d\n", number,i);
        manager.cancelTasks(this);
        return i;
    }
}
```

- 20. 在循环内部，让任务休眠一秒：

```
try {
    TimeUnit.SECONDS.sleep(1);
} catch (InterruptedException e) {
    e.printStackTrace();
}
}
```

- 27. 通过创建一个名为 Main 的类并包含 main() 方法来实现示例的主类：

```
public class Main {
    public static void main(String[] args) {
```

- 28. 使用 ArrayGenerator 类创建包含 1000 个数字的数组：

```
ArrayGenerator generator=new ArrayGenerator();
int array[]=generator.generateArray(1000);
```

- 29. 创建一个 TaskManager 对象：

```
TaskManager manager=new TaskManager();
```

- 30. 使用默认构造函数创建 ForkJoinPool 对象：

```
ForkJoinPool pool=new ForkJoinPool();
```

- 31. 创建一个 Task 对象来处理之前生成的数组：

```
SearchNumberTask task=new SearchNumberTask (array,0,1000,
    5,manager);
```

- 32. 使用 execute() 方法异步在池中执行任务：

```
pool.execute(task);
```

- 33. 使用 shutdown() 方法关闭池：

```
pool.shutdown();
```

- 34. 使用 ForkJoinPool 类的 awaitTermination() 方法等待任务完成：

```
try {
    pool.awaitTermination(1, TimeUnit.DAYS);
} catch (InterruptedException e) {
    e.printStackTrace();
}
```

- 35. 向控制台写入一条消息，指示程序结束：

```
System.out.printf("Main: The program has finished\n");
```

