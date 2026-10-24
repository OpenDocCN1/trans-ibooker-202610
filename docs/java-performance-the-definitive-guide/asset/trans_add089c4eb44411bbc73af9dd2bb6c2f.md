

与所有微基准测试一样，请深入思考这里的使用场景。与其采用我们这里的设置（不断将新创建的字符串塞入其中），不如对 ConcurrentHashMap 进行显式管理。这取决于具体应用，可能容易也可能难以实现；如果足够容易，ConcurrentHashMap 测试将展现出与常规字符串驻留或 CustomConcurrentHashMap 测试相同的优势。而在真实应用中，GC 压力才是关键：我们使用此方法只是为了去除重复字符串，以节省 GC 周期。

不过，这两种情况都不比使用经过适当调优的字符串表的测试更好。自定义映射表的优势在于无需预先设定大小：它可以根据需要自行调整大小。因此，相比于使用 intern() 方法并以依赖于具体应用的方式调整字符串表大小，它对各种应用的适应性要强得多。

## 字符串拼接

字符串拼接是另一个可能存在性能陷阱的领域。考虑这样一个简单的字符串拼接：

```java
String answer = integerPart + "." + mantissa;
```

Java 中的特殊优化可以处理这种构造（不过不同版本之间的细节有所差异）。

在 Java 8 中，javac 编译器会将这条语句转换为以下代码：

```java
String answer = new StringBuilder(integerPart).append(".")
    .append(mantissa).toString();
```

JVM 有专门的代码来处理这类构造（通过设置 -XX:+OptimizeStringConcat 标志来控制，该标志默认为 true）。

在 Java 11 中，javac 编译器会生成截然不同的字节码；该代码会调用 JVM 内部的一个特殊方法，以优化字符串拼接。

这是少数几个不同版本间字节码差异会产生影响的场景之一。通常，当你迁移到新版本时，无需重新编译旧代码：字节码是相同的。（当然，如果你想使用新的语言特性，就需要用新编译器编译新代码。）但这一特定优化依赖于实际的字节码。如果你使用 Java 8 编译执行字符串拼接的代码，并在 Java 11 上运行，Java 11 JDK 会应用与 Java 8 相同的优化。代码仍会被优化并运行得相当快。

不过，如果你在 Java 11 下重新编译代码，字节码将使用新的优化，可能会更快。

让我们考虑以下三种拼接两个字符串的情况：

```java
@Benchmark
public void testSingleStringBuilder(Blackhole bh) {
    String s = new StringBuilder(prefix).append(strings[0]).toString();
    bh.consume(s);
}
```

```java
@Benchmark
public void testSingleJDK11Style(Blackhole bh) {
    String s = prefix + strings[0];
    bh.consume(s);
}
```

```java
@Benchmark
public void testSingleJDK8Style(Blackhole bh) {
    String s = new StringBuilder().append(prefix).append(strings[0]).toString();
    bh.consume(s);
}
```

第一个方法是我们手工编写此操作的方式。第二个方法（使用 Java 11 编译时）会产生最新的优化，而最后一个方法（无论使用哪个编译器）在 Java 8 和 Java 11 中都会以相同方式被优化。

表 12-3 展示了这些操作的结果。

### 表 12-3：单次拼接的性能

| 模式 | 每次操作耗时 |
|---|---|
| JDK 11 优化 | 47.7 ± 0.3 ns |
| JDK 8 优化 | 42.9 ± 0.3 ns |
| 字符串构建器 | 87.8 ± 0.7 ns |

在这种情况下，旧的（Java 8）和新的（Java 11）拼接优化之间没有太大实际差异；尽管 JMH 告诉我们差异在统计上显著，但它们并不是特别重要。关键是，两种优化都比手工编写这个简单案例要好。这多少有些令人意外，因为手工编写的案例看起来更简单：它比 JDK 8 案例少一次对 append() 方法的调用，因此名义上所做的工作更少。但 JVM 内部的字符串拼接优化并不识别这种特定模式，所以最终反而更慢。

不过，Java 8 的优化并不适用于所有拼接。我们可以稍微修改一下测试，如下所示：

```java
@Benchmark
public void testDoubleJDK11Style(Blackhole bh) {
    double d = 1.0;
    String s = prefix + strings[0] + d;
    bh.consume(s);
}
```

```java
@Benchmark
public void testDoubleJDK8Style(Blackhole bh) {
    double d = 1.0;
    String s = new StringBuilder().append(prefix).
        append(strings[0]).append(d).toString();
    bh.consume(s);
}
```

现在性能有所不同，如表 12-4 所示。

### 表 12-4：拼接 double 值的性能

| 模式 | 每次操作耗时 |
|---|---|
| JDK 11 优化 | 49.4 ± 0.6 ns |
| JDK 8 优化 | 77.0 ± 1.9 ns |

JDK 11 的耗时与上一个示例相近，尽管我们附加了一个新值并且做了稍多一些的工作。但 JDK 8 的耗时则糟糕得多——大约慢了 50%。这其实并不是因为额外的拼接，而是因为拼接的类型。JDK 8 的优化对字符串和整数效果良好，但无法处理 double（以及大多数其他类型的数据）。在这些情况下，JDK 8 代码会跳过特殊优化，表现得像之前的手工编码测试一样。

当我们执行多次拼接操作时，尤其是循环内的拼接，这两种优化都不再适用。考虑以下测试：

```java
@Benchmark
public void testJDK11Style(Blackhole bh) {
    String s = "";
    for (int i = 0; i < nStrings; i++) {
        s = s + strings[i];
    }
    bh.consume(s);
}
```

```java
@Benchmark
public void testJDK8Style(Blackhole bh) {
    String s = "";
    for (int i = 0; i < nStrings; i++) {
        s = new StringBuilder().append(s).append(strings[i]).toString();
    }
    bh.consume(s);
}
```

```java
@Benchmark
public void testStringBuilder(Blackhole bh) {
    StringBuilder sb = new StringBuilder();
    for (int i = 0; i < nStrings; i++) {
        sb.append(strings[i]);
    }
    bh.consume(sb.toString());
}
```

现在结果则偏向于手工编码，这也在情理之中。尤其是 Java 8 实现，必须在每次循环迭代时创建一个新的 StringBuilder 对象；即使在 Java 11 中，每次循环都创建一个字符串（而不是在字符串构建器中累积）也会带来不小的开销。这些结果见表 12-5。

### 表 12-5：多次字符串拼接的性能

| 模式 | 10 个字符串 | 1,000 个字符串 |
|---|---|---|
| JDK 11 代码 | 613 ± 8 ns | 2,463 ± 55 µs |
| JDK 8 代码 | 584 ± 8 ns | 2,602 ± 209 µs |
| 字符串构建器 | 412 ± 2 ns | 38 ± 211 µs |

归根结底：如果可以在单行（逻辑行）内完成拼接，尽管使用拼接；但切勿在循环内使用字符串拼接，除非拼接后的字符串不会在下一轮循环迭代中使用。否则，始终显式使用 StringBuilder 对象以获得更好的性能。在第 1 章中，我曾论证有时候需要“过早”优化，即当这个说法在上下文中的含义仅仅是“编写良好的代码”时。这就是一个典型的例子。

![](img/1711f142ae3cfd740ef106afbbba2046_389_0.png)

### 快速总结

- 单行字符串拼接即可获得良好性能。
- 对于多次拼接操作，务必使用 StringBuilder。
- 涉及某些类型的单行字符串拼接，在 JDK 11 中重新编译后会显著更快。

## 缓冲 I/O

2000 年我加入 Java 性能小组时，我的老板刚刚出版了第一本关于 Java 性能的书，而当时最热门的话题之一就是缓冲 I/O。十四年后，我本以为这个话题已经过时，打算在本书第一版中略去不提。然而，就在我着手撰写第一版大纲的那一周，我针对两个毫不相关的项目提交了 bug，这两个项目都因未使用缓冲 I/O 而严重影响了性能。几个月后，在编写第一版的一个示例时，我挠着头想不明白为什么我的“优化”如此之慢。随后我意识到：真笨，我忘了正确地缓冲 I/O。

