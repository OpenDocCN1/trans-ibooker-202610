

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

