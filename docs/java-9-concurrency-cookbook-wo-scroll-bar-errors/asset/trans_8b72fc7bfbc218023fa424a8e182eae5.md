

8. 现在实现一个名为 Results 的类。该类将以数组的形式存储所搜索数字在矩阵每一行中出现的次数：

```
public class Results {
```

9. 声明一个名为 data 的私有 int 数组：

```
private final int data[];
```

10. 实现该类的构造函数。此构造函数接收一个整数参数，表示数组的元素个数：

```
public Results(int size){
  data=new int[size];
}
```

11. 实现 setData() 方法。此方法接收数组中的位置和值作为参数，并设置数组中该位置的值：

```
public void setData(int position, int value){
  data[position]=value;
}
```

12. 实现 getData() 方法。此方法返回包含结果的数组：

```
public int[] getData(){
  return data;
}
```

13. 现在你已经有了辅助类，是时候实现线程了。首先，实现 Searcher 类。该类将在随机数字矩阵的指定行中查找一个数字。创建一个名为 Searcher 的类并指定其实现 Runnable 接口：

```
public class Searcher implements Runnable {
```

14. 声明两个私有 int 属性，分别是 firstRow 和 lastRow。这两个属性将确定此对象将在其中查找数字的行子集：

```
private final int firstRow;
private final int lastRow;
```

15. 声明一个名为 mock 的私有 MatrixMock 属性：

```
private final MatrixMock mock;
```

16. 声明一个名为 results 的私有 Results 属性：

```
private final Results results;
```

17. 声明一个名为 number 的私有 int 属性，它将存储我们要查找的数字：

```
private final int number;
```

18. 声明一个名为 barrier 的 CyclicBarrier 对象：

```
private final CyclicBarrier barrier;
```

19. 实现类的构造函数，初始化之前声明的所有属性：

```
public Searcher(int firstRow, int lastRow, MatrixMock mock,
    Results results, int number, CyclicBarrier barrier){
    this.firstRow=firstRow;
    this.lastRow=lastRow;
    this.mock=mock;
    this.results=results;
    this.number=number;
    this.barrier=barrier;
}
```

20. 实现 run() 方法来查找数字。它使用一个名为 counter 的内部变量来存储数字在每一行中出现的次数：

```
@Override
public void run() {
    int counter;
```

21. 在控制台打印一条消息，显示分配给此任务的行：

```
System.out.printf("%s: Processing lines from %d to %d.\n",
    Thread.currentThread().getName(),
    firstRow,lastRow);
```

22. 处理分配给此线程的所有行。对于每一行，统计要搜索的数字出现的次数并将该数字存储在 Results 对象的相应位置中：

```
for (int i=firstRow; i<lastRow; i++) {
    int row[]=mock.getRow(i);
    counter=0;
    for (int j=0; j<row.length; j++) {
        if (row[j]==number) {
            counter++;
        }
    }
    results.setData(i, counter);
}
```

23. 在控制台打印一条消息以指示此对象已完成搜索：

```
System.out.printf("%s: Lines processed.\n",
                Thread.currentThread().getName());
```

24. 调用 CyclicBarrier 对象的 await() 方法并添加必要的代码来处理此方法可能抛出的 InterruptedException 和 BrokenBarrierException 异常：

```
try {
    barrier.await();
} catch (InterruptedException e) {
    e.printStackTrace();
} catch (BrokenBarrierException e) {
    e.printStackTrace();
}
```

25. 现在实现计算矩阵中数字总出现次数的类。此类使用存储矩阵每一行中数字出现次数的 Results 对象来进行计算。创建一个名为 Grouper 的类并指定其实现 Runnable 接口：

```
public class Grouper implements Runnable {
```

26. 声明一个名为 results 的私有 Results 属性：

```
private final Results results;
```

27. 实现类的构造函数以初始化 Results 属性：

```
public Grouper(Results results){
    this.results=results;
}
```

28. 实现 run() 方法来计算结果数组中数字的总出现次数：

```
@Override
public void run() {
```

29. 声明一个 int 变量并向控制台写入一条消息以指示进程开始：

```
int finalResult=0;
System.out.printf("Grouper: Processing results...\n");
```

30. 使用 results 对象的 getData() 方法获取数字在每一行中出现的次数。然后，处理数组中的所有元素并将其值加到 finalResult 变量中：

```
int data[]=results.getData();
for (int number:data){
    finalResult+=number;
}
```

31. 在控制台打印结果：

```
System.out.printf("Grouper: Total result: %d.\n", finalResult);
```

32. 最后，通过创建一个名为 Main 的类并向其添加 main() 方法来实现示例的主类：

```
public class Main {
    public static void main(String[] args) {
```

33. 声明并初始化五个常量以存储应用程序的参数：

```
final int ROWS=10000;
final int NUMBERS=1000;
final int SEARCH=5;
final int PARTICIPANTS=5;
final int LINES_PARTICIPANT=2000;
```

34. 创建一个名为 mock 的 MatrixMock 对象。它将有 10,000 行，每行 1,000 个元素。现在，你将搜索数字五：

```
MatrixMock mock=new MatrixMock(ROWS, NUMBERS,SEARCH);
```

35. 创建一个名为 results 的 Results 对象。它将有 10,000 个元素：

```
Results results=new Results(ROWS);
```

36. 创建一个名为 grouper 的 Grouper 对象：

```
Grouper grouper=new Grouper(results);
```

37. 创建一个名为 barrier 的 CyclicBarrier 对象。此对象将等待五个线程。当这五个线程完成后，它将执行之前创建的 Grouper 对象：

```
CyclicBarrier barrier=new CyclicBarrier(PARTICIPANTS,grouper);
```

38. 创建五个 Searcher 对象、五个用于执行它们的线程并启动这五个线程：

```
Searcher searchers[]=new Searcher[PARTICIPANTS];
for (int i=0; i<PARTICIPANTS; i++){
    searchers[i]=new Searcher(i*LINES_PARTICIPANT,
                            (i*LINES_PARTICIPANT)+LINES_PARTICIPANT,
                            mock, results, 5,barrier);
    Thread thread=new Thread(searchers[i]);
    thread.start();
}
System.out.printf("Main: The main thread has finished.\n");
```

## 还有更多...

`CyclicBarrier` 类还有另一个版本的 `await()` 方法：

- `await(long time, TimeUnit unit)`：在此方法中，线程将继续休眠直到被中断，即 `CyclicBarrier` 的内部计数器到达 0 或指定的时间过去。`TimeUnit` 类是一个枚举，具有以下常量：`DAYS`、`HOURS`、`MICROSECONDS`、`MILLISECONDS`、`MINUTES`、`NANOSECONDS` 和 `SECONDS`。

该类还提供了 `getNumberWaiting()` 方法，该方法返回在 `await()` 方法中被阻塞的线程数，以及 `getParties()` 方法，该方法返回将与 `CyclicBarrier` 同步的任务数。

## 重置 CyclicBarrier 对象

`CyclicBarrier` 类与 `CountDownLatch` 类有一些共同点，但它们也有一些差异。其中最重要的一点是 `CyclicBarrier` 对象可以重置为初始状态，将其内部计数器赋值为初始化时的值。

此重置操作可以使用 `CyclicBarrier` 类的 `reset()` 方法完成。当这种情况发生时，所有在 `await()` 方法中等待的线程都会收到一个 `BrokenBarrierException` 异常。此异常在示例食谱中通过打印堆栈跟踪来处理；然而，在更复杂的应用程序中，它可以执行其他操作，例如重新启动执行或从中断点恢复操作。

## 破损的 CyclicBarrier 对象

`CyclicBarrier` 对象可以处于一种特殊状态，即破损状态。当有多个线程在 `await()` 方法中等待并且其中一个被中断时，被中断的线程会收到一个 `InterruptedException` 异常，但其他线程会收到一个 `BrokenBarrierException` 异常；`CyclicBarrier` 进入破损状态。

`CyclicBarrier` 类提供了 `isBroken()` 方法。如果对象处于破损状态，则返回 `true`；否则返回 `false`。
