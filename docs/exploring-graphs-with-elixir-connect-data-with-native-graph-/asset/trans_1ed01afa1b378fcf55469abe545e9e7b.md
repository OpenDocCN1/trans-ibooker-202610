
public String toString() { return "quite heavy"; }
}

这个类代表一个假想的重量级资源。在其构造函数中，我们打印一条消息来告知它何时被创建。让我们在 Holder 类的第一个试用版本中使用这个类的实例，名为 HolderNaive。

## lazy/fpij/HolderNaive.java

```java
public class HolderNaive {
    private Heavy heavy;

    public HolderNaive() {
        System.out.println("Holder created");
    }

    public Heavy getHeavy() {
        if (heavy == null) {
            heavy = new Heavy();
        }

        return heavy;
    }

    //...
}
```

乍一看，这段代码似乎很简单。我们创建了一个空引用 heavy，并在首次调用 `getHeavy` 方法时将其赋值为合适的实例。让我们使用这个类来创建 `HolderNaive` 的实例，并看看它是否推迟了 `Heavy` 实例的创建。

## lazy/fpij/HolderNaive.java

```java
final HolderNaive holder = new HolderNaive();
System.out.println("deferring heavy creation...");
System.out.println(holder.getHeavy());
System.out.println(holder.getHeavy());
```

这段代码的输出如下：

Holder created
deferring heavy creation...
Heavy created
quite heavy
quite heavy

这似乎有效。这个解决方案很常见，但也相当简单，无法保证线程安全。让我们逐步分析。

## 提供线程安全

对于 `HolderNaive` 的一个实例，依赖的 `Heavy` 实例会在首次调用 `getHeavy` 方法时创建。在随后对该方法的调用中，将返回已创建的实例。这正是我们想要的，但有个问题。这段代码存在竞态条件。

如果两个或多个线程同时调用 `getHeavy` 方法，我们可能会得到多个 `Heavy` 实例，可能每个线程一个。这个副作用是不希望的。让我们修复它。

```java
public synchronized Heavy getHeavy() {
    if (heavy == null) {
        heavy = new Heavy();
    }
    return heavy;
}
```

我们使用 `synchronized` 关键字标记 `getHeavy` 方法以确保互斥。如果两个或多个线程并发调用此方法，由于互斥，只有一个线程被允许进入，其他线程将排队等待。第一个进入方法的线程将创建实例。

当后续线程进入此方法时，它们会看到实例已存在，并直接返回它。

我们避免了竞态条件，但该方案带来了另一个负面影响。现在对 `getHeavy` 方法的每次调用都必须忍受同步开销；即使没有并发竞争线程，调用线程也必须跨越内存屏障（参见 Brian Goetz 的 *Java 并发编程实践* [Goe06]）。

实际上，竞态条件发生的可能性非常短暂，仅在首次给 heavy 引用赋值时才可能发生，同步方法是一种过于粗暴的解决方案。我们需要在首次创建引用之前保证线程安全，之后则可以自由、无阻碍地访问该引用。我们将采用 David Wheeler 的建议来实现这一点：“计算机科学中的任何问题都可以通过增加一个间接层来解决。”[14]

## 增加一个间接层

本示例中添加的间接层来源于 `Supplier<T>` 类。这是 JDK 中的一个函数式接口，只有一个名为 `get` 的抽象方法，用于返回一个实例。换句话说，这是一个无需输入就不断给出的工厂，有点像母亲的爱。

在最基本的形式中，`Supplier` 将返回一个实例。例如，我们可以实现 `Supplier<Heavy>` 来返回 `Heavy` 的实例，如下所示：

```java
Supplier<Heavy> supplier = () -> new Heavy();
```

或者，我们可以使用构造函数引用代替传统的 `new` 语法来实例化一个实例。构造函数引用很像方法引用，只不过它引用的是构造函数而不是方法。只要 lambda 表达式仅仅是实例化一个实例，就可以使用构造函数引用。让我们看一个使用构造函数引用的示例。

```java
Supplier<Heavy> supplier = Heavy::new;
```

我们看到了 `Supplier` 能为我们做什么，但我们需要比这种简单形式更多的东西。我们需要推迟并缓存实例。我们可以通过将实例创建移到另一个函数中来实现这一点，如下所示，这是 `Holder` 类的最终版本。

lazy/fpij/Holder.java

```java
public class Holder {
    private Supplier<Heavy> heavy = () -> createAndCacheHeavy();

    public Holder() {
        System.out.println("Holder created");
    }

    public Heavy getHeavy() {
        return heavy.get();
    }

    //...
}
```

在这个版本中，字段 `heavy` 是 `Supplier<Heavy>` 的一个实例。我们将其赋值为一个 lambda 表达式，Java 编译器会从中合成一个具有预期 `get` 方法的实现。该实现简单地将调用路由到 `createAndCacheHeavy` 方法，我们很快会实现该方法。`getHeavy` 方法返回与 `Supplier` 的 `get` 方法相同的内容。

如我们所见，创建 `Holder` 实例时，不会创建 `Heavy` 实例。这种设计实现了延迟初始化的目标。我们还需要一种不过分严苛的线程安全解决方案。`createAndCacheHeavy` 方法正是为此而设。

让我们先看看这个方法的代码。

lazy/fpij/Holder.java

```java
private synchronized Heavy createAndCacheHeavy() {
    class HeavyFactory implements Supplier<Heavy> {
        private final Heavy heavyInstance = new Heavy();

        public Heavy get() { return heavyInstance; }
    }

    if (!HeavyFactory.class.isInstance(heavy)) {
        heavy = new HeavyFactory();
    }

    return heavy.get();
}
```

我们将把这个方法标记为 `synchronized`，以便并发调用该方法的线程互斥。但在该方法内部，首次调用时，我们会迅速将 `Supplier` 引用 `heavy` 替换为直接供应商 `HeavyFactory`，后者将返回一个 `Heavy` 实例。让我们看看这如何妥善解决线程安全问题。

让我们考虑这样一个场景：刚刚创建了一个 `Holder` 的新实例。假设两个线程并发调用 `getHeavy` 方法，随后第三个线程在很久之后调用该方法。当前两个线程调用 `Holder` 中默认供应商的 `get` 方法时，`createAndCacheHeavy` 方法会让其中一个通过，并让另一个等待。第一个进入的线程将检查 `heavy` 是否是 `HeavyFactory` 的实例。由于它不是默认的 `Supplier`，该线程会将 `heavy` 替换为 `HeavyFactory` 的一个实例。最后，它返回该 `HeavyFactory` 持有的 `Heavy` 实例。

第二个进入的并发线程会再次检查 `heavy` 是否是 `HeavyFactory` 的实例，并跳过创建。它将简单地返回第一个线程返回的同一实例。这里我们假设 `Heavy` 本身是线程安全的，我们只关注 `Holder` 的线程安全。

我们已经解决了竞态条件，但由于实例是延迟创建的，我们不再需要如此保守。既然 `heavy` 已被替换为 `HeavyFactory`，后续对 `getHeavy` 方法的调用将直接进入 `HeavyFactory` 的 `get` 方法，而不会产生任何同步开销。

我们设计了延迟初始化，同时避免了空检查。我们还确保了延迟实例创建的线程安全性。这是虚拟代理模式的一种简单、轻量级实现。接下来，我们将使用 lambda 表达式来推迟函数求值。

## 延迟求值

在上一节中，我们推迟了重量级对象的创建，以加快代码执行速度。本节将进一步探讨这一点，以推迟方法的运行，并利用这种方法来改进我们的设计。主要目标是将代码执行减少到最低限度——尤其是昂贵的代码——并加快执行速度。

Java 在求值逻辑运算时已经使用了延迟执行。例如，在 `fn1() || fn2()` 中，如果 `fn1` 返回布尔值 `true`，则永远不会执行 `fn2` 的调用。同样，如果将 `||` 替换为 `&&`，当 `fn1` 返回布尔值 `false` 时，`fn2` 的调用也不会发生。程序从这种短路求值中受益；我们避免了不必要的表达式或函数求值，这有助于提高性能。我们经常依赖这种短路求值来保证代码正确性。

虽然 Java 在求值逻辑运算符时使用延迟或正则求值顺序，但在求值方法参数时使用急切或应用顺序。在调用方法之前，会对传递给方法的所有参数进行完全求值。如果方法没有使用所有传递的参数，程序就浪费了时间和精力来执行它们。我们可以使用 lambda 表达式来推迟部分参数的执行。

Java 编译器在调用位置对参数列表中的 lambda 表达式和方法引用进行求值。这些的调用被推迟，直到它们在被传递的方法内部被显式调用。我们可以利用这种行为，通过在 lambda 中嵌入对它们的调用来推迟甚至避免方法调用
