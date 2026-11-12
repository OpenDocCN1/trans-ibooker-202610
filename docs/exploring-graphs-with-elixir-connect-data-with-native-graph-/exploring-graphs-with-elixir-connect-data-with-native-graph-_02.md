## Working with Resources

编程时我们大量使用资源——访问文件、与远程服务通信、使用数据库连接等等。这通常涉及及时释放资源、用于同步的锁定以及处理可能出现的异常等问题。同时处理所有这些问题会令人望而生畏。在本章中，我们将看到如何使用 lambda 表达式来构建代码，以减轻管理资源访问的一般痛苦——也就是说，处理那些我们最好不要搞错的琐碎任务。

我们可能被导致相信 JVM 自动化了所有垃圾回收（GC）。确实，如果我们只使用内部资源，可以让 JVM 来处理。但是，如果我们使用外部资源，例如连接数据库、打开文件和套接字或使用本机资源，那么 GC 就是我们的责任了。

Java 提供了一些正确清理资源的选项，但正如我们将在本章中看到的，没有哪一种比使用 lambda 表达式所能做到的更有效。我们将使用 lambda 表达式来实现 *执行围绕方法*（EAM）模式，该模式让我们对操作的排序有更好的控制。[11] 通过使用这个模式，正如我们将看到的，我们将管理资源生命周期的负担从一段代码的使用者转移到了对这些细节有更好的了解和控制的开发者身上。

然后我们将把管理资源的想法进一步推广，以简化围绕资源使用的更多操作。我们将看到如何以安全的方式管理至关重要且容易出错的锁管理任务。最后，我们将看看这些想法如何也能帮助我们以简洁优雅的方式编写异常测试。

### Cleaning Up Resources

处理 GC 可能是一件麻烦事。一家公司请我帮忙调试一个问题——一位程序员将问题描述为“它大多数时候都能正常工作”。应用程序在高峰使用期间失败。结果发现，代码依赖 finalize 方法来释放数据库连接。JVM 认为内存足够，选择不运行 GC。由于终结器很少被调用，这导致外部资源堵塞并随之引发失败。

我们需要以更好的方式来管理此类情况，lambda 表达式可以提供帮助。让我们从一个涉及 GC 的示例问题开始。我们将使用几种不同的方法构建示例，讨论每种方法的优缺点。这将帮助我们看到使用 lambda 表达式的最终解决方案的优势。

### Peeking into the Problem

我们关注外部资源清理，因此让我们从一个使用 FileWriter 写入一些消息的简单示例类开始。

```
resources/fpij/FileWriterExample.java

public class FileWriterExample {
private final FileWriter writer;

public FileWriterExample(final String fileName) throws IOException {
writer = new FileWriter(fileName);
}

public void writeStuff(final String message) throws IOException {
writer.write(message);
}

public void finalize() throws IOException { //Deprecated in Java 9
writer.close();
}
```

在 FileWriterExample 类的构造函数中，我们初始化一个 FileWriter 实例，并为其提供要写入的文件名。在 writeStuff 方法中，我们使用创建的 FileWriter 实例将给定消息写入文件。然后，在 finalize 方法中，我们清理资源，调用 close 方法，希望它能将内容刷新到文件并关闭它。

乍一看，这段代码似乎合理。毕竟，许多 Java 应用程序中编写的类使用 finalize 方法来清理资源，这是 Java 8 之前的标准做法，大量遗留代码仍然使用该函数。实际上，指望资源被自动清理相当于痴心妄想。

如果 JVM 发现有足够的内存可用，那么 GC 就不会被调用，因此 finalize 方法很长一段时间内不会被调用。这会导致资源无法及时释放，并可能导致资源竞争问题。这是 finalize 方法在 Java 9 中被弃用的原因之一，目的是鼓励程序员不再使用该方法。我们稍后将探讨 finalize 方法的替代方案，但首先，让我们编写一个 main 方法来使用 FileWriterExample 类。

```
resources/fpij/FileWriterExample.java

public static void main(final String[] args) throws IOException {
    final FileWriterExample writerExample =
        new FileWriterExample("peekaboo.txt");

    writerExample.writeStuff("peek-a-boo");
}
```





我们创建了 FileWriterExample 类的一个实例，并在其上调用了 writeStuff 方法，但如果运行这段代码，我们会看到 peekaboo.txt 文件被创建了，但它是空的。终结器从未运行；JVM 决定没必要运行，因为内存足够。结果，文件从未关闭，我们写入的内容也未从内存刷新。

如果在长期运行的进程中创建多个 FileWriterExample 类的实例，我们最终会得到多个打开的文件。由于 JVM 内存充足且认为没有运行垃圾回收（GC）的理由，这些文件中的许多不会及时关闭。

让我们通过添加对 close 的显式调用来修复这个问题，并摆脱 finalize 方法。

> **告别 finalize**
>
> finalize 方法在 Java 9 中已被弃用。花几分钟时间检查你自己的生产代码，看看 finalize 方法是否仍然存在于任何类中。如果发现，请将出现次数记录为技术债务，并安排时间使用本章学到的技术清理它们。

### 关闭资源

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

### 确保清理

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

### 使用 ARM

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

### 使用执行围绕模式清理资源

ARM 是朝着正确方向迈出的一步，但效果并不理想——谁让谁相信带有“管理”一词的东西呢？开个玩笑。任何使用我们类的人都必须弄清楚它实现了 AutoCloseable，并记得使用 try-with-resources 结构。如果我们设计的 API 能够指导程序员，并在编译器的帮助下指引他们正确方向，那将非常棒。我们可以通过 lambda 表达式和执行围绕（EAM）模式轻松实现这一点。

EAM 是一种强大的模式，利用 lambda 表达式来包装一段代码。正如名称所暗示的，我们可以设计在代码执行前后分别执行前置操作和后置操作。因此，我们设计的用户可以专注于业务逻辑，并将资源创建和释放的管理细节委托给代码的设计者。借助此模式，资源创建可以在 lambda 表达式执行之前完成，资源清理可以在 lambda 表达式执行之后自动运行。这听起来很有趣，我相信你很渴望看到它的实际效果。让我们重新处理手头的问题以使用 EAM。

#### 为资源清理准备类

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

#### 使用高阶函数

由于程序员无法直接创建 FileWriterEAM 的实例，我们需要为他们提供一个工厂方法。与创建实例后直接抛出的常规工厂方法不同，我们的方法会将其实例交给用户，并等待他们完成工作。我们将借助 lambda 表达式来实现这一点，如下文所示。让我们先编写这个方法。

resources/fpij/FileWriterEAM.java

```java
public static void use(final String fileName,
```



final UseInstance<FileWriterEAM, IOException> block) throws IOException {
final FileWriterEAM writerEAM = new FileWriterEAM(fileName);

try {
block.accept(writerEAM);
} finally {
writerEAM.close();
}
}
```

在 use 方法中，我们接收两个参数，fileName 和对接口 UseInstance（我们尚未定义）的引用。在此方法中我们实例化 FileWriterEAM，并在 try 和 finally 块的安全港中将实例传递给我们即将创建的接口的 accept 方法。当调用返回时，我们在 finally 块中对实例调用 close 方法。而不是使用这种结构，我们可以在 use 方法中使用 ARM。无论如何，我们类的用户都不必担心这些细节。

use 方法体现了执行围绕方法模式的结构。这里的主要操作是在 accept 方法中使用该实例，但创建和清理操作很好地围绕了这个调用。

在我们能够运行这段代码之前，让我们先处理最后一块缺失的部分，即 UseInstance 接口。

resources/fpij/UseInstance.java

```java
@FunctionalInterface
public interface UseInstance<T, X extends Throwable> {
void accept(T instance) throws X;
}
```

UseInstance 是一个函数式接口，是 Java 编译器从 lambda 表达式或方法引用自动合成的理想候选者。我们使用 @FunctionalInterface 注解标记了该接口。这纯粹是可选的，但有助于更明确地表达我们的意图。无论我们是否使用这个接口，编译器都会在结构上自动识别函数式接口，正如我们在 *一点糖来让它变甜* 中讨论过的那样。

我们本可以使用 `java.util.function.Consumer` 接口，而不是定义我们自己的 `UseInstance`；但是，由于该方法可能会抛出异常，我们需要在接口中指明这一点。Lambda 表达式只能抛出作为正在合成的 *抽象* 方法签名一部分定义的已检查异常（参见第 10 章，*错误处理*）。我们创建 `UseInstance` 接口是为了让 *接受* 方法能够接受泛型类型的实例；在本示例中，我们将其绑定到具体的 `FileWriterEAM` 实例。我们还设计它，使该方法实现可能会抛出泛型异常 X——同样，在本示例中，它绑定到具体的 `IOException` 类。

### 使用该设计进行实例清理

作为该类的设计者，我们投入的精力不仅仅是实现 `AutoCloseable` 接口。我们这边的这点额外投入很快就会带来持续的回报：每次程序员使用我们的类时，都能立即完成资源清理，正如我们在这里所见：

resources/fpij/FileWriterEAM.java

```java
FileWriterEAM.use("eam.txt", writerEAM -> writerEAM.writeStuff("sweet"));
```

首先，我们类的用户不能直接创建实例。这阻止了他们编写会将资源清理推迟到过期时间之后的代码（除非他们采取极端措施，例如使用反射来破坏该机制）。由于编译器会阻止对构造函数或 close 方法的调用，程序员们会很快意识到 use 方法的好处，该方法为他们提供一个可用的实例。要调用 use，他们可以使用 lambda 表达式提供的简洁语法，就像我们在前面代码中看到的那样。

让我们运行此版本的代码，查看它创建的 eam.txt 文件。

sweet

我们也可以查看控制台，查看代码的输出。

close 已自动调用...

我们可以看到文件具有正确的输出，且资源清理已自动完成。

在示例中，我们在 lambda 表达式中仅对给定的实例 writerEAM 进行一次调用。如果需要对其执行更多操作，可以将其作为参数传递给其他函数。我们也可以直接在 lambda 表达式中使用多行语法对它进行一些操作。

resources/fpij/FileWriterEAM.java

```java
FileWriterEAM.use("eam2.txt", writerEAM -> {
    writerEAM.writeStuff("how");
    writerEAM.writeStuff("sweet");
});
```

我们可以通过将多行代码包装在 {} 块中，将其放在 lambda 表达式内。如果 lambda 表达式预期返回结果，请务必在适当的表达式处放置 return。Java 编译器给了我们灵活性，可以只写一行或包装多行，但我们应保持 lambda 表达式简短。

长方法不好，但长 lambda 表达式是邪恶的——我们会失去代码简洁、易于理解且易于维护的好处。与其编写长 lambda 表达式，不如将代码移至其他方法，如果可能，则对其使用方法引用，或从 lambda 表达式中调用它们。

在本示例中，UseInstance 的 accept 是一个 void 方法。如果我们有兴趣向 use 方法的调用者返回某些结果，我们就必须修改此方法的签名，以放置适当的返回类型，例如泛型参数 R。如果我们做出此更改，UseInstance 将更像 Function<U, R> 接口，而不是 Consumer<T> 接口。我们还必须更改 use 方法，以传播修改后的 apply 方法的返回结果。

我们使用 lambda 表达式实现了执行围绕方法模式。在设计需要及时清理资源的类时，我们可以从该模式中受益。与其将负担转嫁给我们类的用户，不如我们多花点精力，这使他们的生活变得轻松得多，也使我们代码的行为更加一致。

该模式不仅限于资源清理。就我而言，该模式在一个项目中得以体现，当时我的团队需要在事务范围内执行操作。与其在代码中到处创建和管理事务，不如将它们封装到一个名为 runWithinTransaction 的方法中。该方法的调用者会收到一个事务实例，当他们返回时，该方法会负责检查状态，并执行提交或回滚事务以及记录日志等操作。

我们使用 lambda 表达式和执行围绕方法模式来管理资源。接下来，我们将使用它们来管理锁。

### 管理锁

锁在并发 Java 应用程序中起着关键作用，因为它们对于确保多线程对共享可变变量的更改的正确性至关重要。在本节中，我们将使用 lambda 表达式来对锁进行更细粒度的控制，并为对临界区正确加锁进行单元测试敞开大门。

synchronized 是一个用于提供互斥的老关键字。同步代码块，例如 synchronized { ... }，是执行围绕方法模式的一种实现。该模式自 Java 1.0 以来就存在，但在 Java 中它被限制并绑定到 synchronized 关键字。lambda 表达式现在释放了该模式的潜力。

synchronized 有一些缺点——参见 Brian Goetz 编著的 Java Concurrency in Practice[Goe06] 和 Programming Concurrency on the JVM[Sub11]。首先，很难为 synchronized 调用设置超时，这会增加死锁和活锁的几率。其次，很难模拟 synchronized，这使得很难进行单元测试以查看代码是否符合适当的线程安全性。

为了解决这些问题，Java 5 引入了 Lock 接口，以及 ReentrantLock 等一些实现。Lock 接口使我们能够更好地控制加锁、解锁、检查锁是否可用，并在一定时间内未获取锁时轻松超时。由于这是一个接口，因此很容易为单元测试模拟其实现。[12]

Lock 接口有一个需要注意的地方——与它的对应物 synchronized 不同，它需要显式加锁和解锁。这意味着我们不仅要记住解锁，还要在 finally 块中执行解锁。从本章迄今的讨论中，我们可以看到 lambda 表达式和执行围绕方法模式在这里大有帮助。

让我们先看一段使用 Lock 的代码。

resources/fpij/Locking.java

```java
public class Locking {
Lock lock = new ReentrantLock(); //or mock
protected void setLock(final Lock mock) {
lock = mock;
}
public void doOp1() {
lock.lock();
try {
//...critical code...
} finally {
lock.unlock();
}
}
//...
}
```

我们使用 Lock lock 字段来在该类的方法之间共享锁。但是，加锁任务——例如在 doOp1 方法中——还有很大改进空间。它冗长、容易出错，且难以维护。让我们借助 lambda 表达式，创建一个小类来管理锁。

resources/fpij/Locker.java

```java
public class Locker {
public static void runUnlocked(Lock lock, Runnable block) {
```




lock.lock();
try {
  block.run();
} finally {
  lock.unlock();
}
}
}

这个类减轻了使用 Lock 接口的痛苦，因此代码的其余部分受益。我们可以在代码中使用 runLocked 方法来包装临界区。

resources/fpij/Locking.java

```
public void doOp2() {
  runLocked(lock, () -> {/...*critical code ...*/});
}

public void doOp3() {
  runLocked(lock, () -> {/...*critical code ...*/});
}

public void doOp4() {
  runLocked(lock, () -> {/...*critical code ...*/});
}
```

这些方法简洁明了，它们使用了我们创建的 Locker 辅助类的静态方法 runLocked（要使这段代码编译，我们需要 `import static Locker.runLocked`）。Lambda 表达式再次为我们提供了帮助。

我们看到，*执行包围方法* 模式如何帮助使代码简洁且不易出错，但优雅和简洁应该帮助消除样板代码，而不是掩盖本质。在使用 Lambda 表达式进行设计时，我们应该确保代码的意图及其后果清晰可见。另外，在创建捕获局部状态的 Lambda 表达式时，我们必须了解我们在“词法作用域有限制吗？”中讨论过的限制。

让我们再看看 *执行包围方法* 模式在 JUnit 单元测试中提供的另一个好处。

### 创建简洁的异常测试

引入 Java 5 注解时，JUnit[13] 很快就开始使用它们。总体而言，这是一项好处，但有一种特定用途，即异常测试的便利性，导致了简短而非简洁的代码。让我们先了解问题，然后使用——猜得没错——Lambda 表达式来解决它们。我们将看到，Lambda 表达式不仅仅是另一个语言特性；它们改变了我们思考、设计以及测试应用程序的方式。

假设我们正在通过单元测试驱动 RodCutter 类的设计，并且我们期望 maxProfit 方法在参数为零时抛出异常。让我们看看几种编写该异常测试的方法。

### 尝试 1：使用 try 和 catch 的冗长测试

下面是针对 maxProfit 方法的测试，使用 try 和 catch 来检查异常。

![](img/f9269002f2414fff96e5e6b4dc20839f_217_0.png)

```
@Test public void verboseExceptionTest() {
rodCutter.setPrices(prices);

try {
rodCutter.maxProfit(0);
fail("Expected exception for zero length");
} catch(RodCutterException ex) {
assertTrue(true);
}
}
```

这很冗长，理解起来可能需要一些努力，但这段代码明确指出了预期失败的内容：对 maxProfit 方法的调用。

### 尝试 2：使用注解的相当简洁的测试

让我们快速看看我们在 JUnit 4 中如何编写异常测试。它本意是——可以说是一次失败的尝试——为了使用注解来使测试不那么冗长。

resources/fpij/RodCutterTest.java

```
@Test(expected = RodCutterException.class) //JUnit 4 feature
public void TerseExceptionTest() {
    rodCutter.setPrices(prices);
    rodCutter.maxProfit(0);
}
```

测试很短，但具有欺骗性——它相当简洁，而不是简洁。它告诉我们，如果收到 RodCutterException 异常，测试应该通过，但它未能确保引发该异常的方法是 maxProfit。如果由于某些代码更改，setPrices 方法抛出了该异常，那么该测试将继续通过，但原因不对。一个好的测试应该只因为正确的原因而通过——这个测试欺骗了我们。幸运的是，我们不再需要编写这样的测试来享受简洁性。让我们看看如何在 JUnit 5 中使用 Lambda 来编写简洁的测试。

### 尝试 3：使用 Lambda 表达式的简洁测试

在测试中使用注解已成为过去式。JUnit 5 彻底重构了编写异常测试的方式，并大量使用 Lambda 表达式。正如你所期望的，这使测试既简洁又高度有效。

在 JUnit 5 中，你可以使用新的 assertThrows 方法来验证一段代码是否抛出预期的异常。该方法接受两个参数。第一个参数传达预期的异常类型。第二个参数是一个 Lambda 表达式，用于执行预期会抛出该异常的代码。如果从 Lambda 表达式调用的代码抛出了预期的异常，断言成功，否则报告失败。

让我们使用 assertThrows 方法来创建一个简洁的测试。

resources/fpij/RodCutterTest.java

```
@Test
public void ConciseExceptionTest() {
    rodCutter.setPrices(prices);

    Exception ex =
        assertThrows(RodCutterException.class, () -> rodCutter.maxProfit(0));

    assertEquals("length should be greater than zero", ex.getMessage());
}
```

这个测试既简洁又精细——只有当 maxProfit 方法抛出预期的异常时，它才会通过。前面的代码还表明，除了检查是否抛出了预期的异常之外，我们还可以选择检查异常是否包含预期的错误消息。

之前的所有测试都实现了相同的目标，但最后一个版本比其他版本更好，因为它既简洁又正确。

我们看到，Lambda 表达式帮助我们编写针对特定方法的预期异常测试，这有助于我们创建简洁、易于阅读且不易出错的测试。

### 总结

在本章中，我们管理了资源。我们不能完全依赖自动垃圾回收，尤其是当我们的应用程序使用外部资源时。*执行包围方法* 模式可以帮助我们对执行流程进行更细致的控制，并释放外部资源。Lambda 表达式非常适合用来实现这个模式。除了控制对象生命周期外，我们还可以使用此模式来更好地管理锁，并编写简洁的异常测试。这可以带来更确定的代码执行、及时清理重量级资源以及更少的错误。

在下一章中，我们将使用 Lambda 表达式来延迟代码某些部分的执行，以使程序更高效。

### 脚注

- [11] http://c2.com/cgi/wiki?ExecuteAroundMethod
- [12] https://www.agiledeveloper.com/presentations/TestDrivingMultiThreadedCode.zip