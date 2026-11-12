
等待下一次对 factorialRec 的调用返回结果。因此，每次调用都会使调用栈增长，如果输入规模不断增大，代码执行最终会崩溃。我们需要一种不保留栈的递归方式。

在《计算机程序的构造和解释》[AS96]中，Abelson 和 Sussman 讨论了 TCO 技术，他们在内部将递归转换为纯迭代。理想情况下，我们希望依赖编译器提供此类优化，但由于编译器不提供，我们可以使用 lambda 表达式手动完成，正如我们接下来将看到的。

## 转向尾递归

在使用 TCO 技术之前，我们必须重新设计代码，使其不会构建调用栈。而不是等待在 factorialRec 方法的第 7 行执行乘法，我们可以使用已有内容计算部分乘积，并将其作为额外参数传递给后续调用。这样，当我们从递归调用返回时，就不需要执行任何算术运算了。这是一个很好的第一步，但还不够。此外，我们在递归调用方法之前，必须先退出当前栈层级。换句话说，我们需要将对 factorialRec 的急切调用转换为惰性调用。为此，我们将使用 TailCall 函数式接口和一个伴随的 TailCalls 类。我们很快就会设计这两者，但先假设它们已经存在。

首先，让我们向 TailCalls 类的方法添加静态导入。

```
recur/fpij/Factorial.java

import static fpij.TailCalls.done;
import static fpij.TailCalls.call;
```

我们将在用于计算阶乘的新递归版本中使用这两个方法，即 `factorialTailRec` 方法。

```
recur/fpij/Factorial.java

public static TailCall<Integer> factorialTailRec(
    final int factorial, final int number) {
    if (number == 1) {
        return done(factorial);
    }

    return call(() -> factorialTailRec(factorial * number, number - 1));
}
```

这个计算阶乘的版本是尾递归的；也就是说，最后一次操作是对自身的（延迟/惰性）调用，并且在返回时无需对结果进行进一步计算。此外，我们没有急切地调用方法 `factorialTailRec`，而是将其包装在 lambda 表达式中以供惰性/稍后执行。

## 创建 TailCall 函数式接口

当我们调用 `factorialTailRec` 方法时，它会立即返回一个 `TailCall` 实例。关键在于，如果我们调用 `done` 方法，则表示递归终止。另一方面，如果我们通过 `call` 方法进行调用，则表示要求递归继续，但只有在退出当前栈层级之后才会继续。为了充分理解这是如何工作的，我们需要查看这些方法的内部，因此让我们深入了解 `TailCall` 接口和 `TailCalls` 伴随类。我们先从接口开始。

```
recur/fpij/TailCall.java

@FunctionalInterface
public interface TailCall<T> {

    TailCall<T> apply();

    default boolean isComplete() { return false; }

    default T result() { throw new Error("not implemented"); }

    default T invoke() {
        return Stream.iterate(this, TailCall::apply)
            .filter(TailCall::isComplete)
            .findFirst()
            .get()
            .result();
    }
}
```

此接口中有四个方法：一个抽象方法，其余为默认方法。isComplete 方法仅返回 false 值。result 方法的默认实现在被调用时会崩溃——只要递归正在进行，我们就永远不会调用此方法；TailCall 接口的替代实现将在递归终止时处理这种情况。

关键工作在 invoke 方法中的简短代码中完成。该方法与 apply 方法协作，apply 方法将返回等待执行的下一个 TailCall 实例。invoke 方法有两个职责。首先，它必须反复遍历待处理的 TailCall 递归，直到达到递归末尾。其次，到达末尾后，它必须返回最终结果（可在终止 TailCall 实例的 result 方法中获取）。

invoke 方法很短，但这里有很多内容，所以让我们放慢速度，深入探讨一下。

我们不知道将评估多少次递归；它不是无限的，但我们可以将其视为一系列未知长度的对象。一旦我们能够将此视为一系列 TailCall 对象，就可以轻松地对待处理 TailCall 实例的 Stream 进行惰性迭代。我们在“创建无限惰性集合”中使用的技术将帮助我们在这里惰性地生成下一个待处理 TailCall 实例。让我们仔细看看如何做到这一点。

要创建待处理 TailCall 实例的惰性列表，我们使用 Stream 接口的 iterate 静态方法。此方法接受一个初始种子值和一个生成器。我们使用当前 TailCall 实例（this）作为种子。生成器是一个 UnaryOperator，它接收当前元素并生成下一个元素。为了让生成器返回下一个待处理 TailCall 实例，它可以使用当前 TailCall 的 apply 方法。我们为此目的使用方法引用 TailCall::apply 来创建生成器。

简而言之，我们设计了 invoke 方法，使得迭代从种子（即第一个 TailCall 实例）开始，并遍历生成器生成的后续 TailCall 实例，直到找到表示递归终止的 TailCall 实例。

## 创建 TailCalls 便利类

迭代将继续，直到 isComplete 方法报告完成。但是，TailCall 接口中此方法的默认实现始终返回 false 值。这时伴随类 TailCalls 就派上用场了。它提供了 TailCall 函数式接口的两种不同实现：一种在 call 方法中，另一种在 done 方法中。

![](img/f9269002f2414fff96e5e6b4dc20839f_256_0.png)

```
public class TailCalls {
    public static <T> TailCall<T> call(final TailCall<T> nextCall) {
        return nextCall;
    }

    public static <T> TailCall<T> done(final T value) {
        return new TailCall<T>() {
            @Override public boolean isComplete() { return true; }
            @Override public T result() { return value; }
            @Override public TailCall<T> apply() {
                throw new Error("not implemented");
            }
        };
    }
}
```

在这个类中，我们实现了两个静态方法 call 和 done。call 方法仅接收一个 TailCall 实例并将其传递下去。这是一个便利方法，以便递归调用（如 factorialTailRec）可以优雅地以对 done 或 call 的对称调用结束。

在 done 方法中，我们返回一个专门的 TailCall 版本以指示递归的终止。在此方法中，我们将接收到的值包装到专门实例的重写 result 方法中。专门版本的 isComplete 将通过返回 true 值来报告递归的结束。最后，apply 方法会抛出异常，因为此方法永远不会在表示递归结束的这个终止 TailCall 实现上被调用。

在这个设计中，我们可以看到通过 call 返回的 TailCall 如何继续递归，而从 done 返回的 TailCall 如何终止递归。此外，递归调用全部在 invoke 默认方法的循环中惰性求值，因此绝不会像简单递归那样增加栈层级。

我们设计 TailCall 和 TailCalls 是为了与 factorialTailRec 一起使用，但它们可用于任何尾递归函数。

## 使用尾递归函数

我们看到了尾递归函数 factorialTailRec、函数式接口 TailCall 和便利类 TailCalls。让我们通过一个场景来了解这些如何协同工作。

让我们先从对 factorialTailRec 的调用开始，以计算 2 的阶乘，如下所示：

```
factorialTailRec(1, 2).invoke();
```

第一个参数 1 是阶乘的初始值；第二个参数 2 是我们想要求阶乘的值。对 factorialTailRec 的调用将检查给定数字是否等于 1，既然不等于，就会使用 call 方法并传递一个 lambda 表达式，该表达式生成一个 TailCall 实例。

这个合成的实例将惰性地调用 factorialTailRec，分别传入两个参数 2 和 1。回到 factorialTailRec 方法调用之外，对 invoke 方法的调用将以这个第一个 TailCall 实例作为种子创建一个惰性集合，并探索该集合，直到收到一个终止的 TailCall 实例。当种子 TailCall 的 apply 方法被调用时，它将导致使用之前提到的两个参数调用 factorialTailRec。这次对 factorialTailRec 的第二次调用将导致对 done 方法的调用。

对 done 的调用将返回一个终止的专门 TailCall 实例，以示递归终止。invoke 方法现在将返回计算的最终结果，在此例中为 2。

阶乘递归的 TCO 已完成。让我们来试用一下 factorialTailRec 方法。我们先用较小的输入参数值来调用它。
