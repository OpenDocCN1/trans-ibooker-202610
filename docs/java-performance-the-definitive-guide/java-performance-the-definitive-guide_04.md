## Working with the JIT Compiler

*just-in-time (JIT)* 编译器是 Java 虚拟机的核心；没有什么比 JIT 编译器更能控制应用程序的性能。

本章深入介绍编译器。 它首先介绍编译器的工作原理，并讨论使用 JIT 编译器的优点和缺点。 直到 JDK 8 出现之前，你必须在两个 Java 编译器之间进行选择。 今天，这两个编译器仍然存在，但协同工作，尽管在极少数情况下选择其中一个是必要的。 最后，我们将查看编译器的一些中级和高级调优。 如果应用程序运行缓慢而没有明显原因，这些部分可以帮助你确定编译器是否出了问题。

## Just-in-Time Compilers: An Overview

我们将从一些入门材料开始；如果你了解即时编译的基础知识，可以随意跳过。

计算机——更具体地说 CPU——只能执行相对较少、特定的指令，这些指令称为 *machine code*。 因此，CPU 执行的所有程序都必须被翻译成这些指令。

像 C++ 和 Fortran 这样的语言被称为 *compiled languages*，因为它们的程序以二进制（编译后）代码交付：程序被编写，然后静态编译器生成二进制文件。 该二进制文件中的汇编代码针对特定 CPU。 兼容的 CPU 可以执行相同的二进制文件：例如，AMD 和 Intel CPU 共享一组基本、通用的汇编语言指令，并且 CPU 的较新版本几乎总是可以执行该 CPU 早期版本相同的指令集。 反之则不总是成立；CPU 的新版本通常会引入在旧版本 CPU 上无法运行的指令。





另一方面，像 PHP 和 Perl 这样的语言是解释执行的。只要机器上安装了正确的解释器（即名为 php 或 perl 的程序），相同的程序源代码就可以在任何 CPU 上运行。解释器会在该行被执行时将程序的每一行翻译成二进制代码。

每种系统都有其优缺点。用解释型语言编写的程序具有可移植性：你可以将相同的代码放到任何安装了相应解释器的机器上，它就能运行。然而，它可能会运行缓慢。以一个简单的例子来看循环会发生什么：解释器在循环中执行代码时，会重新翻译代码的每一行。而编译后的代码不需要重复进行这种翻译。

一个好的编译器在生成二进制文件时会考虑多个因素。一个简单的例子是二进制语句的顺序：并非所有的汇编语言指令执行所需的时间都相同。加法指令可能在一个周期内执行，但从主内存中检索加法所需的值可能需要多个周期。

因此，一个好的编译器会生成一个先执行加载数据的语句、执行其他指令，然后在数据可用时再执行加法的二进制文件。而只一次看一行代码的解释器没有足够的信息来生成这种代码；它会从内存中请求数据，等待数据可用，然后再执行加法。顺便说一下，糟糕的编译器也会做同样的事情，而且并非总是可以保证即使是最好的编译器也能避免偶尔等待指令完成的情况。

由于这些（和其他）原因，解释型代码几乎总是明显比编译型代码慢：编译器拥有足够的信息来对二进制代码进行优化，而解释器根本无法执行这些优化。

解释型代码的优势在于可移植性。为 ARM CPU 编译的二进制文件显然无法在 Intel CPU 上运行。但使用英特尔 Sandy Bridge 处理器最新 AVX 指令的二进制文件也无法在旧的英特尔处理器上运行。因此，商业软件通常编译为处理器的一个相当旧的版本，并且不会利用其可用的最新指令。围绕这一点存在各种技巧，包括随二进制文件一起分发多个共享库来执行性能敏感的代码，并为各种 CPU 架构提供版本。

Java 试图在此找到一个中间地带。Java 应用程序是被编译的——但不是编译成特定 CPU 的特定二进制文件，而是编译成一种中间的低级语言。这种语言（称为 Java 字节码）随后由 java 二进制文件运行（与解释型 PHP 脚本由 php 二进制文件运行的方式相同）。这使 Java 具有了解释型语言的平台独立性。由于它执行的是理想化的二进制代码，java 程序能够在代码执行时将其编译为平台二进制代码。这种编译是在程序执行时发生的：它发生在“即时”。

这种编译仍然受平台依赖性的影响。例如，JDK 8 无法为英特尔 Skylake 处理器的最新指令集生成代码，而 JDK 11 可以。我将在第 105 页的“高级编译器标志”中对此进行更多说明。

Java 虚拟机在执行时编译此代码的方式是本章的重点。

## HotSpot 编译

如第 1 章所述，本书讨论的 Java 实现是 Oracle 的 HotSpot JVM。 这个名称（HotSpot）源于其编译代码的方法。在典型的程序中，只有很小一部分代码会被频繁执行，而应用程序的性能主要取决于这些代码段的执行速度。这些关键部分被称为应用程序的热点；代码段被执行的次数越多，就越被认为“热”。

因此，当 JVM 执行代码时，它并不会立即开始编译代码。这有两个基本原因。首先，如果代码只会被执行一次，那么编译它基本上是浪费精力；解释 Java 字节码比编译它们并仅执行一次编译后的代码要快。

但如果所讨论的代码是一个被频繁调用的方法或一个运行多次迭代的循环，那么编译它是有价值的：编译代码所花费的周期将被执行更快编译代码所带来的多次执行节省所抵消。这种权衡是编译器先执行解释代码的一个原因——编译器可以判断哪些方法被调用的频率足以证明其被编译。

第二个原因是优化：JVM 执行特定方法或循环的次数越多，它就越了解该代码。这使得 JVM 在编译代码时能够进行大量优化。

这些优化（以及影响它们的方法）将在本章后面讨论，但举一个简单的例子，考虑 equals() 方法。此方法存在于每个 Java 对象中（因为它继承自 Object 类）并且经常被重写。当解释器遇到语句 b = obj1.equals(obj2) 时，它必须查找 obj1 的类型（类），以便知道要执行哪个 equals() 方法。这种动态查找可能需要花费一些时间。

随着时间的推移，比如 JVM 注意到每次执行此语句时，obj1 都是 java.lang.String 类型。然后 JVM 可以生成直接调用 String.equals() 方法的编译代码。现在代码不仅因为被编译而更快，而且因为它可以跳过查找要调用哪个方法的步骤。

事情并没有那么简单；下一次执行代码时，obj1 可能会引用 String 以外的对象。JVM 会创建处理这种可能性的编译代码，这将涉及对相关代码进行去优化然后重新优化（你将在第 101 页的“去优化”中看到一个例子）。尽管如此，只要 obj1 继续引用 String，整体编译代码就会更快，因为它跳过了查找要执行哪个方法的步骤。这种优化只能在运行代码一段时间并观察其行为后才能进行：这是 JIT 编译器等待编译代码段的第二个原因。

## 寄存器和主内存

编译器可以做出的最重要的优化之一涉及何时使用主内存中的值以及何时将值存储在寄存器中。考虑以下代码：

```
public class RegisterTest {
    private int sum;

    public void calculateSum(int n) {
        for (int i = 0; i < n; i++) {
            sum += i;
        }
    }
}
```

在某个时刻，sum 实例变量必须驻留在主内存中，但从主内存中检索值是一项昂贵的操作，需要多个周期才能完成。如果在该循环的每次迭代中都从主内存中检索 sum 的值（并将其写回主内存），性能将非常糟糕。相反，编译器将使用 sum 的初始值加载一个寄存器，使用寄存器中的该值执行循环，然后（在不确定的时间点）将最终结果从寄存器写回主内存。

这种优化非常有效，但这意味着线程同步的语义（参见第 9 章）对应用程序的行为至关重要。一个线程无法看到另一个线程使用的寄存器中存储的变量值；同步使得能够确切知道寄存器何时存储到主内存并可供其他线程使用。

寄存器使用是编译器的通用优化，通常 JIT 会积极使用寄存器。我们将在第 110 页的“逃逸分析”中更深入地讨论这个问题。





![](img/1711f142ae3cfd740ef106afbbba2046_108_0.png)

## 快速总结

- Java 设计为利用脚本语言的平台独立性和编译语言的原生性能。
- Java 类文件被编译为中间语言（Java 字节码），然后由 JVM 进一步编译为汇编语言。
- 将字节码编译为汇编语言会执行优化，从而大幅提升性能。

## 分层编译

从前，JIT 编译器有两种风格，你需要根据想要使用的编译器安装不同版本的 JDK。这些编译器被称为客户端编译器和服务端编译器。在 1996 年，这是一个重要的区别；在 2020 年，则没那么重要了。如今，所有发布的 JVM 都包含这两种编译器（尽管在常见用法中，它们通常被称为服务端 JVM）。

> **编译器标志**
> 
> 在旧版本的 Java 中，你会通过一个不符合 JVM 标志常规约定的标志来指定想要使用的编译器：你会使用 `-client` 用于客户端编译器，使用 `-server` 或 `-d64` 用于服务端编译器。
> 
> 由于开发者不会不必要地更改脚本，你很可能会遇到指定 `-client` 或 `-server` 的脚本和其他命令行。但请记住，自 JDK 8 起，这些标志不再起作用。对于许多更早的 JDK 版本也是如此：如果为仅支持服务端编译器的 JVM 指定 `-client`，你仍然会得到服务端编译器。
> 
> 另一方面，请注意旧的 `-d64` 参数（本质上是 `-server` 的别名）已从 JDK 11 中移除，并将导致错误。在 JDK 8 上使用该参数是无操作。

尽管被称为服务端 JVM，客户端和服务端编译器之间的区别仍然存在；两种编译器都可供 JVM 使用并被其使用。因此，了解这种差异对于理解编译器的工作方式很重要。

历史上，JVM 开发者（甚至一些工具）有时会用 C1（编译器 1，客户端编译器）和 C2（编译器 2，服务端编译器）这些名称来指代编译器。现在这些名称更合适，因为客户端和服务端计算机之间的区别早已消失，所以我们将贯穿全文采用这些名称。

两种编译器的主要区别在于它们编译代码的积极程度。C1 编译器比 C2 编译器更早开始编译代码。这意味着在代码执行的开始阶段，C1 编译器会更快，因为它编译的代码比 C2 编译器多。

这里的工程权衡是 C2 编译器在等待时获得的知识：这种知识使 C2 编译器能够对编译后的代码进行更好的优化。最终，C2 编译器生成的代码比 C1 编译器生成的代码更快。从用户的角度来看，这种权衡的好处取决于程序运行的时间长短以及程序启动时间的重要性。

当这些编译器是分离的时候，一个显而易见的问题是为什么需要有选择：JVM 不能先使用 C1 编译器，然后随着代码变热再使用 C2 编译器吗？这种技术被称为分层编译，并且是所有 JVM 现在使用的技术。它可以用 `-XX:-TieredCompilation` 标志显式禁用（其默认值是 true）；在第 105 页的“高级编译器标志”中，我们将讨论这样做的后果。

## 常见编译器标志

有两个常用的标志会影响 JIT 编译器；我们将在本节中介绍它们。

## 调优代码缓存

当 JVM 编译代码时，它将汇编语言指令的集合保存在代码缓存中。代码缓存有一个固定大小，一旦填满，JVM 就无法再编译任何额外代码。

如果代码缓存太小，很容易看出其中的潜在问题。一些热方法会被编译，但其他方法不会：应用程序最终会运行大量（非常慢的）解释代码。

当代码缓存填满时，JVM 会输出以下警告：

```
Java HotSpot(TM) 64-Bit Server VM warning: CodeCache is full.
    Compiler has been disabled.
Java HotSpot(TM) 64-Bit Server VM warning: Try increasing the
    code cache size using -XX:ReservedCodeCacheSize=
```

有时很容易错过这条消息；判断编译器是否已停止编译代码的另一种方法是跟踪本节后面讨论的编译日志的输出。

实际上并没有什么好的机制可以确定特定应用程序需要多少代码缓存。因此，当你需要增加代码缓存大小时，这有点像碰运气操作；一个典型选项是简单地将默认值加倍或四倍。

代码缓存的最大大小通过 `-XX:ReservedCodeCacheSize=N` 标志设置（其中 *N* 是针对特定编译器提到的默认值）。代码缓存像 JVM 中的大多数内存一样管理：有一个初始大小（由 `-XX:InitialCodeCacheSize=N` 指定）。代码缓存大小的分配从初始大小开始，并在缓存填满时增加。代码缓存的初始大小为 2,496 KB，默认最大大小为 240 MB。缓存的调整在后台进行，实际上不会影响性能，因此设置 `ReservedCodeCacheSize` 大小（即设置最大代码缓存大小）通常已足够。

指定一个非常大的最大代码缓存大小值以使其永不耗尽空间是否有缺点？这取决于目标机器上可用的资源。如果指定了 1 GB 的代码缓存大小，JVM 将保留 1 GB 的本地内存。该内存不是在需要时才分配，但它仍然被保留，这意味着你的机器上必须有足够的虚拟内存来满足保留。

> **保留内存与分配内存**
> 
> 必须理解 JVM *保留* 内存和它分配内存之间的区别。这种差异适用于代码缓存、Java 堆以及 JVM 的各种其他本地内存结构。
> 
> 有关此主题的详细信息，请参阅第 249 页的“Footprint”。

此外，如果你仍在使用旧的 Windows 机器上的 32 位 JVM，整个进程大小不能超过 4 GB。这包括 Java 堆、JVM 本身所有代码的空间（包括其本地库和线程栈）、应用程序分配的任何本地内存（直接或通过新 I/O [NIO] 库），当然还有代码缓存。

这些就是代码缓存不是无界的，并且有时需要为大型应用程序进行调优的原因。在内存充足的 64 位机器上，将值设置得过高不太可能对应用程序产生实际影响：应用程序不会耗尽进程空间内存，并且额外的内存保留通常会被操作系统接受。

如果启用了 PrintCompilation，每次方法（或循环）被编译时，JVM 都会打印一行关于刚刚编译了什么的信息。

大多数编译日志行的格式如下：

```
timestamp compilation_id attributes (tiered_level) method_name size deopt
```

这里的时间戳是编译完成后的时间（相对于 0，即 JVM 启动时）。

compilation_id 是一个内部任务 ID。通常，这个数字会单调递增，但有时你可能会看到编译 ID 乱序的情况。这最常发生在有多个编译线程时，表示编译线程相对于彼此运行得更快或更慢。但不要因此得出某个特定编译任务异常缓慢的结论：这通常只是线程调度的结果。

attributes 字段是一系列五个字符，表示正在编译的代码的状态。如果某个特定属性适用于给定的编译，则打印以下列表中显示的字符；否则，为该属性打印一个空格。因此，五字符属性字符串可能显示为两个或多个项目，中间用空格分隔。各种属性如下：

- %
  编译是 OSR。
- s
  方法是同步的。
- !
  方法有异常处理程序。
- b
  编译以阻塞模式发生。
- n
  编译是为了对本地方法的包装器。

这些属性中的第一个指的是栈上替换（OSR）。JIT 编译是一个异步过程：当 JVM 决定某个方法应该被编译时，该方法会被放入队列。JVM 然后继续解释该方法，而不会等待编译，下一次调用该方法时，JVM 将执行该方法的编译版本（当然，前提是编译已完成）。

但考虑一个长时间运行的循环。JVM 会注意到循环本身应该被编译，并将该代码排队进行编译。但这还不够：JVM 必须有能力在



loop is still running—it would be inefficient to wait until the loop and enclosing method exit (which may not even happen). Hence, when the code for the loop has finished compiling, the JVM replaces the code (on stack), and the next iteration of the loop will execute the much faster compiled version of the code. This is OSR.

The next two attributes should be self-explanatory. The blocking flag will never be printed by default in current versions of Java; it indicates that compilation did not occur in the background (see “Compilation Threads” on page 107 for more details). Finally, the native attribute indicates that the JVM generated compiled code to facilitate the call into a native method.

If tiered compilation has been disabled, the next field (tiered_level) will be blank. Otherwise, it will be a number indicating which tier has completed compilation.

Next comes the name of the method being compiled (or the method containing the loop being compiled for OSR), which is printed as ClassName::method.

Next is the size (in bytes) of the code being compiled. This is the size of the Java bytecodes, not the size of the compiled code (so, unfortunately, this can’t be used to predict how large to size the code cache).

Finally, in some cases a message at the end of the compilation line will indicate that some sort of deoptimization has occurred; these are typically the phrase made not entrant or made zombie. See “Deoptimization” on page 101 for more details.

## Inspecting Compilation with jstat

Seeing the compilation log requires that the program be started with the -XX:+PrintCompilation flag. If the program was started without that flag, you can get limited visibility into the working of the compiler by using jstat.

jstat has two options to provide information about the compiler. The -compiler option supplies summary information about the number of methods compiled (here 5003 is the process ID of the program to be inspected):

```
% jstat -compiler 5003
Compiled Failed Invalid   Time   FailedType FailedMethod
     206      0       0   1.97            0
```

Note this also lists the number of methods that failed to compile and the name of the last method that failed to compile; if profiles or other information lead you to suspect that a method is slow because it hasn’t been compiled, this is an easy way to verify that hypothesis.

Alternatively, you can use the -printcompilation option to get information about the last method that is compiled. Because jstat takes an optional argument to repeat its

operation, you can see over time which methods are being compiled. In this example, jstat repeats the information for process ID 5003 every second (1,000 ms):

```
% jstat -printcompilation 5003 1000
Compiled  Size  Type  Method
207       64    1     java/lang/CharacterDataLatin1 toUpperCase
208       5     1     java/math/BigDecimal$StringBuilderHelper getCharArray
```

The compilation log may also include a line that looks like this:

```
timestamp compile_id COMPILE SKIPPED: reason
```

This line (with the literal text COMPILE SKIPPED) indicates that something has gone wrong with the compilation of the given method. In two cases this is expected, depending on the reason specified:

### Code cache filled

The size of the code cache needs to be increased using the ReservedCodeCache flag.

### Concurrent classloading

The class was modified as it was being compiled. The JVM will compile it again later; you should expect to see the method recompiled later in the log.

In all cases (except the cache being filled), the compilation should be reattempted. If it is not, an error prevents compilation of the code. This is often a bug in the compiler, but the usual remedy in all cases is to refactor the code into something simpler that the compiler can handle.

Here are a few lines of output from enabling PrintCompilation on the stock REST application:

```
28015  850     4   net.sdo.StockPrice::getClosingPrice (5 bytes)
28179  905  s   3   net.sdo.StockPriceHistoryImpl::process (248 bytes)
28226  25 %  3   net.sdo.StockPriceHistoryImpl::<init> @ 48 (156 bytes)
28244  935     3   net.sdo.MockStockPriceEntityManagerFactory\MockStockPriceEntityManager::find (507 bytes)
29929  939     3   net.sdo.StockPriceHistoryImpl::<init> (156 bytes)
106805 1568  !  4   net.sdo.StockServlet::processRequest (197 bytes)
```

This output includes only a few of the stock-related methods (and not necessarily all of the lines related to a particular method). A few interesting things to note: the first such method wasn’t compiled until 28 seconds after the server was started, and 849 methods were compiled before it. In this case, all those other methods were methods of the server or JDK (filtered out of this output). The server took about 2 seconds to start; the remaining 26 seconds before anything else was compiled were essentially idle as the application server waited for requests.

The remaining lines are included to point out interesting features. The `process()` method is synchronized, so the attributes include an `s`. Inner classes are compiled just like any other class and appear in the output with the usual Java nomenclature: `outer-classname$inner-classname`. The `processRequest()` method shows up with the exception handler as expected.

Finally, recall the implementation of the `StockPriceHistoryImpl` constructor, which contains a large loop:

```
public StockPriceHistoryImpl(String s, Date startDate, Date endDate) {
    EntityManager em = emf.createEntityManager();
    Date curDate = new Date(startDate.getTime());
    symbol = s;
    while (!curDate.after(endDate)) {
        StockPrice sp = em.find(StockPrice.class, new StockPricePK(s, curDate));
        if (sp != null) {
            if (firstDate == null) {
                firstDate = (Date) curDate.clone();
            }
            prices.put((Date) curDate.clone(), sp);
            lastDate = (Date) curDate.clone();
        }
        curDate.setTime(curDate.getTime() + msPerDay);
    }
}
```

The loop is executed more often than the constructor itself, so the loop is subject to OSR compilation. Note that it took a while for that method to be compiled; its compilation ID is 25, but it doesn’t appear until other methods in the 900 range are being compiled. (It’s easy to read OSR lines like this example as 25% and wonder about the other 75%, but remember that the number is the compilation ID, and the % just signifies OSR compilation.) That is typical of OSR compilation; the stack replacement is harder to set up, but other compilation can continue in the meantime.

## Tiered Compilation Levels

The compilation log for a program using tiered compilation prints the tier level at which each method is compiled. In the sample output, code was compiled either at level 3 or 4, even though we’ve discussed only two compilers (plus the interpreter) so far. It turns out that there are five levels of compilation, because the C1 compiler has three levels. So the levels of compilation are as follows:

- 0 — Interpreted code
- 1 — Simple C1 compiled code
- 2 Limited C1 compiled code
- 3 Full C1 compiled code
- 4 C2 compiled code

A typical compilation log shows that most methods are first compiled at level 3: full C1 compilation. (All methods start at level 0, of course, but that doesn’t appear in the log.) If a method runs often enough, it will get compiled at level 4 (and the level 3 code will be made not entrant). This is the most frequent path: the C1 compiler waits to compile something until it has information about how the code is used that it can leverage to perform optimizations.

If the C2 compiler queue is full, methods will be pulled from the C2 queue and compiled at level 2, which is the level at which the C1 compiler uses the invocation and back-edge counters (but doesn’t require profile feedback). That gets the method compiled more quickly; the method will later be compiled at level 3 after the C1 compiler has gathered profile information, and finally compiled at level 4 when the C2 compiler queue is less busy.

On the other hand, if the C1 compiler queue is full, a method that is scheduled for compilation at level 3 may become eligible for level 4 compilation while still waiting to be compiled at level 3. In that case, it is quickly compiled to level 2 and then transitioned to level 4.

Trivial methods may start in either level 2 or 3 but then go to level 1 because of their trivial nature. If the C2 compiler for some reason cannot compile the code, it will also go to level 1. And, of course, when code is deoptimized, it goes to level 0.

Flags control some of this behavior, but expecting results when tuning at this level is optimistic. The best case for performance happens when methods are compiled as expected: tier 0 → tier 3 → tier 4. If methods frequently get compiled into tier 2 and extra CPU cycles are available, consider increasing the number of compiler threads; that will reduce the size of the C2 compiler queue. If no extra CPU cycles are available, all you can do is attempt to reduce the size of the application.

## Deoptimization

The discussion of the output of the PrintCompilation flag mentioned two cases of the compiler deoptimizing the code. Deoptimization means that the compiler has to “undo” a previous compilation. The effect is that the performance of the application will be reduced—at least until the compiler can recompile the code in question.





Deoptimization occurs in two cases: when code is made not entrant and when code is made zombie.

## Not entrant code

Two things cause code to be made not entrant. One is due to the way classes and interfaces work, and one is an implementation detail of tiered compilation.

Let’s look at the first case. Recall that the stock application has an interface StockPriceHistory. In the sample code, this interface has two implementations: a basic one (StockPriceHistoryImpl) and one that adds logging (StockPriceHistory Logger) to each operation. In the REST code, the implementation used is based on the log parameter of the URL:

```
StockPriceHistory sph;
String log = request.getParameter("log");
if (log != null && log.equals("true")) {
    sph = new StockPriceHistoryLogger(...);
}
else {
    sph = new StockPriceHistoryImpl(...);
}
// Then the JSP makes calls to:
sph.getHighPrice();
sph.getStdDev();
// and so on
```

If a bunch of calls are made to http://localhost:8080/StockServlet (that is, without the log parameter), the compiler will see that the actual type of the sph object is StockPriceHistoryImpl. It will then inline code and perform other optimizations based on that knowledge.

Later, say a call is made to http://localhost:8080/StockServlet?log=true. Now the assumption the compiler made regarding the type of the sph object is incorrect; the previous optimizations are no longer valid. This generates a deoptimization trap, and the previous optimizations are discarded. If a lot of additional calls are made with logging enabled, the JVM will quickly end up compiling that code and making new optimizations.

The compilation log for that scenario will include lines such as the following:

```
841113  25 %           net.sdo.StockPriceHistoryImpl::<init> @ -2 (156 bytes)
                        made not entrant
841113  937  s          net.sdo.StockPriceHistoryImpl::process (248 bytes)
                        made not entrant
1322722  25 %           net.sdo.StockPriceHistoryImpl::<init> @ -2 (156 bytes)
                        made zombie
1322722  937  s          net.sdo.StockPriceHistoryImpl::process (248 bytes)
                        made zombie
```

Note that both the OSR-compiled constructor and the standard-compiled methods have been made not entrant, and some time much later, they are made zombie.

Deoptimization sounds like a bad thing, at least in terms of performance, but that isn’t necessarily the case. Table 4-1 shows the operations per second that the REST server achieves under deoptimization scenarios.

Table 4-1. Throughput of server with deoptimization

| Scenario | OPS |
|---|---|
| Standard implementation | 24.4 |
| Standard implementation after deopt | 24.4 |
| Logging implementation | 24.1 |
| Mixed impl | 24.3 |

The standard implementation will give us 24.4 OPS. Suppose that immediately after that test, a test is run that triggers the StockPriceHistoryLogger path—that is the scenario that ran to produce the deoptimization examples just listed. The full output of PrintCompilation shows that all the methods of the StockPriceHistoryImpl class get deoptimized when the requests for the logging implementation are started. But after deoptimization, if the path that uses the StockPriceHistoryImpl implementation is rerun, that code will get recompiled (with slightly different assumptions), and we will still end up still seeing about 24.4 OPS (after another warm-up period).

That’s the best case, of course. What happens if the calls are intermingled such that the compiler can never really assume which path the code will take? Because of the extra logging, the path that includes the logging gets about 24.1 OPS through the server. If operations are mixed, we get about 24.3 OPS: just about what would be expected from an average. So aside from a momentary point where the trap is processed, deoptimization has not affected the performance in any significant way.

The second thing that can cause code to be made not entrant is the way tiered compilation works. When code is compiled by the C2 compiler, the JVM must replace the code already compiled by the C1 compiler. It does this by marking the old code as not entrant and using the same deoptimization mechanism to substitute the newly compiled (and more efficient) code. Hence, when a program is run with tiered compilation, the compilation log will show a slew of methods that are made not entrant. Don’t panic: this “deoptimization” is, in fact, making the code that much faster.

The way to detect this is to pay attention to the tier level in the compilation log:

```
40915   84 %     3       net.sdo.StockPriceHistoryImpl::<init> @ 48 (156 bytes)
40923 3697       3       net.sdo.StockPriceHistoryImpl::<init> (156 bytes)
41418   87 %     4       net.sdo.StockPriceHistoryImpl::<init> @ 48 (156 bytes)
41434   84 %     3       net.sdo.StockPriceHistoryImpl::<init> @ -2 (156 bytes)
```

```
made not entrant
41458 3749    4    net.sdo.StockPriceHistoryImpl::<init> (156 bytes)
41469 3697    3    net.sdo.StockPriceHistoryImpl::<init> (156 bytes)
                 made not entrant
42772 3697    3    net.sdo.StockPriceHistoryImpl::<init> (156 bytes)
                 made zombie
42861  84 %   3    net.sdo.StockPriceHistoryImpl::<init> @ -2 (156 bytes)
                 made zombie
```

Here, the constructor is first OSR-compiled at level 3 and then fully compiled also at level 3. A second later, the OSR code becomes eligible for level 4 compilation, so it is compiled at level 4 and the level 3 OSR code is made not entrant. The same process then occurs for the standard compilation, and finally the level 3 code becomes a zombie.

## Deoptimizing zombie code

When the compilation log reports that it has made zombie code, it is saying that it has reclaimed previous code that was made not entrant. In the preceding example, after a test was run with the StockPriceHistoryLogger implementation, the code for the StockPriceHistoryImpl class was made not entrant. But objects of the StockPriceHistoryImpl class remained. Eventually all those objects were reclaimed by GC. When that happened, the compiler noticed that the methods of that class were now eligible to be marked as zombie code.

For performance, this is a good thing. Recall that the compiled code is held in a fixed-size code cache; when zombie methods are identified, the code in question can be removed from the code cache, making room for other classes to be compiled (or limiting the amount of memory the JVM will need to allocate later).

The possible downside is that if the code for the class is made zombie and then later reloaded and heavily used again, the JVM will need to recompile and reoptimize the code. Still, that’s exactly what happened in the previous scenario, where the test was run without logging, then with logging, and then without logging; performance in that case was not noticeably affected. In general, the small recompilations that occur when zombie code is recompiled will not have a measurable effect on most applications.

![](img/1711f142ae3cfd740ef106afbbba2046_120_0.png)

## Quick Summary

- The best way to gain visibility into how code is being compiled is by enabling PrintCompilation.
- Output from enabling PrintCompilation can be used to make sure that compilation is proceeding as expected.
- Tiered compilation can operate at five distinct levels among the two compilers.
- Deoptimization is the process by which the JVM replaces previously compiled code. This usually happens in the context of C2 code replacing C1 code, but it can happen because of changes in the execution profile of an application.

## Advanced Compiler Flags

This section covers a few other flags that affect the compiler. Mostly, this gives you a chance to understand even better how the compiler works; these flags should not generally be used. On the other hand, another reason they are included here is that they were once common enough to be in wide usage, so if you’ve encountered them and wonder what they do, this section should answer those questions.

## Compilation Thresholds

This chapter has been somewhat vague in defining just what triggers the compilation of code. The major factor is how often the code is executed; once it is executed a certain number of times, its compilation threshold is reached, and the compiler deems that it has enough information to compile the code.

Tunings affect these thresholds. However, this section is really designed to give you better insight into how the compiler works (and introduce some terms); in current JVMs, tuning the threshold never really makes sense.

Compilation is based on two counters in the JVM: the number of times the method has been called, and the number of times any loops in the method have branched back. Branching back can effectively be thought of as the number of times a loop has completed execution, either because it reached the end of the loop itself or because it executed a branching statement like continue.

When the JVM executes a Java method, it checks the sum of those two counters and decides whether the method is eligible for compilation. If it is, the method is queued for compilation (see “Compilation Threads” on page 107 for more details about queuing). This kind of compilation has no official name but is often called standard compilation.





类似地，每当循环完成一次执行，分支计数器就会递增并被检查。如果分支计数器超过了其各自的阈值，循环（而不是整个方法）就有资格进行编译。

调优会影响这些阈值。当分层编译被禁用时，标准编译由 `-XX:CompileThreshold=N` 标志的值触发。N 的默认值为 10,000。更改 CompileThreshold 标志的值将导致编译器选择比平常更早（或更晚）地编译代码。但请注意，虽然这里只有一个标志，但阈值的计算方式是：反向边循环计数器加上方法进入计数器的总和。

你经常会看到建议更改 CompileThreshold 标志的推荐，而且一些 Java 基准测试出版物也使用该标志（例如，经常在 8,000 次迭代之后）。有些应用程序在默认情况下仍然设置该标志。

但请记住，我说过这个标志在分层编译被禁用时才会生效——这意味着当分层编译启用时（通常情况下是启用的），这个标志根本不起作用。使用这个标志实际上只是 JDK 7 及更早时代的遗留做法。

这个标志过去被推荐有两个原因：第一，降低它可以改善使用 C2 编译器的应用程序的启动时间，因为代码会被更快地编译（而且通常效果相同）。第二，它可能让一些原本永远不会被编译的方法得到编译。

最后一点是一个有趣的怪现象：如果一个程序永远运行，我们难道不会期望它的所有代码最终都被编译吗？事实并非如此，因为编译器使用的计数器会随着方法和循环的执行而增加，但也会随着时间推移而减少。定期地（具体来说，当 JVM 到达安全点时），每个计数器的值都会被降低。

实际上，这意味着计数器是方法或循环*近期*热度的一个相对度量。一个副作用是，一些执行频率较高的代码可能永远不会被 C2 编译器编译，即使是永远运行的程序也是如此。这些方法有时被称为*温的*（相对于热的）。在分层编译之前，这种情况下降低编译阈值是有益的。

然而在今天，即使是温的方法也会被编译，尽管如果我们能让它们由 C2 编译器而不是 C1 编译器编译，也许还能有极微小的改进。实际收益很小，但如果你真的感兴趣，可以尝试更改标志 `-XX:Tier3InvocationThreshold=N`（默认 200）让 C1 更快地编译方法，以及 `-XX:Tier4InvocationThreshold=N`（默认 5000）让 C2 更快地编译方法。对于反向边阈值也有类似的标志。

![](img/1711f142ae3cfd740ef106afbbba2046_122_0.png)

## 快速总结

- 方法（或循环）被编译的阈值是通过可调参数设置的。
- 在没有分层编译的情况下，调整这些阈值有时是有意义的，但在分层编译的情况下，不再建议进行这种调优。

## 编译线程

第 105 页“编译阈值”一节提到，当方法（或循环）符合编译条件时，它会被排入编译队列。该队列由一个或多个后台线程处理。

这些队列并不是严格先进先出的；调用计数器较高的方法具有更高优先级。因此，即使程序刚开始执行并有很多代码需要编译，这种优先级排序也有助于确保最重要的代码首先被编译。（这也是 PrintCompilation 输出中编译 ID 可能乱序的另一个原因。）

C1 和 C2 编译器有不同的队列，每个队列由（可能多个）不同的线程处理。线程数基于一个复杂的对数公式，但表 4-2 列出了详细信息。

表 4-2. 分层编译时 C1 和 C2 编译器线程的默认数量

| CPU 数 | C1 线程数 | C2 线程数 |
|---|---|---|
| 1 | 1 | 1 |
| 2 | 1 | 1 |
| 4 | 1 | 2 |
| 8 | 1 | 2 |
| 16 | 2 | 6 |
| 32 | 3 | 7 |
| 64 | 4 | 8 |
| 128 | 4 | 10 |

编译器线程数可以通过设置 -XX:CICompilerCount=N 标志来调整。这是 JVM 用于处理队列的线程总数；对于分层编译，三分之一（但至少一个）将用于处理 C1 编译器队列，其余线程（也至少一个）将用于处理 C2 编译器队列。该标志的默认值是上表中两列之和。

![](img/1711f142ae3cfd740ef106afbbba2046_124_0.png)

## 快速总结

- 编译对于放入编译队列的方法是异步发生的。
- 队列并不是严格有序的；热点方法会排在队列中其他方法之前被编译。这也是编译日志中编译 ID 可能乱序的另一个原因。

## 内联

编译器所做的最重要的优化之一就是内联方法。遵循良好面向对象设计的代码通常包含通过 getter（可能还有 setter）访问的属性：

```
public class Point {
    private int x, y;

    public void getX() { return x; }
    public void setX(int i) { x = i; }
}
```

像这样的方法调用开销相当高，特别是相对于方法中的代码量而言。事实上，在 Java 的早期，性能建议常常反对这种封装，正是由于所有这些方法调用所带来的性能影响。幸运的是，JVM 现在通常会为这类方法执行代码内联。因此，你可以编写这样的代码：

```
Point p = getPoint();
p.setX(p.getX() * 2);
```

编译后的代码本质上会执行以下操作：

```
Point p = getPoint();
p.x = p.x * 2;
```

内联默认是启用的。可以使用 -XX:-Inline 标志禁用它，尽管它是一个非常重要的性能提升手段，你实际上永远不会这么做（例如，禁用内联会使股票批处理测试的性能降低超过 50%）。尽管如此，因为内联非常重要，也许还因为我们还有很多其他参数可以调整，所以经常会有关于调优 JVM 内联行为的建议。

不幸的是，对于 JVM 如何内联代码，没有基本的可见性。如果你从源码编译 JVM，可以生成一个包含 -XX:+PrintInlining 标志的调试版本。该标志提供了关于编译器做出的内联决策的各种信息。此时，你能做的最好的事情就是查看代码剖析结果，如果剖析结果顶部附近的任何简单方法看起来应该被内联，就尝试调整内联标志。

是否内联一个方法的基本决策取决于它的热点程度和大小。JVM 根据内部计算来确定方法是否为热点方法（即被频繁调用）；它不直接受任何可调参数的影响。如果一个方法因为被频繁调用而有资格内联，那么只有当其字节码大小小于 325 字节（或 -XX:MaxFreqInlineSize=N 标志指定的任何值）时才会被内联。否则，只有当它小于 35 字节（或 -XX:MaxInlineSize=N 标志指定的任何值）时，才有资格内联。

有时你会看到建议增大 MaxInlineSize 标志的值，以便内联更多方法。这种关系的一个经常被忽视的方面是，将 MaxInlineSize 的值设置得高于 35 意味着方法可能在第一次调用时就被内联。然而，如果方法被频繁调用——在这种情况下其性能重要得多——那么它最终也会被内联（假设其大小小于 325 字节）。否则，调整 MaxInlineSize 标志的净效果是它可能会减少测试所需的预热时间，但不太可能对长期运行的应用程序产生重大影响。

## 快速总结

![](img/1711f142ae3cfd740ef106afbbba2046_125_0.png)

- 内联是编译器能做的最有益的优化，尤其是对于属性封装良好的面向对象代码。
- 很少需要调整内联标志，而且这样做的建议往往没有考虑普通内联和频繁内联之间的关系。在调查内联效果时，请务必同时考虑这两种情况。

## 逃逸分析

C2 编译器在启用了逃逸分析（-XX:+DoEscapeAnalysis，默认值为 true）的情况下会执行激进优化。例如，考虑这个用于处理阶乘的类：

```
public class Factorial {
    private BigInteger factorial;
    private int n;
    public Factorial(int n) {
        this.n = n;
    }
    public synchronized BigInteger getFactorial() {
        if (factorial == null)
            factorial = ...;
```





```
return factorial;
}
}
```

要将前 100 个阶乘值存储到数组中，可使用以下代码：

```
ArrayList<BigInteger> list = new ArrayList<BigInteger>();
for (int i = 0; i < 100; i++) {
    Factorial factorial = new Factorial(i);
    list.add(factorial.getFactorial());
}
```

该阶乘对象仅在该循环内部被引用；没有任何其他代码能够访问该对象。因此，JVM 可以对该对象执行优化：

- 它在调用 getFactorial() 方法时无需获取同步锁。
- 它无需将字段 n 存储在内存中；它可以将该值保存在寄存器中。同样，它也可以将阶乘对象引用存储在寄存器中。
- 实际上，它甚至无需分配一个实际的阶乘对象；它只需跟踪该对象的各个字段即可。

这种优化非常复杂：在此示例中看似简单，但即使在更复杂的代码中也可能实现这些优化。根据代码的使用情况，并非所有优化都一定适用。但逃逸分析可以确定哪些优化是可能的，并在编译代码中做出必要的更改。

逃逸分析默认启用。在极少数情况下，它会出错。这通常不太可能，而且在当前 JVM 中更是罕见。尽管如此，由于过去曾出现过一些高调的错误，你有时会看到禁用逃逸分析的建议。但这些建议可能已不再适用；与所有激进的编译器优化一样，禁用此功能可能导致代码更稳定并非不可能。如果你发现确实如此，简化相关代码是最好的做法：更简单的代码编译效果更好。 （这是一个错误，应报告。）

![](img/1711f142ae3cfd740ef106afbbba2046_126_0.png)

## 快速总结

- 逃逸分析是编译器能够执行的最高级优化之一。这类优化经常导致微基准测试出现偏差。

## CPU 特定代码

我之前提到过，JIT 编译器的一大优势是它可以根据运行所在的处理器发出代码。这当然要求 JVM 在构建时具备对更新处理器的知识。

这正是编译器针对英特尔芯片所做的。2011 年，英特尔为 Sandy Bridge（及后续）芯片引入了高级向量扩展（AVX2）。随后 JVM 对这些指令的支持也随之而来。然后在 2016 年，英特尔将其扩展到包括 AVX-512 指令；这些指令存在于 Knights Landing 及后续芯片上。这些指令在 JDK 8 中不受支持，但在 JDK 11 中受支持。

通常，你无需担心此功能；JVM 会检测其运行的 CPU 并选择适当的指令集。但与所有新功能一样，有时会出现问题。

对 AVX-512 指令的支持首次在 JDK 9 中引入，但默认未启用。在几次尝试后，它被默认启用然后又被默认禁用。在 JDK 11 中，这些指令默认启用。然而，从 JDK 11.0.6 开始，这些指令再次默认禁用。因此，即使在 JDK 11 中，这仍在进行中。 （顺便说一下，这在 Java 中并不独特；许多程序在使 AVX-512 指令支持完全正确方面都遇到困难。）

因此，在某些更新的英特尔硬件上运行某些程序时，你可能会发现较早的指令集效果更好。从新指令集受益的应用类型通常涉及比 Java 程序更常见的科学计算。

这些指令集使用 -XX:UseAVX=N 参数选择，其中 N 如下：

- 0
  不使用任何 AVX 指令。
- 1
  使用英特尔 AVX 1 级指令（适用于 Sandy Bridge 及更高版本处理器）。
- 2
  使用英特尔 AVX 2 级指令（适用于 Haswell 及更高版本处理器）。
- 3
  使用英特尔 AVX-512 指令（适用于 Knights Landing 及更高版本处理器）。

此标志的默认值取决于运行 JVM 的处理器；JVM 会检测 CPU 并选择其支持的最高值。Java 8 不支持 3 级，因此在大多数处理器上你会看到使用 2。 在较新的英特尔处理器上的 Java 11 中，11.0.5 及更早版本的默认值为 3，之后的版本默认值为 2。

这是我在第 1 章中提到最好使用 Java 8 或 Java 11 最新版本的原因之一，因为像这样的重要修复都在这些最新版本中。如果必须在最新的英特尔处理器上使用较早版本的 Java 11，请尝试设置 -XX:UseAVX=2 标志，在许多情况下这会带来性能提升。

说到代码成熟度：为完整起见，我将提到 -XX:UseSSE=N 标志支持英特尔流式 SIMD 扩展（SSE）一到四。这些扩展适用于 Pentium 系列处理器。在 2010 年调整此标志是有意义的，因为其所有排列组合的使用方式都正在被研究。今天，我们通常可以依赖该标志的稳健性。

## 分层编译权衡

我多次提到，当禁用分层编译时，JVM 的工作方式会有所不同。鉴于其提供的性能优势，是否有理由将其关闭？

一个可能的原因是在内存受限的环境中运行。 当然，你的 64 位机器可能有大量内存，但你可能在内存限制较小的 Docker 容器中运行，或在云虚拟机中运行，该虚拟机内存可能不足。 或者你可能在大型机器上运行数十个 JVM。 在这些情况下，你可能希望减少应用程序的内存占用。

第 8 章提供了关于此的一般建议，但在本节中我们将研究分层编译对代码缓存的影响。

表 4-3 显示了在我的系统上启动 NetBeans 的结果，该系统在启动时会打开几十个项目。

表 4-3. 分层编译对代码缓存的影响

| 编译器模式 | 编译的类 | 已提交代码缓存 | 启动时间 |
|---|---|---:|---:|---:|
| +TieredCompilation | 22,733 | 46.5 MB | 50.1 seconds |
| -TieredCompilation | 5,609 | 10.7 MB | 68.5 seconds |

C1 编译器编译了大约四倍的类，并且可以预测地需要大约四倍的内存用于代码缓存。 绝对而言，在此示例中节省 34 MB 可能不会产生巨大差异。 在编译 200,000 个类的程序中节省 300 MB 在某些平台上可能是不同的选择。

禁用分层编译我们会失去什么？ 如表所示，我们确实花费更多时间启动应用程序并加载所有项目类。 但对于长期运行的程序，所有热点都将被编译的情况又如何？

在这种情况下，只要预热期足够长，禁用分层编译时的执行情况应该大致相同。 表 4-4 显示了我们的标准 REST 服务器在 0、60 和 300 秒预热期后的性能。

### 表 4-4. 分层编译下服务器应用的吞吐量

| 预热期 | -XX:-TieredCompilation | -XX:+TieredCompilation |
|---|---|---|
| 0 seconds | 23.72 | 24.23 |
| 60 seconds | 23.73 | 24.26 |
| 300 seconds | 24.42 | 24.43 |

测量期为 60 秒，因此即使没有预热，编译器也有机会获得足够的信息来编译热点；因此即使没有预热期，差异也很小。 （此外，在服务器启动期间编译了大量代码。）请注意，最终分层编译仍能获得微小的优势（尽管这种优势不太可能被注意到）。 我们在讨论编译阈值时讨论过原因：使用分层编译时，始终会有少量方法由 C1 编译器编译，而不会由 C2 编译器编译。

## javac 编译器

从性能角度来看，编译实际上是指 JVM 内置的 JIT。 回顾一下，Java 代码首先被编译成字节码；这通过 javac 过程完成。 因此，我们将以关于它的几点说明结束本节。

最重要的是，javac 编译器——有一个例外——根本不影响性能。 特别是：

- 包含额外调试信息的 -g 选项不影响性能。
- 在 Java 程序中使用 final 关键字不会产生更快的编译代码。
- 使用更新的 javac 版本重新编译通常不会使程序更快。

这三点多年来一直是通用建议，然后 JDK 11 出现了。 JDK 11 引入了一种新的字符串连接方式，其速度可能比以前版本更快，但需要重新编译代码才能利用它。 这是这里的例外；通常，你永远不需要



这再次是我们 REST 服务器的性能表现（尽管硬件与之前略有不同，因此基准 OPS 仅为 20.5 OPS 而非 24.4）。

这里值得注意的是其进展：JDK 11 是使用相当早期的 Graal 编译器版本构建的，因此该编译器的性能落后于 C2 编译器。Graal 编译器在其早期访问构建中得到了改进，但即使是其最新的早期访问（1.0）构建也并不比标准 VM 快。2019 年底的 Graal 版本（作为生产版本 19.2.1 发布）则变得明显更快。JDK 13 的早期访问版本使用了其中一个较晚的构建，并且在 Graal 编译器下实现了接近相同的性能，即使其 C2 编译器自 JDK 11 以来仅有适度的改进。

## 预编译

我们本章开头讨论了即时编译器的设计理念。虽然它有其优势，但代码在执行前仍需经历预热期。如果在我们的环境中，传统的编译模型会更好怎么办：没有 JIT 所需额外内存的嵌入式系统，或是在有机会预热之前就已完成的程序？

在本节中，我们将介绍两种实验性功能来解决该场景。提前编译（Ahead-of-time compilation）是标准 JDK 11 的实验性功能，而生成完全原生二进制文件的能力则是 Graal VM 的功能。

## 提前编译

*提前编译（Ahead-of-time, AOT）* 最初在 JDK 9 中仅适用于 Linux，但从 JDK 11 开始可在所有平台上使用。从性能角度来看，它仍在不断完善，但本节将为您提供一个预览。¹

> ¹ AOT 编译的一个好处是启动更快，但应用程序类数据共享——至少目前而言——在启动性能方面提供了更好的收益，并且是一个完全受支持的功能；有关更多详细信息，请参阅第 377 页的“类数据共享”。

AOT 编译允许您在运行应用程序之前预先编译应用程序的部分（或全部）。编译后的代码成为一个共享库，JVM 在启动应用程序时使用它。理论上，这意味着 JIT 在应用程序启动时可能无需参与：您的代码最初至少应与 C1 编译的代码一样好运行，而无需等待该代码被编译。

实际上情况略有不同：应用程序的启动时间很大程度上受共享库大小的影响（从而影响将该共享库加载到 JVM 中的时间）。这意味着像“Hello, world”这样简单的应用程序在使用 AOT 编译时不会运行得更快（事实上，根据预编译共享库的选择，它可能会运行得更慢）。AOT 编译的目标是像 REST 服务器这样具有相对较长启动时间的应用程序。这样，加载共享库的时间就可以被较长的启动时间抵消，从而产生收益。但请记住，AOT 编译是一个实验性功能，随着技术的演进，较小的程序也可能从中受益。

要使用 AOT 编译，您使用 jaotc 工具生成一个包含您选择的已编译类的共享库。然后该共享库通过运行时参数加载到 JVM 中。

jaotc 工具有多个选项，但要生成最佳库的方式大致如下：

```
$ jaotc --compile-commands=/tmp/methods.txt \
--output JavaBaseFilteredMethods.so \
--compile-for-tiered \
--module java.base
```

此命令将使用一组编译命令生成 java.base 模块的编译版本，并输出到指定的文件。您可以选择 AOT 编译一个模块，如我们在此所做的那样，也可以编译一组类。

加载共享库的时间取决于其大小，而大小取决于库中方法的数量。您也可以加载多个预编译代码不同部分的共享库，这可能更容易管理，但性能相同，因此我们将集中于单个库。

虽然您可能想预编译所有内容，但如果您审慎地仅预编译代码的子集，您将获得更好的性能。这就是为什么建议仅编译 java.base 模块。

编译命令（在本示例中为 /tmp/methods.txt 文件中的内容）也用于限制编译到共享库中的数据。该文件包含如下行：

```
compileOnly java.net.URI.getHost()Ljava/lang/String;
```

此行告诉 jaotc 在编译 java.net.URI 类时，仅包含 getHost() 方法。我们可以添加引用该类的其他方法的行以包含其编译；最终，只有文件中列出的方法才会被包含在共享库中。

要创建编译命令列表，我们需要一个应用程序实际使用的每个方法的列表。为此，我们按如下方式运行应用程序：

```
$ java -XX:+UnlockDiagnosticVMOptions -XX:+LogTouchedMethods \
-XX:+PrintTouchedMethodsAtExit <other arguments>
```

程序退出时，它将以如下格式打印程序使用的每个方法：

```
java/net/URI.getHost:()Ljava/lang/String;
```

要生成 methods.txt 文件，请保存这些行，为每行加上 compileOnly 前缀，并删除方法参数前面的冒号。

jaotc 预编译的类将使用 C1 编译器的形式，因此在长时间运行的程序中，它们不会得到最优编译。因此我们需要的最后一个选项是 --compile-for-tiered。该选项安排共享库使其方法仍有资格被 C2 编译器编译。

如果您对短生命周期程序使用 AOT 编译，省略此参数是可以的，但请记住目标应用集是服务器。如果我们不允许预编译的方法有资格进行 C2 编译，服务器的热性能将比最终可能达到的更慢。

毫不奇怪，如果您使用启用了分层编译的库运行应用程序并使用 -XX:+PrintCompilation 标志，您会看到与之前观察到的相同的代码替换技术：AOT 编译将显示为输出中的另一个层级，您会看到 AOT 方法在 JIT 编译它们时变得不可进入并被替换。

创建库后，您按如下方式使用它：

```
$ java -XX:AOTLibrary=/path/to/JavaBaseFilteredMethods.so <other args>
```

如果您想确保库正在被使用，请在 JVM 参数中加入 -XX:+PrintAOT 标志；该标志默认关闭。与 -XX:+PrintCompilation 标志一样，-XX:+PrintAOT 标志会在 JVM 使用预编译方法时产生输出。典型的行如下所示：

```
373 105 aot[ 1] java.util.HashSet.<init>(I)V
```

这里的第一列是程序启动以来的毫秒数，因此在 HashSet 类的构造函数从共享库加载并开始执行之前花费了 373 毫秒。第二列是分配给该方法的 ID，第三列告诉我们方法是从哪个库加载的。索引（本例中为 1）也会由此标志打印：

```
18 1 loaded /path/to/JavaBaseFilteredMethods.so aot library
```

JavaBaseFilteredMethods.so 是本例中加载的第一个（也是唯一一个）库，因此其索引为 1（第二列），后续对 aot 的引用带有该索引即指此库。

## GraalVM 原生编译

AOT 编译对相对较大的程序有益，但对小型、快速运行的程序没有帮助（甚至可能阻碍）。这是因为它仍是一个实验性功能，并且其架构要求 JVM 加载共享库。

另一方面，GraalVM 可以生成无需 JVM 即可运行的完整原生可执行文件。这些可执行文件非常适合短生命周期程序。如果您运行了示例，您可能在某些内容中注意到对 GraalVM 类的引用（例如被忽略的错误）：AOT 编译以 GraalVM 为基础。这是 GraalVM 的早期采用者功能；它可以在适当的许可下用于生产，但不受保修。

GraalVM 生成的二进制文件启动速度相当快，特别是与 JVM 中运行的程序相比。然而，在此模式下，GraalVM 并不会像 C2 编译器那样积极地优化代码，因此对于足够长时间运行的应用程序，传统的 JVM 最终会胜出。与 AOT 编译不同，GraalVM 原生二进制文件在执行期间不会使用 C2 编译类。

同样，由 GraalVM 生成的原生程序的内存占用起初明显小于传统的 JVM。然而，随着程序运行并扩展堆，这种内存优势会逐渐消失。

在将 Java 程序编译为原生代码时，也存在可使用的 Java 功能的限制。这些限制包括以下内容：

- 动态类加载（例如通过调用 Class.forName()）。
- 清理器。
- Java 安全管理器。
- JMX 和 JVMTI（包括 JVMTI 配置文件）。
- 使用反射通常需要特殊编码或配置。
- 使用动态代理通常需要特殊配置。
- 使用 JNI 需要特殊编码或配置。

我们可以通过使用 GraalVM 项目中的一个演示程序来看到所有这些情况，该程序递归地统计目录中的文件。对于少量文件，GraalVM 生成的原生程序非常小且快速，但随着更多工作被完成且 JIT 启动，传统的 JVM 编译器会生成更好的代码优化并且更快，如表 4-6 所示。

Table 4-6. Time to count files with native and JIT-compiled code

| 文件数 | Java 11.0.5 | 原生应用程序 |
|---|---|---|
| 7 | 217 ms (36K) | 4 ms (3K) |
| 271 | 279 ms (37K) | 20 ms (6K) |
| 169,000 | 2.3 s (171K) | 2.1 s (249K) |
| 1.3 million | 19.2 s (212K) | 25.4 s (269K) |



