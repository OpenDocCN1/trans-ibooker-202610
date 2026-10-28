

## 如何操作...

按照以下步骤实现示例：

1. 创建一个名为 `MyFormatter` 的类，继承 `java.util.logging.Formatter` 类。实现抽象的 `format()` 方法。它接收一个 `LogRecord` 对象作为参数并返回一个包含日志消息的 `String` 对象：

```
public class MyFormatter extends Formatter {
    @Override
    public String format(LogRecord record) {

        StringBuilder sb=new StringBuilder();
        sb.append("["+record.getLevel()+"] - ");
        sb.append(new Date(record.getMillis())+" : ");
        sb.append(record.getSourceClassName()+", "
            +record.getSourceMethodName()+" : ");
        sb.append(record.getMessage()+"\n");
        return sb.toString();
    }
}
```

2. 创建一个名为 `MyLoggerFactory` 的类：

```
public class MyLoggerFactory {
```

3. 声明一个私有静态 `Handler` 属性名为 `handler`：

```
private static Handler handler;
```

4. 实现公共静态方法 `getLogger()` 来创建用于写入日志消息的 `Logger` 对象。它接收一个名为 `name` 的 `String` 参数。我们使用 `synchronized` 关键字同步此方法：

```
public synchronized static Logger getLogger(String name){
```

5. 使用 `Logger` 类的 `getLogger()` 方法获取与作为参数接收的 `name` 关联的 `java.util.logging.Logger`：

```
Logger logger=Logger.getLogger(name);
```

6. 使用 `setLevel()` 方法设置日志级别以写入所有日志消息：

```
logger.setLevel(Level.ALL);
```

7. 如果 `handler` 属性值为 `null`，则创建一个新的 `FileHandler` 对象以将日志消息写入 `recipe6.log` 文件。为此处理器分配一个 `MyFormatter` 对象；使用 `setFormatter()` 方法将其指定为格式化程序：

```
try {
    if (handler==null) {
        handler=new FileHandler("recipe6.log");
        Formatter format=new MyFormatter();
        handler.setFormatter(format);
    }
```

8. 如果 `Logger` 对象没有关联的处理器，则使用 `addHandler()` 方法分配处理器：

```
if (logger.getHandlers().length==0) {
    logger.addHandler(handler);
}
} catch (SecurityException | IOException e) {
    e.printStackTrace();
}
```

9. 返回创建的 `Logger` 对象：

```
return logger;
}
```

10. 创建一个名为 `Task` 的类，实现 `Runnable` 接口。它将是用于测试你的 `Logger` 对象的任务：

```
public class Task implements Runnable {
```

11. 实现 `run()` 方法：

```
@Override
public void run() {
```

12. 首先，声明一个名为 `logger` 的 `Logger` 对象。使用 `MyLoggerFactory` 类的 `getLogger()` 方法初始化它，并将此类名作为参数传递：

```
Logger logger= MyLoggerFactory.getLogger(this.getClass().getName());
```

13. 使用 `entering()` 方法写入指示方法执行开始的日志消息：

```
logger.entering(Thread.currentThread().getName(), "run()");
```

14. 让线程休眠两秒：

```
try {
    TimeUnit.SECONDS.sleep(2);
} catch (InterruptedException e) {
    e.printStackTrace();
}
```

15. 使用 `exiting()` 方法写入指示方法执行结束的日志消息：

```
logger.exiting(Thread.currentThread().getName(), "run()",
        Thread.currentThread());
}
```

16. 通过创建名为 `Main` 的类并包含 `main()` 方法来实现示例的主类：

```
public class Main {
    public static void main(String[] args) {
```

17. 声明一个名为 `logger` 的 `Logger` 对象。使用 `MyLoggerFactory` 类的 `getLogger()` 方法初始化它，并将类名作为参数传递：

```
Logger logger=MyLoggerFactory.getLogger(Main.class.getName());
```

18. 使用 `entering()` 方法写入指示主程序执行开始的日志消息：

```
logger.entering(Main.class.getName(), "main()",args);
```

19. 创建一个 `Thread` 数组来存储五个线程：

```
Thread threads[]=new Thread[5];
```

20. 创建五个 `Task` 对象和五个线程来执行它们。写入日志消息以指示你将启动一个新线程以及你已创建该线程：

```
for (int i=0; i<threads.length; i++) {
    logger.log(Level.INFO,"Launching thread: "+i);
    Task task=new Task();
    threads[i]=new Thread(task);
    logger.log(Level.INFO,"Thread created: "+
                threads[i].getName());
    threads[i].start();
}
```

21. 写入日志消息以指示你已创建线程：

```
logger.log(Level.INFO,"Ten Threads created."+
    "Waiting for its finalization");
```

22. 使用 `join()` 方法等待五个线程的结束。每个线程结束后，写入日志消息指示线程已完成：

```
for (int i=0; i<threads.length; i++) {
    try {
        threads[i].join();
        logger.log(Level.INFO,"Thread has finished its execution",
                    threads[i]);
    } catch (InterruptedException e) {
        logger.log(Level.SEVERE, "Exception", e);
    }
}
```

23. 使用 `exiting()` 方法写入指示主程序执行结束的日志消息：

```
logger.exiting(Main.class.getName(), "main()");
}
```

## 其工作原理...

在本教程中，你使用了 Java 日志 API 提供的 `Logger` 类在并发应用程序中写入日志消息。首先，你实现了 `MyFormatter` 类为日志消息分配格式。该类扩展了 `Formatter` 类，后者声明了抽象的 `format()` 方法。此方法接收包含日志消息所有信息的 `LogRecord` 对象并返回格式化的日志消息。在你的类中，你使用了 `LogRecord` 类的以下方法来获取有关日志消息的信息：

- `getLevel()`: 返回消息的级别
- `getMillis()`: 返回消息发送到 `Logger` 对象的时间
- `getSourceClassName()`: 返回向 `Logger` 发送消息的类的名称
- `getSourceMethodName()`: 返回向 `Logger` 发送消息的方法的名称
- `getMessage()`: 返回日志消息

`MyLoggerFactory` 类实现了静态方法 `getLogger()`。此方法创建 `Logger` 对象并分配 `Handler` 对象以使用 `MyFormatter` 格式化程序将应用程序的日志消息写入 `recipe6.log` 文件。你使用 `Logger` 类的静态方法 `getLogger()` 创建 `Logger` 对象。此方法根据作为参数传递的名称返回不同的对象。你只创建了一个 `Handler` 对象，因此所有 `Logger` 对象都将把日志消息写入同一个文件。你还配置了记录器以写入所有日志消息，而不管其级别。

最后，你实现了一个 `Task` 对象和一个写入日志文件中的不同日志消息的主程序。你使用了以下方法：

- `entering()`: 写入 `FINER` 级别的消息，指示方法已开始执行
- `exiting()`: 写入 `FINER` 级别的消息，指示方法已结束执行
- `log()`: 写入指定级别的消息

## 还有更多...

当你使用日志系统时，必须考虑两个重要点：

- **写入必要的信息**：如果你写入的信息太少，记录器将无用，因为它无法实现其目的。如果你写入大量信息，你将生成庞大且难以管理的日志文件；这将使获取必要信息变得困难。
- **为消息使用适当的级别**：如果你为高级别信息消息或低级别错误消息写入内容，将会混淆查看日志文件的用户。这将使在错误情况下了解发生了什么变得更加困难；或者你将拥有太多信息，使其难以知道错误的主要原因。

还有其他库提供比 `java.util.logging` 包更完整的日志系统，例如 `Log4j` 或 `slf4j` 库。但 `java.util.logging` 包是 Java API 的一部分，其所有方法都是多线程安全的；因此，我们可以在并发应用程序中使用它而不会出现问题。

## 另请参阅

- `Chapter 7`，*Concurrent Collections* 中的 *Using non-blocking thread-safe deques*、*Using blocking thread-safe deques*、*Using blocking thread-safe queues ordered by priority*、*Using thread-safe lists with delayed elements* 和 *Using thread-safe navigable maps* 教程

## 使用 FindBugs 分析并发代码

**静态代码分析工具** 是一组在查找潜在错误的同时分析应用程序源代码的工具。这些工具，例如 Checkstyle、PMD 或 FindBugs，具有一组预定义的好实践规则，并解析源代码以查找违反这些规则的情况。目标是在生产中执行之前尽早发现错误或导致性能不佳的地方。编程语言通常提供此类工具，Java 也不例外。其中一个帮助分析 Java 代码的工具是 FindBugs。它是一个开源工具，包含一系列用于分析 Java 并发代码的规则。
