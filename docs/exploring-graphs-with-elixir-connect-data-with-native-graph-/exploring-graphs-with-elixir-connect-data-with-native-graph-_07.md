## 重构为函数式风格

过去，Java 程序员只能使用命令式编程风格编写面向对象代码。如今，他们可以使用命令式风格或函数式风格编写面向对象代码。编写代码时拥有这样的选择很不错。函数式风格代码并不总是理想，例如，在处理副作用或异常时，正如我们在上一章中所见。但它通过减少偶然复杂性，使代码更易于阅读和理解，从而优于命令式风格。

当我们坐下来编写新代码时，可以运用我们的函数式编程技能。但许多企业应用程序并非是绿地项目。我们常常被抛入一个已经存在多年且原作者已不在公司的代码库中。

世界上有大量的命令式风格 Java 代码。主要有两个原因。首先，命令式风格长期以来是 Java 的主流方式，大多数程序员对这种风格非常熟悉，因为很长一段时间里这是唯一的选择。其次，尽管 Java 8 之后就支持函数式编程，但我们很自然地会回到自己熟悉且惯用的方式上，因为那些方式更熟悉。指望我们某天晚上还在写命令式风格代码，第二天早上就能同样轻松地写出函数式风格代码，这不现实。因此，过去几年中编写了更多新的命令式风格代码。

代码能正常工作，所以你可能会问：何必费心重构到函数式风格。最简单的原因是减少偶然复杂性，使代码更易于理解，并首先，让它更明显地正确。使用函数式风格时，偶然复杂性被隐藏起来，进入底层代码库，不在我们视野中，我们的代码看起来更像问题陈述本身。这减少了阻抗失配和代码维护成本。这听起来很棒，但这是否意味着我们必须重构所有现有代码？当然不是。

将所有现有命令式风格代码改为函数式风格代码既不实际，我也绝不建议这样做。不要为了改变而改变，踏上修改代码的使命。有些时候我们可以，也应该将命令式风格代码重构为函数式风格代码。我们应务实，而不是教条。

没有其他修改理由的代码模块，不值得花时间将其从命令式风格重构为函数式风格。无论该代码多么复杂，没有人碰它，因此明智的做法是把时间和精力花在其他地方。

对于已经存在一段时间的应用程序，当你触及一段代码时——无论是增强现有功能、添加新功能还是修复 Bug——都是重构的好时机。你正在修改的代码很可能会继续变化，它值得变得不那么复杂、更易读且维护成本更低。

编写新代码时（无论是在绿地项目还是现有应用程序中），我们可能会因为命令式风格很熟悉而倾向于使用它。这完全没问题。我们不是在比赛——不要让任何人，尤其是你自己，因为你用命令式风格编写代码而嘲笑你。我非常赞同这条格言："先让它运行起来，然后尽快把它做得更好"。如果你觉得用命令式风格表达想法更容易，那就这样吧。一旦代码能正常运行，并且周围有良好的测试，就将新代码重构为函数式风格，并验证测试通过。你捕捉了逻辑，然后降低了复杂性，这很高尚。随着你对函数式风格经验的积累，你会发现自己会逐渐优先使用那种风格编写代码，而不是先写命令式风格再重构。给你自己和同事一些时间来完成这个过渡。

你可能已经意识到，用函数式风格编写代码不是学习某一套特定语法。这是一种范式转变，需要改变我们的思维方式，并涉及探索、尝试想法、原型设计和耐心。将代码从命令式风格重构为函数式风格是获得这种实践的最佳方式之一。

在本章中，我们将重构每天遇到的许多常见编程任务的代码，将其从命令式风格转换为函数式风格。通过处理这些任务的重构步骤，将帮助你思考如何用函数式风格编程。在查看每个问题时，尝试代码并用函数式风格重写解决方案。然后继续阅读，将你的思路与概述的细节和生成的代码进行比较。

在深入第一个重构任务之前，让我们讨论一下重构的关键步骤——拥有一套自动化测试。

### 为重构构建安全网

重构应该在不影响外部行为的前提下，改进代码的内部结构。我们往往急于获取重构带来的好处，但也必须意识到风险。担心更改后代码行为不同是合理的。由于代码中隐藏的复杂性，我们可能并未完全理解要更改的代码。无论函数式风格代码看起来多么迷人，如果它不能产生与其替换的命令式风格代码相同的结果，那就毫无用处。自动化测试是重构的安全网。

一旦有了通过的测试，每次更改后我们都可以快速验证之前能正常工作的代码仍然以相同方式工作。编写新代码时，无论是命令式还是函数式风格，都要编写测试。即使我们一开始就用函数式风格编写代码，也肯定会找到改进代码设计和质量的方法。测试会让我们非常有信心，重构没有破坏现有功能。

遗留代码带来额外挑战，因为它可能不包含用于验证行为的自动化测试。如果可能，在重构前为代码编写自动化单元测试。如果做不到，请确定触及你计划重构的代码的集成测试。如果这些选项都不存在，则依靠验证代码行为的手动测试。一旦确定能以最快、最可靠方式提供反馈的测试，就可以开始重构代码了。

本章中每个示例，我们都将从一个任务的命令式风格代码及一组验证代码行为的单元测试开始。跟着练习时，同时输入代码和相关测试，并验证测试通过。然后将代码重构为函数式风格。最后，验证测试仍然通过，以确认函数式风格代码产生与其替换的命令式风格代码相同的结果。

让我们从第一个重构任务开始——我们将从一个相对简单的问题开始热身。





### 重构传统的 for 循环

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

#### 暂停、重构、继续。

现在你回来了，让我们比较一下你采取的步骤和我这里建议的步骤。在尝试从命令式风格重构为函数式风格时，不总是清楚从哪里开始以及如何进行。一种有帮助的技术是先进行声明式思考，然后以函数式方式进行编程。

#### 函数式风格 == 声明式风格 + 高阶函数的使用

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

### 重构更复杂的循环

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

#### 暂停、重构、继续。

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

### 重构无界循环

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





让我们看看可重构的命令式风格版本：

```java
interface Continue {
    boolean check(int year);
}

public class LeapYearsUnbounded {
    public int countFrom1900(Continue shouldContinue) {
        int numberOfLeapYears = 0;

        for(int i = 1900;; i += 4) {
            if(!shouldContinue.check(i)) {
                break;
            }

            if(Year.isLeap(i)) {
                numberOfLeapYears++;
            }
        }

        return numberOfLeapYears;
    }
}
```

该方法使用名为 Continue 的接口，其中包含 check 方法，而 countFrom1900 接收实现该接口的实例作为参数。在方法内部，循环是无界的，并使用传入的 Continue 来确定循环是否应继续或跳出。

暂停，重构，继续。

作为第一个重构机会，我们可以将自定义的 Continue 接口替换为 JDK 的函数式接口 Predicate。

无界 for 循环的函数式等价写法不需要 Predicate，因为它不需要检查上界。IntStream 的 iterate 方法的创建者预料到了这种用法，并提供了一个不带 Predicate、只接收两个参数而非三个参数的重载版本。

命令式风格的 for 循环使用 break 语句退出循环。在函数式编程中没有 break，但我们有 takeWhile—请参阅 Terminating Iterations。我们可以通过将 iterate 调用与 takeWhile 调用链接起来，来重构命令式风格的无界循环，如下所示：

```java
public int countFrom1900(Predicate<Integer> shouldContinue) {
    return (int) IntStream.iterate(1900, year -> year + 4)
            .takeWhile(shouldContinue::test)
            .filter(Year::isLeap)
            .count();
}
```

我们少了一个接口，方法变得简洁且富有表达力。重构大获成功。在下一个任务中，我们将把使用 for-each 循环的命令式代码重构为函数式风格。

### 重构 for-each

作为 Java 程序员，我们已经喜欢并广泛使用所谓的 for-each 语法，其形式为 for(Type variable: iterable)。当我们不需要索引且只关心集合中的元素时，它比传统的 for 循环简单得多。但在两种 for 循环版本中使用 break 和 continue 并不罕见。结果是冗长的代码，长期维护起来费力。让我们尝试重构使用 for-each 的代码，将其改为函数式风格。

延续我们的传统，让我们先从测试开始：

refactoring/fpij/AgencyTest.java

```java
public class AgencyTest {
    Agency agency;

    @BeforeEach
    public void init() {
        agency = new Agency();
    }

    @Test
    public void isChaperoneRequired() {
        assertAll(
            () -> assertTrue(agency.isChaperoneRequired(
                Set.of(new Person("Jake", 12)))),
            () -> assertTrue(agency.isChaperoneRequired(
                Set.of(new Person("Jake", 12), new Person("Pam", 14)))),
            () -> assertTrue(agency.isChaperoneRequired(
                Set.of(new Person("Shiv", 8),
                new Person("Sam", 9), new Person("Jill", 11)))),
            () -> assertFalse(agency.isChaperoneRequired(
                Set.of(new Person("Jake", 12), new Person("Pam", 18)))),
            () -> assertFalse(agency.isChaperoneRequired(Set.of()))
        );
    }
}
```

测试使用了 Person，它可以实现为 record（记录类，或旧版 Java 中的类）。

refactoring/fpij/Person.java

```java
public record Person(String name, int age) {}
```

机构负责决定团体出行是否需要陪同人员。做出该决定的逻辑位于 Agency 类的 isChaperoneRequired 方法中：

refactoring/fpij/Agency.java

```java
public class Agency {
    public boolean isChaperoneRequired(Set<Person> people) {
        boolean required = true;

        if(people.size() == 0) {
            required = false;
        } else {
            for(var person: people) {
                if(person.age() >= 18) {
                    required = false;
                    break;
                }
            }
        }

        return required;
    }
}
```

该方法的功能相当简单，但实现起来却充斥着大量冗余代码。如果组中有人年满 18 岁或以上，或者组为空，则不需要陪同人员。这段代码可以进行一些重构，以减少杂乱并消除可变性。

暂停，重构，继续。

我们知道，我们可以使用 `stream` 方法遍历集合，而不是使用 `for-each`。给定一组人员，我们想知道列表中是否有人年龄超过 17 岁，以决定是否需要陪同人员。我们可以为此使用 `Stream` 的 `noneMatch`，如下所示：

```java
public boolean isChaperoneRequired(Set<Person> people) {
    return people.size() > 0 &&
           people.stream()
                .noneMatch(person -> person.age() >= 18);
}
```

这段代码的简洁性难以匹敌。在我们迄今看到的重构示例中，我们在从命令式风格转换为函数式风格的同时，都成功保持了逻辑不变。但这并非总是可行，有时我们可能需要重新设计算法或逻辑，正如我们接下来将看到的那样。

### 重构以重新设计逻辑

有时在重构为函数式风格时，我们可能需要退后一步，重新思考逻辑或算法，而不仅仅是试图从命令式风格映射到函数式风格。如果在这种情况下不采用替代方法，要么我们可能在创建函数式代码时遇到问题，要么重构后的版本可能过于复杂，让我们渴望更好的解决方案。本节将探讨一个这样的问题——这是由一位对解决此问题感兴趣且在创建函数式等价物时遇到困难的开发人员提出的。

给定一个字符串，我们想找到字符串中在其他任何位置重复出现的第一个字母。例如，在字符串 hellothere 中，第一个重复的字母是 h，即使字母 l 和 e 也被重复。在处理此类问题时，尤其是在希望重构代码时，测试是澄清细节和消除任何歧义的最佳方式之一。以下是该问题的一些测试：

refactoring/fpij/FirstRepeatedLetterTest.java

```java
public class FirstRepeatedLetterTest {
    FirstRepeatedLetter firstRepeatedLetter;

    @BeforeEach
    public void init() {
        firstRepeatedLetter = new FirstRepeatedLetter();
    }

    @Test
    public void findFirstRepeating() {
        assertAll(
            () -> assertEquals('l', firstRepeatedLetter.findIn("hello")),
            () -> assertEquals('h', firstRepeatedLetter.findIn("hellothere")),
            () -> assertEquals('a', firstRepeatedLetter.findIn("magicalguru")),
            () -> assertEquals('\0', firstRepeatedLetter.findIn("once")),
            () -> assertEquals('\0', firstRepeatedLetter.findIn(""))
        );
    }
}
```

让我们看看通过上述测试的命令式风格代码：

```java
public class FirstRepeatedLetter {
    public char findIn(String word) {
        char[] letters = word.toCharArray();

        for(char candidate: letters) {
            int count = 0;

            for(char letter: letters) {
                if(candidate == letter) {
                    count++;
                }
            }

            if(count > 1) {
                return candidate;
            }
        }

        return '\0';
    }
}
```

我们在外层循环中遍历给定单词中的字母。在内层循环中，我们检查外层循环选取的字母在给定单词的字母中出现的次数。一旦意识到某个字母已重复，我们就将其作为结果返回。这段代码冗长、嘈杂且难以阅读，但一旦我们看清其中隐藏的逻辑，就会同意它相当直接。当然，我们希望代码更易于阅读和理解，因此我们希望将其重构为函数式风格。让我们试一下。

暂停，重构，继续。

如果我们直接尝试将两个循环翻译成函数式等价写法，那顶多也是一团混乱。相反，我们可以退后一步，重新思考解决此问题的方法。在此过程中，我们可以利用函数式管道的惰性求值能力。

如果字母在给定单词中最后一次出现的位置大于其第一次出现的位置，则该字母重复。如果未重复，其最后出现位置将与第一次出现位置相同。从给定单词中，我们可以筛选出重复的字母。乍一看，这似乎相当低效。既然我们只需要第一个，为什么要费力获得所有重复字母呢？但得益于惰性求值（请参见 Chapter 7, Being Lazy），一旦找到第一个重复字母，计算就可以停止，后续字母无需处理。我们可以通过使用 findFirst 方法来实现这一点。让我们应用这些想法来重构代码：

```java
public char findIn(String word) {
    return Stream.of(word.split(""))
        .filter(letter -> word.lastIndexOf(letter) > word.indexOf(letter))
        .findFirst()
        .map(letter -> letter.charAt(0))
        .orElse('\0');
}
```

这一次，代码同样富有表达力、简洁且易于阅读。如果您愿意，可以将 lambda 表达式移动到一个名为 isDuplicated 的独立函数中，以使代码更易读一些。




Stream API 可轻易用于处理内存中的数据集合。接下来，我们将了解如何处理文件中的数据。

### 重构文件处理

从文件读取数据是编程中常见的操作，我相信你肯定无数次这样做过。但在过去，执行该操作的代码相当冗长且混乱。让我们来看一个读取文件内容并统计其中单词出现次数的函数。然后我们将重构为使用函数式风格。

让我们先从一小组测试开始，验证 `countInFile` 函数是否按预期工作：

```
refactoring/fpij/WordCountTest.java

public class WordCountTest {
    WordCount wordCount;

    @BeforeEach
    public void init() {
        wordCount = new WordCount();
    }

    @Test
    public void count() {
        assertAll(
            () -> assertEquals(2,
                wordCount.countInFile("public", "fpij/WordCount.java")),
            () -> assertEquals(1,
                wordCount.countInFile("package", "fpij/WordCount.java"))
        );
    }
}
```

测试表明，被测方法接收一个搜索单词和要搜索的文件路径。下面是急需重构的命令式风格代码：

```
refactoring/fpij/WordCount.java

```java
public class WordCount {
    public long countInFile(
        String searchWord, String filePath) throws IOException {
        long count = 0;

        BufferedReader bufferedReader =
            new BufferedReader(new FileReader(filePath));

        String line = null;

        while ((line = bufferedReader.readLine()) != null) {
            String[] words = line.split(" ");

            for (String word : words) {
                if (word.equals(searchWord)) {
                    count++;
                }
            }
        }

        return count;
    }
}
```
```

该函数通过将读取的内容与 null 进行比较来检查文件中是否有更多行可读——呃，这相当难看。我们需要更简便的方法来读取和处理文件内容，幸运的是，JDK 提供了较新的函数，为该任务带来了函数式风格的魅力。

暂停，重构，继续。

给定文件内容，我们想要查找其中某个单词的出现次数。可以使用 filter 操作检查每个单词是否是我们要找的，随后使用 reduce 操作计算出现次数。但是，要采取这些步骤，我们需要文件内容的流。

JDK `java.nio.file` 包中的 **Files** 类为此提供了合适的函数。`lines` 方法返回一个 `Stream<String>`，我们可以应用钟爱的 **Stream** 操作来处理文件内容。让我们使用该函数重构 `countInFile` 方法：

```
public long countInFile(
    String searchWord, String filePath) throws IOException {
    return Files.lines(Paths.get(filePath))
        .flatMap(line -> Stream.of(line.split(" ")))
        .filter(word -> word.equals(searchWord))
        .count();
}
```

`lines` 方法创建一个内部迭代器，一次处理文件的一行内容。但我们需要处理文件中的单词，而不仅仅是行。可以调用 `split` 将一行拆分为单词，以便对单词进行进一步处理。由于将一行拆分为单词是一对多映射，我们必须使用 `flatMap` 而不是 `map` 来将数据从行流转换为单词流——参见 [使用 map 还是 flatMap]。得到单词流后，可以 `filter` 然后最终计数。

除了代码简洁且富有表现力之外，作为额外的好处，我们不必再与 `null` 打交道，这本身就是一大胜利。

接下来，我们将处理一个数据位于 `Map` 中的示例。

### 重构数据分组操作

根据某些标准对数据进行分组是业务应用中常见的操作。我们可能希望根据工作地点对员工分组，根据业务单元对项目分组，根据最新收入对产品分组，等等。在命令式风格中，为分组创建 Map 并为键添加值可能会变得冗长，并且还可能涉及一些低效之处。你将看到执行此类处理的命令式风格代码及其等效的函数式风格代码之间存在鲜明对比。

为了深入了解重构执行分组操作的代码，我们将使用一组游戏中的假设分数，以玩家姓名和分数的键值对形式提供。让我们先从一些测试开始，验证名为 namesForScores 的函数是否根据分数对数据进行分组，并为每个分数创建关联姓名列表。

```
refactoring/fpij/ScoresTest.java

public class ScoresTest {
    Scores scores;

    @BeforeEach
    public void init() {
        scores = new Scores();
    }

    @Test
    public void namesForScores() {
        assertAll(
            () -> assertEquals(Map.of(), scores.namesForScores(Map.of())),
            () -> assertEquals(
                Map.of(1, Set.of("Jill")), scores.namesForScores(Map.of("Jill", 1))),
            () -> assertEquals(
                Map.of(1, Set.of("Jill"), 2, Set.of("Paul")),
                scores.namesForScores(Map.of("Jill", 1, "Paul", 2))),
            () -> assertEquals(
                Map.of(1, Set.of("Jill", "Kate"), 2, Set.of("Paul")),
                scores.namesForScores(Map.of("Jill", 1, "Paul", 2, "Kate", 1)))
        );
    }
}
```

要根据分数对数据进行分组，我们需要遍历给定 Map 的键集。对于每个玩家，检查其分数值是否已存在于目标映射中。如果存在，则将姓名添加到现有键的值集合中。如果分数尚未作为键存在，则创建一个包含玩家姓名的新集合，并将该集合作为新键的值添加到目标映射中。下面是执行这些琐碎操作的代码：

```
refactoring/fpij/Scores.java

public class Scores {
    public Map<Integer, Set<String>> namesForScores(
        Map<String, Integer> scores) {

        Map<Integer, Set<String>> namesForScores = new HashMap<>();

        for (String name : scores.keySet()) {
            int score = scores.get(name);

            Set<String> names = new HashSet<>();
            if (namesForScores.containsKey(score)) {
                names = namesForScores.get(score);
            }

            names.add(name);
            namesForScores.put(score, names);
        }

        return namesForScores;
    }
}
```

大量垃圾变量、显式突变、低级操作，等等，不胜枚举——令人不寒而栗。

暂停，重构，继续。

namesForScores 方法的整个主体可以重构为仅三行代码：

```
public Map<Integer, Set<String>> namesForScores(
    Map<String, Integer> scores) {
    return scores.keySet()
        .stream()
        .collect(groupingBy(scores::get, toSet()));
}
```

我们使用 stream 方法遍历键集，并要求根据每个名称的分数值对值进行分组。然后，我们通过将 toSet 返回的 Collector 提供给 groupingBy 函数，要求将名称放入集合中。该代码威力惊人。

在每个示例中，我们都在处理日益复杂的问题，每个示例在重构为函数式风格时都提供了一些值得考虑的新思路。下一个示例是本重构系列任务中的最后一个，它有一个很好的额外复杂度，并带来了一组强大的解决方案——让我们来看看。

### 重构嵌套循环

本节中你看到的命令式风格代码是由一位开发者发送给我的，他有兴趣应用函数式编程，并好奇如何将其重构为函数式风格。该代码创建正数 (a, b, c) 的勾股数[19]，满足条件 a² + b² = c²。

让我们先从一些测试开始，验证 PythagoreanTriples 类的 compute 方法的结果：

```
refactoring/fpij/PythagoreanTriplesTest.java

public class PythagoreanTriplesTest {
    PythagoreanTriples pythagoreanTriples;

    @BeforeEach
    public void init() {
        pythagoreanTriples = new PythagoreanTriples();
    }

    @Test
    public void compute() {
        assertAll(
            () -> assertEquals(List.of(), pythagoreanTriples.compute(0)),
            () -> assertEquals(List.of(triple(3, 4, 5)),
                pythagoreanTriples.compute(1)),
            () -> assertEquals(
                List.of(triple(3, 4, 5), triple(8, 6, 10), triple(5, 12, 13)),
                pythagoreanTriples.compute(3)),
            () -> assertEquals(
                List.of(triple(3, 4, 5), triple(8, 6, 10),
                    triple(5, 12, 13), triple(15, 8, 17),
                    triple(12, 16, 20)),
                pythagoreanTriples.compute(5))
        );
    }
}
```



接下来，使用命令式风格实现的 `compute` 方法如下：

```
refactoring/fpij/PythagoreanTriples.java

record Triple(int a, int b, int c) {
public static Triple triple(int a, int b, int c) {
return new Triple(a, b, c);
}

public String toString() { return String.format("%d %d %d", a, b, c); }
}

public class PythagoreanTriples {
public Triple getTripleEuclidsWay(int m, int n) {
int a = m * m - n * n;
int b = 2 * m * n;
int c = m * m + n * n;

return triple(a, b, c);
}

public List<Triple> compute(int numberOfValues) {
if(numberOfValues == 0) {
return List.of();
}

List<Triple> triples = new ArrayList<>();
int count = 1;

for(int m = 2; ; m++) {
for(int n = 1; n < m; n++) {
triples.add(getTripleEuclidsWay(m, n));
count++;

if(count > numberOfValues)
break;
}

if(count > numberOfValues)
break;
}
return triples;
}
}
```

我们将勾股数三元组存储在由名为 Triple 的 Java Record 表示的元组中。该记录提供了一个名为 triple 的静态方法，以便使用 triple(...) 而不是 new Triple(...) 来创建实例，从而略增流畅度。

getTripleEuclidsWay 方法使用欧几里得算法，为给定的正整数 m 和 n（其中 m > n）创建一个勾股数三元组。

compute 方法以我们期望创建的三元组数量作为参数。然后，它从 2 开始遍历 m 的值。对于 1 到 m - 1 之间的 n 值，它使用 getTripleEuclidsWay 方法来计算所需数量的三元组。

除了冗长之外，代码中真正有异味的部分是两个检查，用于判断是否已计算出所需数量的值。

暂停，重构，然后继续。

对于每个 m 值，我们都会创建许多三元组值。你明白了；这是一个一对多的问题，根据我们在《何时使用 map 与 flatMap》中的讨论，你知道这是一个适合使用 flatMap 的问题。让我们利用它将代码重构为函数式风格：

```
public List<Triple> compute(int numberOfValues) {
 return Stream.iterate(2, e -> e + 1)
 .flatMap(m -> IntStream.range(1, m)
 .mapToObj(n -> getTripleEuclidsWay(m, n)))
 .limit(numberOfValues)
 .toList();
}
```

太棒了。

我们为 m 创建一个从 2 开始的无界/无限值流。对于每个 m 值，我们创建一系列 *m - 1* 个三元组。最后，我们要求流将值的数量限制为参数 *numberOfValues* 中的所需值，并将值打包为一个列表。

这段代码中最大的挑战是将其识别为 *flatMap* 问题。一旦做到这一点，其余部分相对容易实现。

说到识别，让我们来讨论一下在重构现实世界代码时可以借鉴的一些常见模式。

### 现实世界中的重构

本章中你看到的示例可能会让你有信心在正在开发的应用程序中重构自己的代码。不幸的是，现实世界中我们不得不处理的代码，很少看起来是重构的友好候选对象。重构代码的旅程可能令人生畏，有时甚至令人却步。挫折感可能会让我们感到绝望，并诱使我们半途而废。但请记住，这不必是一蹴而就的努力。

大爆炸式重构可能导致大爆炸式失败。相反，应逐步重构。你可以首先使用 Stream、IntStream 等内部迭代器，将命令式循环转换为函数式迭代。if 条件可以重构为 filter()，循环内部的操作可以重构为 map()。此类更改可能有助于我们减少代码中的某些复杂性，使其更易读，并向更函数式风格的代码迈进。即使代码尚未转变为最优雅、完全函数式的代码，这样做仍然可能是值得的。

当你将代码重构为函数式风格时，请寻找一些常见的重构模式。当你知道要映射的常见模式时，可以快速识别出要使用的合适函数。请参阅表格，了解常见命令式代码结构的一些函数式替代方案。

| 命令式风格 | 函数式替代 |
| :--- | :--- |
| 常规 for 循环 | IntStream、LongStream 等的 range() 或 rangeClosed() |
| 不规则 for 循环 | Stream、IntStream 等的 iterate() |
| 循环中的 break | takeWhile() |
| for-each 循环 | Stream 的 of() 或 Collection 的 stream() |
| 嵌套循环 | Stream 的 flatMap() |
| 带 continue 的 if 块 | Stream 的 filter() |
| 数据累积 | reduce()、collect，或合适的专用 reduce，如 sum()、average() 等 |
| 获取选定数量的匹配值 | 结合 filter() 使用 limit() |
| 获取匹配值 | 结合 filter() 使用 findFirst，并可选使用 skip() 或 dropWhile() |
| 布尔结果的短路循环 | anyMatch()、allMatch()、noneMatch() |

重构代码时，请将表格作为参考。在过程中，如果你发现代码中出现其他此类模式，请记下它们并与团队分享，以让他们的旅程更加顺畅。

在重构常见任务方面，我们已经涵盖了大量内容。每个问题都让你有机会更深入地探索使用函数式风格编写代码。休息一下；在进入下一章之前，先回顾并思考这些示例。

### 总结

我们研究了若干用命令式风格编写的示例，并将它们重构为函数式风格。该旅程的第一步是进行声明式思考，然后寻找函数来实现向函数式风格的一系列转换。一旦掌握了要采取的步骤，请寻找（在 JDK 中）可以委托这些步骤的函数。有时，在将代码重构为函数式风格之前，我们必须后退一步并重新思考算法。通过练习，你会变得更好、更自在，并踏上减少遗留代码中偶然复杂性的道路。

在下一章中，我们将讨论几种编写更高质量函数式代码的方法。

### 脚注

[19] https://en.wikipedia.org/wiki/Pythagorean_triple
