

我们创建了 FileWriterExample 类的一个实例，并在其上调用了 writeStuff 方法，但如果运行这段代码，我们会看到 peekaboo.txt 文件被创建了，但它是空的。终结器从未运行；JVM 决定没必要运行，因为内存足够。结果，文件从未关闭，我们写入的内容也未从内存刷新。

如果在长期运行的进程中创建多个 FileWriterExample 类的实例，我们最终会得到多个打开的文件。由于 JVM 内存充足且认为没有运行垃圾回收（GC）的理由，这些文件中的许多不会及时关闭。

让我们通过添加对 close 的显式调用来修复这个问题，并摆脱 finalize 方法。

> **告别 finalize**
>
> finalize 方法在 Java 9 中已被弃用。花几分钟时间检查你自己的生产代码，看看 finalize 方法是否仍然存在于任何类中。如果发现，请将出现次数记录为技术债务，并安排时间使用本章学到的技术清理它们。

## 关闭资源

尽管对象的内存清理仍然取决于 JVM 的 GC，但我们可以让自己相信，实例使用的外部资源可以通过显式调用快速清理。不幸的是，这会导致更多问题。为了看清这一点，让我们编写一个 close 方法。

resources/fpij/FileWriterExample.java

```java
public void close() throws IOException { // Not a good solution
    writer.close();
}
```

在 close 方法中，我们反过来调用 FileWriter 实例的 close 方法。如果在 FileWriterExample 中使用了任何其他外部资源，我们也可以在这里清理它们。让我们在 main 方法中显式使用此方法。

![](img/f9269002f2414fff96e5e6b4dc20839f_202_0.png)

```java
final FileWriterExample writerExample =
    new FileWriterExample("peekaboo.txt");
writerExample.writeStuff("peek-a-boo");
writerExample.close();
```

如果现在运行代码并查看 peekaboo.txt 文件，我们会看到 peek-a-boo 消息。代码可用，但远非完美。

对 close 的显式调用会在我们指示实例不再需要时立即清理其使用的任何外部资源。但是，如果在导致该调用的代码中出现异常，我们可能无法到达对 close 方法的调用。我们需要做更多工作来确保 close 被调用。让我们接下来处理这个问题。

## 确保清理

我们需要确保无论是否有异常，close 的调用都会发生。为了实现这一点，我们可以将调用包装在 finally 块中。

![](img/f9269002f2414fff96e5e6b4dc20839f_202_1.png)

```java
final FileWriterExample writerExample =
    new FileWriterExample("peekaboo.txt");
try { // Rather verbose
    writerExample.writeStuff("peek-a-boo");
} finally {
    writerExample.close();
}
```

这个版本即使在代码中发生异常时也能确保资源清理，但这需要很多努力，代码冗余且有异味。Java 7 引入了一种功能来减少此类异味，我们将在下一节中看到。

## 使用 ARM

自动资源管理（ARM）是自 Java 7 起即可用的功能，用于在资源使用结束时自动释放资源。正确使用时，ARM 可以减少代码的冗余。与之前示例中同时使用 try 和 finally 块不同，我们可以使用 ARM 功能，配合带有附加资源的特殊形式的 try 块。使用此语法时，Java 编译器会负责在字节码中自动插入 finally 块和对 close 方法的调用。

让我们看看使用 ARM 时代码会是什么样子；我们将使用新的 FileWriterARM 类的一个实例。

resources/fpij/FileWriterARM.java

```java
try (final FileWriterARM writerARM = new FileWriterARM("peekaboo.txt")) {
    writerARM.writeStuff("peek-a-boo");
    System.out.println("done with the resource...");
}
```

实现 AutoCloseable 接口，该接口只有一个方法 close。

围绕 AutoCloseable 的规则在 Java 中经历了几次变化。首先，Stream 实现了 AutoCloseable，因此所有基于输入/输出（I/O）的流都可以与 try-with-resources 一起使用。AutoCloseable 的契约已从严格的“必须关闭资源”修改为较宽松的“可以关闭资源”。如果我们确定代码使用 I/O 资源，则应该使用 try-with-resources。

下面是之前代码中使用的 FileWriterARM 类。

resources/fpij/FileWriterARM.java

```java
public class FileWriterARM implements AutoCloseable {
    private final FileWriter writer;

    public FileWriterARM(final String fileName) throws IOException {
        writer = new FileWriter(fileName);
    }

    public void writeStuff(final String message) throws IOException {
        writer.write(message);
    }

    public void close() throws IOException {
        System.out.println("close called automatically...");
        writer.close();
    }

    //...
}
```

让我们运行代码并查看 peekaboo.txt 文件和控制台中的代码输出。

已处理完资源...
已自动调用 close...

我们可以看到，一旦离开 try 块，close 方法就被调用了。在进入 try 块时创建的实例在离开块之后不可访问。该实例使用的内存最终将根据 JVM 采用的 GC 策略进行垃圾回收。

之前使用 ARM 的代码简洁迷人，但程序员必须记住使用它。如果我们忽略这个优雅的结构，代码也不会报错；它只是会在 try 块之外创建实例并调用 writeStuff 之类的方法。如果我们希望确保及时清理并避免程序员错误，我们需要超越 ARM，我们将在下一节中探讨。

## 使用执行围绕模式清理资源

ARM 是朝着正确方向迈出的一步，但效果并不理想——谁让谁相信带有“管理”一词的东西呢？开个玩笑。任何使用我们类的人都必须弄清楚它实现了 AutoCloseable，并记得使用 try-with-resources 结构。如果我们设计的 API 能够指导程序员，并在编译器的帮助下指引他们正确方向，那将非常棒。我们可以通过 lambda 表达式和执行围绕（EAM）模式轻松实现这一点。

EAM 是一种强大的模式，利用 lambda 表达式来包装一段代码。正如名称所暗示的，我们可以设计在代码执行前后分别执行前置操作和后置操作。因此，我们设计的用户可以专注于业务逻辑，并将资源创建和释放的管理细节委托给代码的设计者。借助此模式，资源创建可以在 lambda 表达式执行之前完成，资源清理可以在 lambda 表达式执行之后自动运行。这听起来很有趣，我相信你很渴望看到它的实际效果。让我们重新处理手头的问题以使用 EAM。

### 为资源清理准备类

我们将设计一个名为 FileWriterEAM 的类，封装需要及时清理的重型资源。在此示例中，我们将使用 FileWriter 来代表该资源。让我们将构造函数和 close 方法都设为私有——这会吸引试图使用该类的程序员的注意。他们不能直接创建实例，也不能在其中调用 close。在进一步讨论之前，让我们先实现迄今设计的方案。

resources/fpij/FileWriterEAM.java

```java
public class FileWriterEAM {
    private final FileWriter writer;

    private FileWriterEAM(final String fileName) throws IOException {
        writer = new FileWriter(fileName);
    }

    private void close() throws IOException {
        System.out.println("close called automatically...");
        writer.close();
    }

    public void writeStuff(final String message) throws IOException {
        writer.write(message);
    }
    //...
}
```

私有构造函数和私有 close 方法已就位，还有公共方法 writeStuff。

### 使用高阶函数

由于程序员无法直接创建 FileWriterEAM 的实例，我们需要为他们提供一个工厂方法。与创建实例后直接抛出的常规工厂方法不同，我们的方法会将其实例交给用户，并等待他们完成工作。我们将借助 lambda 表达式来实现这一点，如下文所示。让我们先编写这个方法。

resources/fpij/FileWriterEAM.java

```java
public static void use(final String fileName,
```
