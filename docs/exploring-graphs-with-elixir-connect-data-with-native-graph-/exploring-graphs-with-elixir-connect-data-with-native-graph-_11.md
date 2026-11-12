## 附录 2

## 语法概览

在本书中，我们一直在使用函数式接口、Lambda 表达式、方法引用和构造器引用的新语法。本附录是语法快速参考，选用了本书各部分的示例代码。

### 定义函数式接口

```java
@FunctionalInterface
public interface TailCall<T> {
    TailCall<T> apply();

    default boolean isComplete() { return false; }

    //...
}
```

函数式接口必须有一个抽象方法（未实现的方法）。它可以有零个或多个默认方法或已实现的方法。它也可以有静态方法。

### 创建无参数 Lambda 表达式

```
lazyEvaluator(() -> evaluate(1), () -> evaluate(2));
```

如果 Lambda 表达式不接受参数，则围绕空参数列表的括号 () 是必需的。-> 将参数与 Lambda 表达式的主体分隔开。

### 创建单参数 Lambda 表达式

```
friends.forEach((final String name) -> System.out.println(name));
```

Java 编译器可以根据上下文推断 Lambda 表达式的类型。在某些上下文不足以推断类型或我们希望获得更好清晰度的情况下，可以在参数名称前指定类型。

### 推断 Lambda 表达式的参数类型

```
friends.forEach((name) -> System.out.println(name));
```

如果我们不提供参数类型，Java 编译器会尝试推断参数类型。使用推断类型更简洁，所需努力更少，但如果我们为某个参数指定了类型，则必须为 Lambda 表达式中的所有参数指定类型。

### 对单参数推断类型省略括号

```
friends.forEach(name -> System.out.println(name));
```

如果 Lambda 表达式只接受一个参数且其类型被推断，则参数周围的括号 () 是可选的。我们可以写 `name -> ...` 或 `(name) -> ...`；倾向于使用前者，因为它更简洁。

### 创建多参数 Lambda 表达式

```
friends.stream()
.reduce((name1, name2) ->
name1.length() >= name2.length() ? name1 : name2);
```

如果 Lambda 表达式接受多个参数或不接受参数，则围绕参数列表的括号 () 是必需的。

### 调用带混合参数的方法

```
friends.stream()
.reduce("Steve", (name1, name2) ->
name1.length() >= name2.length() ? name1 : name2);
```

方法可以将常规类、基本类型和函数式接口作为参数的混合体。方法的任何参数都可以是函数式接口，我们可以传递 Lambda 表达式或方法引用作为参数。

### 存储 Lambda 表达式

```
final Predicate<String> startsWithN = name -> name.startsWith("N");
```

为了便于重用并避免重复，我们通常希望将 Lambda 表达式存储在变量中。

### 创建多行 Lambda 表达式

```
FileWriterEAM.use("eam2.txt", writerEAM -> {
writerEAM.writeStuff("how");
writerEAM.writeStuff("sweet");
});
```

我们应该保持 Lambda 表达式简短，但很容易偷偷加入几行代码。为此我们需要使用花括号 {}，如果 Lambda 表达式预期返回值，则需要使用 return 关键字。

### 返回 Lambda 表达式

```
public static Predicate<String> checkIfStartsWith(final String letter) {
    return name -> name.startsWith(letter);
}
```

如果方法的返回类型是函数式接口，我们可以在其实现中返回一个 Lambda 表达式。

### 从 Lambda 表达式返回 Lambda 表达式

```
final Function<String, Predicate<String>> startsWithLetter =
letter -> name -> name.startsWith(letter);
```

我们可以构建自身返回 Lambda 表达式的 Lambda 表达式。此处 Function 接口的实现接受一个 String letter 并返回一个符合 Predicate 接口的 Lambda 表达式。

### 闭包中的词法作用域

```java
public static Predicate<String> checkIfStartsWith(final String letter) {
    return name -> name.startsWith(letter);
}
```

在 Lambda 表达式内部，我们可以访问封闭方法作用域中的变量。例如，`checkIfStartsWith` 中的变量 `letter` 在 Lambda 表达式内被访问。绑定到封闭作用域变量的 Lambda 表达式称为 _闭包_。

### 传递实例方法的方法引用

```
friends.stream()
.map(String::toUpperCase);
```

如果 Lambda 表达式直接将参数作为目标路由到简单的方法调用，则可以用方法引用替换 Lambda 表达式。前面的示例代码等价于此：

```
friends.stream()
.map(name -> name.toUpperCase());
```

### 传递静态方法的方法引用

```
str.chars()
.filter(Character::isDigit);
```

如果 Lambda 表达式直接将参数作为参数路由到静态方法，则可以用方法引用替换 Lambda 表达式。前面的示例代码等价于此：

```
str.chars()
.filter(ch -> Character.isDigit(ch));
```

### 传递另一实例上方法的方法引用

```
str.chars()
.forEach(System.out::println);
```

如果 Lambda 表达式直接将参数作为参数路由到另一实例上的方法，则可以用方法引用替换 Lambda 表达式；例如 System.out 上的 println。前面的示例代码等价于此：

```
str.chars()
.forEach(ch -> System.out.println(ch));
```

### 传递带参数方法的方法引用

```
people.stream()
.sorted(Person::ageDifference)
```

如果 Lambda 表达式直接将第一个参数作为方法调用的目标，并将剩余参数作为该方法的参数，则可以用方法引用替换 Lambda 表达式。前面的示例代码等价于此：

```
people.stream()
.sorted((person1, person2) -> person1.ageDifference(person2))
```

### 使用构造器引用

```
Supplier<Heavy> supplier = Heavy::new;
```

与调用构造器不同，我们可以要求 Java 编译器从简洁的构造器引用语法创建对适当构造器的调用。这些与方法引用非常相似，只不过它们引用构造器并导致对象实例化。前面的示例代码等价于此：

```
Supplier<Heavy> supplier = () -> new Heavy();
```

### 函数组合

```
symbols.map(StockUtil::getPrice)
.filter(StockUtil.isPriceLessThan(500))
.reduce(StockUtil::pickHigh)
.get();
```

我们可以像本示例那样组合函数，通过一系列操作转换对象。在函数式编程风格中，函数组合或链式调用是实现关联操作的强大构造。

## Web 资源

- **切股问题**  
  http://en.wikipedia.org/wiki/Cutting_stock_problem

  一种可使用备忘录技术的优化问题。

- **依赖倒置原则**  
  http://c2.com/cgi/wiki?DependencyInversionPrinciple

  描述了一种通过将类与抽象（接口）而非其实现耦合来实现可扩展性的方式。

- **不要重复自己**  
  http://c2.com/cgi/wiki?DontRepeatYourself

  我让读者自行参考该 URL，以体现 DRY 精神。

- **执行环绕方法模式**  
  http://c2.com/cgi/wiki?ExecuteAroundMethod

  描述了一种通过前置和后置操作来控制逻辑流的模式。

- **“Lambda：一窥内部”**  
  https://www.youtube.com/watch?v=C_QbkGU_lqY

  Brian Goetz 的一次演示。





### 贷出模式
https://wiki.scala-lang.org/display/SYGN/Loan
对 Scala 中贷出模式的讨论。

### MapReduce
http://research.google.com/archive/mapreduce.xhtml
“MapReduce：大规模集群上的简化数据处理”——由 Jeffrey Dean 和 Sanjay Ghemawat 撰写的论文，讨论了这种编程模型。

### 开闭原则
http://en.wikipedia.org/wiki/Open/closed_principle
描述了 Bertrand Meyer 的开闭原则，该原则规定软件模块应对扩展开放，但无需修改代码。

### 过早优化
http://c2.com/cgi/wiki?PrematureOptimization
一个讨论过早优化危害的网页。

### “告诉我，不要询问”
http://pragprog.com/articles/tell-dont-ask
一篇讨论“告诉我，不要询问”原则的专栏文章。

### “测试驱动多线程代码”
http://tinyurl.com/ab5up2w
关于线程安全单元测试的演示文稿中的代码示例。直接 URL 是 https://www.agiledeveloper.com/presentations/TestDrivingMultiThreadedCode.zip。

### 本书的网页
http://www.pragprog.com/titles/vsjava2e
本书的网页，包含完整的源代码清单。
