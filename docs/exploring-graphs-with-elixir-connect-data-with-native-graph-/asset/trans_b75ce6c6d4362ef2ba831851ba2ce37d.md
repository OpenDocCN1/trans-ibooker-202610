

```java
public static List<Integer> primes(final int fromNumber, final int count) {
    return Stream.iterate(primeAfter(fromNumber - 1), Primes::primeAfter)
           .limit(count)
           .collect(toList());
}
//...
}
```

我们首先定义了一个便捷方法 primeAfter，它返回给定数之后的一个质数。如果给定数旁边的数是质数，则立即返回；否则，该方法会递归地询问接下来的质数。处理无限系列的代码位于 primes 方法中。它在功能上虽然简洁，但真正的复杂性隐藏在 iterate 方法和 Stream 中。

primes 方法将创建一个无限质数系列，起始于大于或等于给定参数的第一个质数。在调用 iterate 方法时，第一个参数为无限系列提供种子值。如果给定的数是质数，则它被用作种子。否则，则使用该数之后的第一个质数。第二个参数（即方法引用）充当 UnaryOperator，该操作符接受一个参数并返回一个值。在此示例中，由于我们引用了 primeAfter 方法，它接受一个数并返回该数之后的质数。

调用 iterate 方法的结果是一个 Stream，它会缓存所给定的 UnaryOperator。当我们请求特定数量的元素，且只有在此时，Stream 才会将当前元素（给定的种子值作为第一个元素）传递给缓存的 UnaryOperator 以获取下一个元素，然后再将该元素传回给 UnaryOperator 以获取接下来的元素。这个序列会重复所需的次数，直到获取到我们请求的元素数量，如下图所示。

![](img/f9269002f2414fff96e5e6b4dc20839f_247_0.png)

### 仅按需执行

让我们先调用 `primes` 方法，获取从 1 开始的十个质数，然后再获取从 100 开始的五个质数。

```
lazy/fpij/Primes.java
```

```
System.out.println("10 primes from 1: " + primes(1, 10));

System.out.println("5 primes from 100: " + primes(100, 5));
```

`primes` 方法创建一个从给定输入开始的无限质数集合的 `Stream`。要从集合中获取特定数量的元素，我们调用 `limit` 方法。然后，我们将返回的元素集合转换为列表并打印出来。对 `collect` 的调用触发了序列的求值。方法 `limit` 也是一种中间操作，它会惰性地记录后续求值所需的元素数量！让我们来看这段代码的输出。

```
10 primes from 1: [2, 3, 5, 7, 11, 13, 17, 19, 23, 29]
5 primes from 100: [101, 103, 107, 109, 113]
```

该代码生成了两个质数系列，一个从 1 开始，另一个从 100 开始。它们是从我们在 `primes` 方法中如此简洁地创建的无限系列中提取的，这要归功于 Streams 的惰性特性以及 lambda 表达式/方法引用的强大功能。

我们看到 lambda 表达式和 Stream 实现是如何协同工作以提高执行效率的。虽然 lambda 表达式和方法引用使代码优雅、富有表现力且简洁，但 Java 应用程序中真正的性能提升将来自 Streams。lambda 表达式是 Java 的入门毒品，而 Streams 才是真正的瘾头——在开发 Java 应用程序时，请做好沉迷其中的准备。

我们仅用几行代码就完成了大量工作；花几分钟欣赏 lambda 表达式、函数式接口以及 Streams 效率的强大之处，完全没问题。在下一章中，我们将把 lambda 表达式的使用再提升一个层次，使递归更加高效。

## 总结

lambda 的引入为 Java 带来了效率提升；我们可以惰性地推迟代码执行，直到真正需要它时才执行。我们可以延迟重量级资源的初始化，并轻松实现虚拟代理模式。同样，我们可以推迟方法参数的求值，以使调用更加高效。改进版 JDK 的真正功臣是 Stream 接口及其相关类。我们可以利用它们的惰性行为，只需几行代码即可创建无限集合。这意味着可以编写高度富有表现力、简洁的代码来执行复杂操作，而这些操作在以前的 Java 中我们甚至无法想象。

在下一章中，我们将探讨 lambda 表达式在优化递归中的作用。

### 脚注

[14] [http://en.wikipedia.org/wiki/David_Wheeler_(computer_scientist)](http://en.wikipedia.org/wiki/David_Wheeler_(computer_scientist))

## 优化递归

递归是解决问题的一种强大且迷人的方式。它高度富有表现力——使用递归，我们可以通过对子问题应用相同的解决方案来提供一个问题的解决方案，这种方法被称为分治法。各种应用都采用递归，例如在地图上寻找最短距离、计算最小成本或最大利润，或减少浪费。

当今大多数使用的语言都支持递归。不幸的是，真正受益于递归的问题往往规模相当大，而简单的实现很快就会导致栈溢出。在本章中，我们将探讨尾调用优化（TCO）技术，使递归能够处理大规模输入。接着，我们将研究可以用高度递归的重叠解决方案来表达的问题，并探讨如何使用记忆化技术使它们运行得飞快。

## 使用尾调用优化

使用递归最大的障碍是大规模输入问题的栈溢出风险。出色的 TCO 技术可以消除这一顾虑。尾调用是指最后执行的操作是对自身的调用的递归调用。这与普通递归不同，在普通递归中，函数除了调用自身外，通常还会对递归调用的结果进行进一步计算。TCO 让我们能够将普通递归调用转换为尾调用，从而使递归在处理大规模输入时变得实用。

Java 在编译器级别不直接支持 TCO，但我们可以使用 lambda 表达式在几行代码中实现它。这种解决方案有时被称为*蹦床调用*，让我们可以享受递归的强大功能，而无需担心栈溢出。

我们将使用一个非常简单且常见的示例来实现 TCO，即计算一个数的阶乘。

### 从未优化的递归开始

让我们先来看一段使用简单未优化递归来计算阶乘的代码。

```
recur/fpij/Factorial.java

1: public class Factorial {
2:   public static int factorialRec(final int number) {
3:     if(number == 1) {
4:       return number;
5:     }
6: 
7:       return number * factorialRec(number - 1);
8:   }
9: }
```

当递归降到值为 1 时终止。对于较大的值，我们递归调用该数乘以该数减 1 的阶乘。让我们使用数字 5 来试用这个方法。

```
recur/fpij/Factorial.java
```

```
System.out.println(factorialRec(5));
```

120

这似乎有效，但让我们再试一次，这次使用一个较大的输入值。

```
recur/fpij/Factorial.java
```

```
try {
    System.out.println(factorialRec(20000));
} catch(StackOverflowError ex) {
    System.out.println(ex);
}
```

我们以防御性的方式编写了那个调用；让我们来看看输出结果。

java.lang.StackOverflowError

递归没有处理大规模输入。它轰然崩溃。这在采用这种强大且富有表现力的技术时是一个严重障碍。

问题不在于递归本身。它是由在等待递归完成时保持计算的部分结果引起的。让我们更仔细地看看 factorialRec 方法中的第 7 行。我们在该行上执行的最后操作是乘法（*）。在保留给定数字的同时，我们

