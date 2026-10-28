

`getDelay(TimeUnit unit)`: 此方法必须返回到激活日期为止剩余的时间（以单位计），单位由 unit 参数指定。`TimeUnit` 类是一个枚举，包含以下常量：`DAYS, HOURS, MICROSECONDS, MILLISECONDS, MINUTES, NANOSECONDS, and SECONDS.`

在本示例中，你将学习如何通过在其中存储具有不同激活日期的事件来使用 `DelayedQueue` 类。

## Getting ready

本食谱的示例使用 Eclipse IDE 实现。如果您使用 Eclipse 或其他 IDE，例如 NetBeans，请打开它并创建一个新的 Java 项目。

## How to do it...

按照以下步骤实现示例：

- 1. 创建一个名为 `Event` 的类并指定其实现 `Delayed` 接口：
  
  ```
  public class Event implements Delayed {
  ```
- 2. 声明一个名为 `startDate` 的私有 `Date` 属性：
  
  ```
  private final Date startDate;
  ```
- 3. 实现类的构造函数以初始化其属性：
  
  ```
  public Event (Date startDate) {
      this.startDate=startDate;
  }
  ```
- 4. 实现 `compareTo()` 方法。它接收一个 `Delayed` 对象作为参数。返回当前对象的延迟与作为参数传递的对象的延迟之间的差值：
  
  ```
  @Override
  public int compareTo(Delayed o) {
      long result=this.getDelay(TimeUnit.NANOSECONDS)-o.getDelay(TimeUnit.NANOSECONDS);
      if (result<0) {
          return -1;
  } else if (result>0) {
      return 1;
  }
  return 0;
  }
  ```
- 5. 实现 getDelay() 方法。返回对象的开始日期与实际日期之间的差值，单位为作为参数接收的 TimeUnit：

  ```
  public long getDelay(TimeUnit unit) {
      Date now=new Date();
      long diff=startDate.getTime()-now.getTime();
      return unit.convert(diff,TimeUnit.MILLISECONDS);
  }
  ```
- 6. 创建一个名为 Task 的类并指定其实现 Runnable 接口：

  ```
  public class Task implements Runnable {
  ```
- 7. 声明一个名为 id 的私有 int 属性，用于存储标识此任务的数字：

  ```
  private final int id;
  ```
- 8. 声明一个以 Event 类为参数化的私有 DelayQueue 属性 named queue：

  ```
  private final DelayQueue<Event> queue;
  ```
- 9. 实现类的构造函数以初始化其属性：

  ```
  public Task(int id, DelayQueue<Event> queue) {
      this.id=id;
      this.queue=queue;
  }
  ```
- 10. 实现 run() 方法。首先，计算此任务将要创建的事件的激活日期。然后，将等于对象 ID 的秒数添加到实际日期：

  ```
  @Override
  public void run() {
      Date now=new Date();
      Date delay=new Date();
      delay.setTime(now.getTime()+(id*1000));
      System.out.printf("Thread %s: %s\n",id,delay);
  ```
- 11. 使用 add() 方法在队列中存储 100 个事件：

  ```
  for (int i=0; i<100; i++) {
      Event event=new Event(delay);
      queue.add(event);
  }
  ```
- 12. 通过创建名为 Main 的类并向其添加 main() 方法来实现示例的主类：

  ```
  public class Main {
      public static void main(String[] args) throws Exception {
  ```
- 13. 创建一个以 Event 类为参数化的 DelayQueue 对象：

  ```
  DelayQueue<Event> queue=new DelayQueue<>();
  ```
- 14. 创建一个包含五个 Thread 对象的数组来存储您将要执行的任务：

  ```
  Thread threads[]=new Thread[5];
  ```
- 15. 创建五个具有不同 ID 的 Task 对象：

  ```
  for (int i=0; i<threads.length; i++){
      Task task=new Task(i+1, queue);
      threads[i]=new Thread(task);
  }
  ```
- 16. 启动之前创建的全部五个任务：

  ```
  for (int i=0; i<threads.length; i++) {
      threads[i].start();
  }
  ```
- 17. 使用 join() 方法等待线程的结束：

  ```
  for (int i=0; i<threads.length; i++) {
      threads[i].join();
  }
  ```
- 18. 将队列中存储的事件写入控制台。当队列的大小大于零时，使用 poll() 方法获取一个 Event 类。如果它返回 null，则让主线程休眠 500 毫秒以激活更多事件：

  ```
  do {
      int counter=0;
      Event event;
      do {
          event=queue.poll();
          if (event!=null) counter++;
      } while (event!=null);
      System.out.printf("At %s you have read %d events\n", new Date(), counter);
      TimeUnit.MILLISECONDS.sleep(500);
  } while (queue.size()>0);
  }
  ```

## How it works...

在本食谱中，我们实现了 Event 类。此类具有一个唯一的属性，即事件的激活日期，并且它实现了 Delayed 接口。您可以将 Event 对象存储在 DelayQueue 类中。

getDelay() 方法返回激活日期与实际日期之间的纳秒数。两个日期都是 Date 类的对象。您使用了返回日期转换为毫秒的 getTime() 方法。然后，您将此值转换为作为参数接收的 TimeUnit。DelayQueue 类以纳秒为单位工作，但此时对您来说是透明的。

compareTo() 方法如果执行该方法的对象的延迟小于作为参数传递的对象的延迟，则返回小于零的值。如果执行该方法的对象的延迟大于作为参数传递的对象的延迟，则返回大于零的值。如果两个延迟相等，则返回 0。

## There's more...

`DelayQueue` 类还有其他有趣的方法，如下所示：

- `clear()`: 此方法删除队列中的所有元素。
- `offer(E e)`: 这里，E 表示用于参数化 `DelayQueue` 类的类。此方法将作为参数传递的元素插入队列中。
- `peek()`: 此方法检索但不删除队列的第一个元素。
- `take()`: 此方法检索并删除队列的第一个元素。如果没有活动的元素，执行该方法的线程将被阻塞，直到线程获得一些活动元素。

## See also

- 本章中 *Using blocking thread-safe deques* 食谱

## Using thread-safe navigable maps

`ConcurrentNavigableMap` 是一个接口，定义了 Java API 提供的有趣的数据结构，您可以在并发程序中使用。实现 `ConcurrentNavigableMap` 接口的类将元素存储在两个部分中：

- 一个 **key** 唯一标识一个元素
- 定义元素的其余数据，称为 **value**

Java API 还提供了一个实现 `ConcurrentSkipListMap` 的类，它是实现具有 `ConcurrentNavigableMap` 接口行为的非阻塞列表的接口。内部，它使用 **Skip List** 来存储数据。Skip List 是一种基于并行列表的数据结构，允许我们获得与二叉树相关的效率。您可以在 https://en.wikipedia.org/wiki/Skip_list 获取有关 Skip Lists 的更多信息。通过它，您可以获得一个排序的数据结构，而不是排序列表，具有更好的插入、搜索或删除元素的访问时间。

![](img/cc06c59cd21f558f43e9475d7f212170_340_0.png)

Skip List 由 William Pugh 于 1990 年引入。

当您将元素插入到映射中时，映射使用键对它们进行排序；因此，所有元素都将被排序。键必须实现 `Comparable` 接口，或者您必须向映射的构造函数提供一个 `Comparator` 类。该类还提供方法来获取映射的子映射，以及返回具体元素的方法。

在本食谱中，您将学习如何使用 `ConcurrentSkipListMap` 类来实现联系人映射。

## Getting ready

本食谱的示例使用 Eclipse IDE 实现。如果您使用 Eclipse 或其他 IDE，例如 NetBeans，请打开它并创建一个新的 Java 项目。

## How to do it...

按照以下步骤实现示例：

- 1. 创建一个名为 `Contact` 的类：
  ```
  public class Contact {
  ```
- 2. 声明两个私有 `String` 属性，名为 `name` 和 `phone`：
  ```
  private final String name;
  private final String phone;
  ```
- 3. 实现类的构造函数以初始化其属性：
  ```
  public Contact(String name, String phone) {
      this.name=name;
      this.phone=phone;
  }
  ```
- 4. 实现方法以返回 `name` 和 `phone` 属性的值：
  ```
  public String getName() {
      return name;
  }
  
  public String getPhone() {
      return phone;
  }
  ```

