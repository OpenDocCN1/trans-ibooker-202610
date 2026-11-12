

## 重构传统的 for 循环

现有代码库中充斥着大量的 `for` 循环。我们使用它们来遍历数据集合，也遍历一系列索引。为了说明如何将传统的命令式 `for` 循环重构为函数式风格代码，我们将看看如何使用 `for` 循环来计算一组数字的阶乘。但当然，我们将先从一个测试开始，以提供刚才讨论的安全网。

refactoring/fpij/FactorialTest.java

```java
public class FactorialTest {
    Factorial factorial;

    @BeforeEach
    public void init() {
        factorial = new Factorial();
    }

    @Test
    public void computeFactorial() {
        assertAll(
            () -> assertEquals(BigInteger.ONE, factorial.compute(1)),
            () -> assertEquals(BigInteger.TWO, factorial.compute(2)),
            () -> assertEquals(BigInteger.valueOf(6), factorial.compute(3)),
            () -> assertEquals(BigInteger.valueOf(120), factorial.compute(5))
        );
    }
}
```

该测试验证了尚未展示的 `Factorial` 类的 `compute` 方法是否返回给定上限值的正确阶乘值。现在让我们看看 `Factorial` 类，其中的 `compute` 方法采用命令式风格实现：

refactoring/fpij/Factorial.java

```java
public class Factorial {
    public BigInteger compute(long upTo) {
        BigInteger result = BigInteger.ONE;
        for (int i = 1; i <= upTo; i++) {
            result = result.multiply(BigInteger.valueOf(i));
        }
        return result;
    }
}
```

与传统的 for 循环用法类似，我们正在遍历从 1 到变量 upTo 中值的范围。你的任务：暂停阅读，运行测试，并验证它通过。然后将 compute 方法重构为函数式风格。验证测试通过后继续阅读。我们将在后续章节中将这些步骤称为：

### 暂停、重构、继续。

现在你回来了，让我们比较一下你采取的步骤和我这里建议的步骤。在尝试从命令式风格重构为函数式风格时，不总是清楚从哪里开始以及如何进行。一种有帮助的技术是先进行声明式思考，然后以函数式方式进行编程。

### 函数式风格 == 声明式风格 + 高阶函数的使用

函数式编程风格的一个关键优势在于其声明式特性。在进入函数式风格之前，花些时间进行声明式思考——这可以帮助你识别与命令式风格不同的代码实现中的一些关键要素。不要关注每个步骤是如何执行的，而是将问题视为一系列将数据从给定输入转换为所需输出的高级步骤。让我们深入探讨手头的问题，看看这种方法如何提供帮助。

要计算阶乘，我们处理从 1 到给定数字作为上限的一系列值。对于范围中的每个数字，我们需要其 BigInteger 表示。最后，我们累积 BigInteger 值的乘积——这是一个归约操作。手头的问题表现为一系列或管道：范围 => 映射 => 归约。我们可以重构代码以实现这些想法。

作为第一步，我们需要从 1 到给定数字获取一系列 long 值。快速搜索 JDK 显示，LongStream 接口有一个名为 rangeClosed 的静态方法，它将生成一个 LongStream 来遍历指定范围中的值。一旦我们有了 LongStream，我们希望将范围中的值转换为这些值的 BigInteger 表示。我们不能使用 map 方法，因为它期望将 LongStream 转换为另一个 LongStream，而不是 Stream<BigInteger>，因此我们将使用 mapToObj 方法。作为最后一步，我们可以使用 reduce 操作。让我们将这些步骤合并在代码中：

```java
public BigInteger compute(long upTo) {
    return LongStream.rangeClosed(1, upTo)
        .mapToObj(BigInteger::valueOf)
        .reduce(BigInteger.ONE, BigInteger::multiply);
}
```

除了使用 Stream API，我们还使用了方法引用而不是创建 lambda 表达式，使代码更加简洁和富有表现力。没有显式可变性、更少的杂乱、更强的表现力——这些都是我们可以从函数式风格中期望的所有好处。

## 重构更复杂的循环

传统的 for 循环允许我们以不同的步长值递增或递减范围。让我们来看看 LeapYears 类中的 countFrom1900 方法，它计算从 1900 年到给定年份的闰年数量。

让我们先从测试开始：

refactoring/fpij/LeapYearsTest.java

```java
public class LeapYearsTest {
    LeapYears leapYears;

    @BeforeEach
    public void init() {
        leapYears = new LeapYears();
    }

    @Test
    public void countFrom1900() {
        assertAll(
            () -> assertEquals(25, leapYears.countFrom1900(2000)),
            () -> assertEquals(27, leapYears.countFrom1900(2010)),
            () -> assertEquals(31, leapYears.countFrom1900(2025)),
            () -> assertEquals(49, leapYears.countFrom1900(2100)),
            () -> assertEquals(0, leapYears.countFrom1900(1800))
        );
    }
}
```

这些测试验证 countFrom1900 方法是否为传入的不同年份返回正确结果。

现在让我们看看我们想要重构的命令式风格代码：

refactoring/fpij/LeapYears.java

```java
public class LeapYears {
    public int countFrom1900(int upTo) {
        int numberOfLeapYears = 0;

        for (int i = 1900; i <= upTo; i += 4) {
            if (Year.isLeap(i)) {
                numberOfLeapYears++;
            }
        }
        return numberOfLeapYears;
    }
}
```

由于闰年最多每四年出现一次，`for` 循环以 4 的增量遍历索引。然后它使用 JDK 库中的 `isLeap` 方法来检查一年是否为闰年，并相应地递增计数。

### 暂停、重构、继续。

我们需要为以任意增量遍历的 `for` 循环找到函数式风格的等价物。让我们用函数式的视角更仔细地看看 `for` 语句：

```java
for(int i = 1900; i <= upTo; i += 4) {
```

读起来就像

```java
for(seedValue, predicate to check upper limit, step function)
```

`for` 循环以初始值开始，它可以使用 `Predicate` 来检查循环是否应继续，并使用函数来递增索引值。现在我们需要在 JDK 中查找这样的函数。事实证明，`IntStream` 正好有我们要找的东西——即 `iterate` 函数。让我们再次思考，使用 `iterate` 函数，`for` 循环在函数式风格中会是什么样子：

```java
iterate(1900, year -> year <= upTo, year -> year + 4)
```

循环可以从 1900 的值开始。第二个参数是一个 Predicate，它可以检查在迭代过程中年份是否在 upTo 的值范围内。第三个参数是一个 Function，它可以将 year 的值增加所需的步长值 4。iterate 函数将生成一个数字流，从种子值开始，按给定增量递增，直到检查 Predicate 中的值。我们可以使用这个流来重构命令式风格代码：

```java
public int countFrom1900(int upTo) {
    return (int) IntStream.iterate(
        1900, year -> year <= upTo, year -> year + 4)
        .filter(Year::isLeap)
        .count();
}
```

一旦我们得到了 for 的函数式等价物，其余步骤就非常直接了。命令式风格中更复杂的循环变成了一个优雅的函数式管道。接下来，我们将把一个无界命令式风格循环重构为另一个优雅的函数式风格迭代。

## 重构无界循环

传统的 for 循环虽然复杂，但非常通用。不指定上限是一个很好的功能，使执行循环直到满足条件变得更加容易。让我们看看 countFrom1900 方法的一个变体，它利用了此功能。哦，是的，当然，先写测试。

refactoring/fpij/LeapYearsUnboundedTest.java

```java
public class LeapYearsUnboundedTest {
    LeapYearsUnbounded leapYearsUnbounded;

    @BeforeEach
    public void init() {
        leapYearsUnbounded = new LeapYearsUnbounded();
    }

    @Test
    public void count() {
        assertAll(
            () -> assertEquals(25,
                leapYearsUnbounded.countFrom1900(year -> year <= 2000)),
            () -> assertEquals(27,
                leapYearsUnbounded.countFrom1900(year -> year <= 2010)),
            () -> assertEquals(31,
                leapYearsUnbounded.countFrom1900(year -> year <= 2025)),
            () -> assertEquals(49,
                leapYearsUnbounded.countFrom1900(year -> year <= 2100)),
            () -> assertEquals(0,
                leapYearsUnbounded.countFrom1900(year -> year <= 1800))
        );
    }
}
```

测试不是向 countFrom1900 方法传递年份的上限，而是传递一个 lambda 来检查给定年份，以确定是否已达到上限。

