

```
recur/fpij/Factorial.java
System.out.println(factorialTailRec(1, 5).invoke());
```

我们使用初始阶乘值 1 和数字来初始化 `factorialTailRec`。这次调用的结果是一个 `TailCall` 实例，我们在其上调用 `invoke` 方法。该调用的结果应该与我们之前看到的未优化递归版本相同。

让我们用大输入值运行这个版本。

```
recur/fpij/Factorial.java
System.out.println(factorialTailRec(1, 20000).invoke());
```

之前的版本遇到了栈溢出。让我们看看这个版本的命运。

0

我们的努力有了回报。我们避免了栈溢出，但由于算术溢出，结果为 0；阶乘结果是一个非常大的数字。我们很快就会修复它——我们需要使用 `BigInteger` 而不是 `int`。在解决那个问题之前，让我们重新审视一下解决方案。我们有一些清理工作要做。

## 清理递归

`factorialTailRec` 的实现看起来简单诱人。但它有一个缺点：我们污染了方法的接口。我们不再传递一个简单清晰的输入 `number`，而是必须传递两个参数。我们依赖调用者为第一个参数提供 1；像 0 这样的参数会导致结果偏离。我们还必须在对 `factorialTailRec` 的调用结果上调用 `invoke`——这不太愉快。我们可以通过引入额外的间接层来轻松修复这些问题。

我们可以将 `factorialTailRec` 设为私有方法，并引入一个调用它的公共方法。

recur/fpij/Factorial.java

```
public static int factorial(final int number) {
    return factorialTailRec(1, number).invoke();
}
```

这个方法恢复了简单的接口并封装了尾递归的细节。它处理额外的参数，并在最后负责调用必要的 `invoke` 方法。让我们使用这个修改后的版本。

recur/fpij/Factorial.java

```
System.out.println(factorial(5));
System.out.println(factorial(20000));
```

我们用较小的值和离谱的大值运行了最新版本；让我们看看输出。

```
120
0
```

对于较小的值，结果良好，但大值需要修复。让我们把它作为最后一步来处理。

## 修复算术溢出

使用 int 原始类型时，阶乘代码简洁优雅。为了避免算术溢出，我们必须切换到 `BigInteger`。不幸的是，我们将失去像 `*` 和 `-` 这样的简单算术运算符的流畅性，而必须使用 `BigInteger` 上的方法来执行这些操作。我们将通过在 `BigFactorial` 类中为这些操作创建小函数来减少代码中的混乱。

recur/fpij/BigFactorial.java

```
public class BigFactorial {
    public static BigInteger decrement(final BigInteger number) {
        return number.subtract(BigInteger.ONE);
    }

    public static BigInteger multiply(
        final BigInteger first, final BigInteger second) {
        return first.multiply(second);
    }

    final static BigInteger ONE = BigInteger.ONE;
    final static BigInteger FIVE = new BigInteger("5");
    final static BigInteger TWENTYK = new BigInteger("20000");

    //...
}
```

我们编写了一些使用 `BigInteger` 的便捷方法和字段。现在让我们来看看重要部分：封装的尾递归函数及其周围的流畅包装器。

![](img/f9269002f2414fff96e5e6b4dc20839f_261_0.png)

```
private static TailCall<BigInteger> factorialTailRec(
    final BigInteger factorial, final BigInteger number) {
    if(number.equals(BigInteger.ONE)) {
        return done(factorial);
    }

    return call(() ->
        factorialTailRec(multiply(factorial, number), decrement(number)));
}

public static BigInteger factorial(final BigInteger number) {
    return factorialTailRec(BigInteger.ONE, number).invoke();
}
```

与之前版本使用 int 不同，这个版本使用 `BigInteger`。其余代码基本相同，使用 `TailCall` 接口、`TailCalls` 类和 TCO 技术。

让我们调用这个修改后的阶乘版本。

recur/fpij/BigFactorial.java

```
public static void main(final String[] args) {
    System.out.println(factorial(FIVE));
    System.out.println(String.format("%.10s...", factorial(TWENTYK)));
}
```

既然我们使用了 `BigInteger`，操作应该可以正常进行。

120
1819206320...

我们看到数字 5 的正确阶乘值和大输入的裁剪输出值。

只需几行代码，我们就将未优化的递归转换为尾递归，并避免了栈溢出，这要归功于 lambda 表达式、函数式接口和无限流。

有了这项技术，我们可以大胆地实现递归解决方案，只需进行小幅重新设计即可将其转换为尾调用。

我们在此处使用的方法使递归对于大型输入成为可能。接下来，我们将了解如何从性能角度使其变得实用。

## 使用记忆化加速

快，25 乘以 12 等于多少？

除非你有超能力，否则你需要花些努力和时间才能得出 300 的结果。现在，如果我再问一次 25 乘以 12 等于多少，你会立即脱口而出 300。你怎么这么快就变好了呢？嗯，你记得结果，暂时记住了它——或者我们应该说你对其进行了记忆化。

如果你最近评估过一个表达式或查找过一些信息，并且你再次需要它，你很可能会使用记忆中的细节而不是重复执行任务。但计算机可以比我们高效得多地做这种事情。重用计算结果而不是重复评估，这项技术称为记忆化。

这在递归问题中尤其有用，因为解决方案包含多个具有相同解的子问题。

让我们看看记忆化如何将过度递归的问题转变为极快的执行。我们将探索一个问题，使用递归实现它，并注意随着问题规模增加它变得指数级缓慢。然后我们将使用记忆化技术加速它，并在此过程中了解 lambda 表达式如何帮助解决方案。

### 优化问题

我们可以在各个领域看到优化问题，例如经济、金融和资源分配，在这些领域中，从多个可行方案中选择一个最优解。例如，我们可能需要找到资产销售的最大利润或地点之间的最短路径。在一种称为 *动态规划* 的算法技术中，我们大量使用递归来解决问题。这将递归提升到一个新的水平；问题的解与子问题的解重叠。

如果我们天真地实现这样的递归，我们最终会得到计算，其运行时间随着输入规模的增加而呈指数级增长。这时就该用到 *记忆化* 了。在这项技术中，我们查找解决方案（如果已存在），并只执行和存储计算一次。反复请求重叠解决方案中存在的冗余不会转化为重新计算，而是转化为对结果的快速查找。这项技术将指数级时间复杂度转变为仅线性时间。让我们使用一个示例来实现这一点：杆切割问题。[15]

我们将为一家批发购买杆材并零售销售的公司提供解决方案。他们发现，通过将杆材切割成不同尺寸，可以最大化利润。公司对不同长度杆材收取的价格经常变化，因此公司希望我们编写一个程序，揭示给定尺寸杆材的最大利润是多少。让我们先找到一个简单的解决方案，然后再对其进行改进。

我们将从一个存储不同长度杆材价格的类开始。

recur/fpij/RodCutter.java

```
public class RodCutter {
private final List<Integer> prices;
public RodCutter(final List<Integer> pricesForLengths) {
    prices = pricesForLengths;
}

//...
}
```

让我们使用不同长度的一些示例价格，从 1 英寸开始。

recur/fpij/RodCutter.java

```
final List<Integer> priceValues =
    Arrays.asList(2, 1, 1, 2, 2, 2, 1, 8, 9, 15);

final RodCutter rodCutter = new RodCutter(priceValues);
```

### 简单递归

我们可以使用简单的递归来解决这个问题。如果给定一根 5 英寸的杆，我们可以查询该长度的价格。在此示例中，这将使我们得到 2 美元。我们可以做得更好——毕竟，一根 4 英寸的杆也能卖 2 美元，所以我们可以将杆切成两段——4 英寸和 1 英寸——来增加利润。继续这种方法，我们发现，任意长度 n 的最大利润是该长度所有可能 2^(n-1) 次切割所得利润的最大值。即，对于给定长度 n，max(不切割, 切割(1, n - 1), 切割(2, n - 2), ...)。下图是 5 英寸杆的所有可能切割所得利润的一个示例。

