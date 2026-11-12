

![](img/f9269002f2414fff96e5e6b4dc20839f_239_0.png)

但是，filter 和 map 方法骨子里都是惰性的。随着执行过程沿链条推进，filter 和 map 方法会存储 lambda 表达式，并将一个外观传递给链中的下一个调用。只有在调用终端操作 findFirst 时，求值才会开始。

求值顺序也不同，如下图所示。filter 方法不会一次性处理完集合中的所有元素。相反，它会运行直到找到满足附加 lambda 表达式中给定条件的第一个元素为止。一旦找到元素，它就将其传递给链中的下一个方法。在这个示例中，这个下一个方法（即 map）对给定输入执行其部分操作，并将其传递下链。当求值到达末尾时，终端操作会检查是否已收到所需的结果。

如果终端操作得到了所需内容，链的计算即终止。如果终端操作不满意，它会要求对集合中的更多元素执行操作链。

通过检查此操作排序的逻辑，我们可以看到执行将仅遍历集合中必要的元素。通过运行代码，可以看到这种行为的证据。

```
getting length for Brad
getting length for Kate
getting length for Kim
converting to uppercase: Kim
KIM
```

从输出可以看到，一旦找到了我们要找的候选名称，示例列表中的大多数元素都未被求值。

上一个示例中看到的操作逻辑序列，在 JDK 内部是通过融合操作实现的——中间操作中的所有函数被融合为一个函数，在每个元素上按需求值，直到终端操作满意为止。归根结底，数据只进行一次遍历——过滤、映射和元素选择都在一次操作中完成。

## 窥探惰性

在 Java 中，将一系列操作写成链式调用是首选且自然的方式。但要真正看出惰性求值直到调用终端操作才开始，我们可以将前面代码中的链拆分为步骤。

```
lazy/fpij/LazyStreams.java
Stream<String> namesWith3Letters =
names.stream()
.filter(name -> length(name) == 3)
.map(name -> toUpper(name));

System.out.println("Stream created, filtered, mapped...");
System.out.println("ready to call findFirst...");

final String firstNameWith3Letters =
namesWith3Letters.findFirst()
.orElse("");

System.out.println(firstNameWith3Letters);
```

我们将集合转换为流，过滤了值，然后映射了结果集合。然后，单独调用了终端操作。让我们运行代码来查看求值顺序。

```
Stream created, filtered, mapped...
ready to call findFirst...
getting length for Brad
getting length for Kate
getting length for Kim
converting to uppercase: Kim
KIM
```

从输出可以清楚地看出，中间操作将真正的工作推迟到了调用终端操作这一最后负责时刻。即使如此，它们也只完成了满足终端操作所需的最少工作。非常高效，是吧？

这个例子帮助我们揭示了 Stream 的真正威力。接下来，我们将使用 Streams 创建无限集合。

## 创建无限惰性集合

使用无限集合可以使创建不断增长序列（例如斐波那契数）的代码更清晰、更易于表达。但根据我们在 Java 中的经验，由于实际内存限制，可能会认为序列不能是无限的。在这里，Stream 的惰性再次派上用场。

在 Java 中，集合仍然需要是有限的，但流可以是无限的。我们将在这里看到，惰性如何成为实现无限流的关键支撑。我们将使用它们来创建富有表现力、易于理解的代码，以生成无限增长的序列。

### 绝望的尝试

我们将使用素数序列 2、3、5、7、... 作为示例来探讨这里的概念。首先，让我们创建一个辅助函数来判断一个数字是否为质数。

```
public static boolean isPrime(final int number) {
    return number > 1 &&
    IntStream.rangeClosed(2, (int) Math.sqrt(number))
    .noneMatch(divisor -> number % divisor == 0);
}
```

大于 1 的数如果不能被 2 到其平方根之间的任何数整除，则是质数。通常，我们会使用外部迭代器在该范围内搜索除数。相反，这里我们使用了更多 Java 的精髓——IntStream 接口的 rangeClosed 静态方法。

方法名中的 closed 后缀强调，值的范围将包含第二个参数。例如，rangeClosed(1, 10) 将返回包含值 1、2、...、10 的范围，并将其打包到一个 Stream 中。相比之下，接口中的另一个静态方法 range 将返回一个值范围，直到（但不包括）第二个参数中的值。

在 `isPrime` 方法中，我们使用 `rangeClosed` 方法返回的流上的简洁明了的 `noneMatch` 方法。`noneMatch` 方法将 `Predicate` 作为其参数，我们使用它来确定给定数字是否有除数。如果 lambda 表达式对范围内所有值都返回 `false`，`noneMatch` 方法将产生 `boolean` 值 `true`——也就是说，如果没有除数。

在第一次尝试中，我们将使用 `isPrime` 方法从给定 `number` 开始创建一系列质数。

```
//don't try this at the office
public static List<Integer> primes(final int number) {
    if(isPrime(number)) {
        return concat(number, primes(number + 1));
    }

    return primes(number + 1);
}
```

如果给定的 `number` 是质数，我们将其包含在其后的质数列表中。否则，我们跳过它并继续获取该列表。

别急着发推文；不，你谦卑的作者并没有疯——那段代码诱人地优雅，但 sadly，行不通。如果我们实现 `concat` 方法并运行代码，将进入无限递归并最终抛出 `java.lang.StackOverflowError`。

让我们深入研究一下代码，看看能否从中挽回些什么。`StackOverflowError` 来自对 `primes` 方法的递归调用。如果我们能对这个调用采取惰性策略，就不会出问题。

## 触及星辰

前面我们已经看到 Stream 有多惰性。它们在我们要求结果之前不会做任何实际工作——有点像我的孩子。我们可以依赖这种惰性来轻松创建一个惰性的无限集合。

当我们从集合或通过其他方式创建 Stream 时，我们会很快收到一个外观，它有可能返回一个无限列表。但它非常巧妙；它只返回我们要求的那么多元素，按需生成元素。我们可以使用该功能来表达无限集合，并从该列表中生成我们想要的任意数量的（有限）元素。让我们来看看如何实现。

Stream 接口有一个名为 iterate 的静态方法，可以创建无限 Stream。它需要两个参数：启动集合的种子值，以及一个 UnaryOperator 接口的实例，它是集合中的数据提供者。iterate 方法返回的 Stream 会推迟创建元素，直到我们使用终止方法请求它们为止。例如，要获取第一个元素，可以调用 findFirst 方法。要获取十个元素，可以在 Stream 上调用 limit 方法，例如：limit(10)。

让我们看看这些想法在代码中如何体现。

```
lazy/fpij/Primes.java

public class Primes {
  private static int primeAfter(final int number) {
    if(isPrime(number + 1)) {
      return number + 1;
    }

    return primeAfter(number + 1);
  }
}

