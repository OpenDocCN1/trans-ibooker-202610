
要执行，请使用 Java 11 运行 `java` 命令。Java 会即时编译源文件，并使用类路径中的类文件运行程序。任何可能出现的编译错误都会在控制台中显示：

```
$ java -version
openjdk version "11" 2018-09-25
OpenJDK Runtime Environment 18.9 (build 11+28)
OpenJDK 64-Bit Server VM 18.9 (build 11+28, mixed mode)

$ java HelloWorld.java
Hello World!
```

无需编译即可启动单文件程序非常方便，但有几个限制需要注意。在 Java 11 中，程序不能使用除 `java.base` 模块之外的任何外部依赖。此外，只能启动单文件程序。目前不支持从其他 Java 源文件调用方法，但如有需要，可以在同一文件中定义多个类。我个人认为这一功能是快速原型设计的有用工具。

在下一节中，你将了解已添加到现有库类中的新方法。

## 字符串、集合和文件的新库方法

很多时候，Java 开发人员不得不依赖像 Google Guava 或 Apache Commons 这样的外部库来获取便捷方法，否则他们就需要从一个项目复制粘贴到另一个项目。这些实现可能很小，但它们确实增加了需要维护和测试的额外 API 接口。Java 11 为 `String`、`Collections` 和 `Files` API 添加了便捷方法。

让我们通过示例来探索新功能。为了有趣，让我们编写一些 JUnit 测试，应用新的 API 并验证它们在不同用例中的正确行为。

## 字符串 API 增强

`String` API 新增了四个便捷方法。让我们通过示例逐一了解它们。

### 重复字符串

我们来看的第一个方法是 `String.repeat(Integer)`。从名称来看，你可能已经猜到该功能的目的。它只是将字符串重复 n 次。

我能想到不止一种情况下 `repeat` 方法会派上用场。假设你需要构建一个数据表，在键/值对之间填充空白以提高可读性。以下测试用例演示了该方法在这种场景下的使用：

```
@Test
void canRepeatString() {
    assertEquals("CPU Usage:               5%",
        renderInfo("CPU Usage:", "5%"));
    assertEquals("Memory Usage:          9.14 MB",
        renderInfo("Memory Usage:", "9.14 MB"));
    assertEquals("Free Disk:             96.5 GB",
        renderInfo("Free Disk:", "96.5 GB"));
}

private String renderedInfo(String title, String value) {
    return title + " ".repeat(30 - title.length()
        - value.length()) + value;
}
```

### 检查空白或空字符串

我相信你在职业生涯中的某个时刻一定需要实现数据验证。假设你想验证输入到基于 Web 的输入字段中的值的有效性。当然，我们不想允许空白值。

Java 11 引入了 `String.isBlank()` 方法，它指示字符串是否为空或仅包含空白。现在此功能已成为 Java 核心特性，不再需要引入 Apache `StringUtils` 作为外部依赖。你可以在以下测试用例中看到它的使用：

```
@Test
void canCheckIfStringContainsWhitespaces() {
    String nameFormFieldWithoutWhitespace = "Duke";
    String nameFormFieldWithWhitespace = "   ";
    assertFalse(nameFormFieldWithoutWhitespace.isBlank());
    assertTrue(nameFormFieldWithWhitespace.isBlank());
}
```

### 移除前导和尾随空白

验证字符串与清理字符串密切相关。开发人员经常遇到需要清理以提取实际信息的数据。这可能涉及过滤隐藏、错误或无用的字符。

新引入的 `String.strip()` 方法负责移除前导和尾随空白。你还可以更具体地使用 `String.stripLeading()` 仅移除前导字符，或使用 `String.stripTrailing()` 仅移除尾随字符。以下测试用例演示了所有三种方法的使用：

```
@Test
void canStripStringOfLeadingAndTrailingWhitespaces() {
    String nameFormField = "   Java Duke   ";
    assertEquals("Java Duke", nameFormField.strip());
    assertEquals("Java Duke", nameFormField.stripLeading());
    assertEquals("   Java Duke", nameFormField.stripTrailing());
}
```

### 处理多行字符串

逐行处理多行字符串可能既繁琐又消耗内存。首先，你必须根据行终止符拆分字符串。接下来，你需要遍历每一行。对于处理大型文本操作的软件系统，内存消耗可能是一个大问题。

Java 11 中引入的 `String.lines()` 方法允许你将多行文本作为 `Stream` 处理。`Stream` 表示一个元素序列，可以顺序处理，无需一次性将所有元素加载到内存中。以下是一个实际使用该方法的测试用例：

```
@Test
void canStreamLines() {
    String testString = "This\nis\na\ntest";
    List<String> lines = new ArrayList<>();
    testString.lines().forEach(line -> lines.add(line));
    assertEquals(List.of("This", "is", "a", "test"), lines);
}
```

## 集合 API 增强

在过去，将集合转换为数组很麻烦。你需要提供数组的新实例、其泛型类型以及数组的最终大小。

Java 11 使这种转换更加方便。现在，你可以使用 `Collection.toArray(IntFunction<T[]>)` 方法提供函数，而不是传递数组实例。以下代码示例比较了将集合转换为数组的旧方式和新方式：

```
@Test
void canTurnListIntoArray() {
    List<String> months = new ArrayList<>();
    months.add("January");
    months.add("February");
    months.add("March");
    assertArrayEquals(new String[]
        { "January", "February", "March" },
        months.toArray(new String[months.size()]));
    assertArrayEquals(new String[]
        { "January", "February", "March" },
        months.toArray(String[]::new));
}
```

1 在 Java 11 之前将集合转换为数组。

2 在 Java 11 中使用新方法将集合转换为数组。

## 文件 API 增强

对于开发人员来说，读写文件是非常常见的操作。Java 11 之前的代码需要你编写不必要的样板代码。为了解决这个问题，项目通常会引入可重用的工具方法，或者求助于众多开源库之一。

Java 11 添加了带各种重载的方法 `Files.readString(Path)` 和 `Files.writeString(Path, CharSequence, OpenOption)`，这使得读写文件更加容易。以下代码片段展示了这两种方法的实际应用：

```
@Test
void canReadString() throws URISyntaxException, IOException {
    URI txtFileUri = getClass().getClassLoader()
        .getResource("helloworld.txt").toURI();
    String content = Files.readString(Path.of(txtFileUri),
        Charset.defaultCharset());
    assertEquals("Hello World!", content);
}

@Test
void canWriteString() throws IOException {
    Path tmpFilePath = Path.of(
        File.createTempFile("tempFile", ".tmp").toURI());
    Path returnedFilePath = Files.writeString(tmpFilePath,
        "Hello World!", Charset.defaultCharset(),
        StandardOpenOption.WRITE);
    assertEquals(tmpFilePath, returnedFilePath);
}
```

## Optional 和 Predicate 的增强

Java 8 添加了 `Optional` 类型。该类型是一个包含可选值的单值容器。其主要目的是避免代码中充满不必要的 `null` 检查。你可以通过调用 `isPresent()` 方法来检查容器中是否存在值，而无需实际检索它。

在最新版本的 Java 中，你还可以询问相反的情况：“值是否为空？”无需使用 `isPresent()` 方法的取反，现在只需使用 `isEmpty()` 方法即可让代码更易于理解。以下测试用例比较了两种方法的使用：

```
@Test
void canCheckOptionalForEmpty() {
    String payDay = null;
    assertTrue(!Optional.ofNullable(payDay).isPresent());    ①
    assertTrue(Optional.ofNullable(payDay).isEmpty());    ②
    payDay = "Monday";
    assertFalse(!Optional.ofNullable(payDay).isPresent());    ①
    assertFalse(Optional.ofNullable(payDay).isEmpty());    ②
}
```

① 在 Java 11 之前检查 `Optional` 中是否存在值。

② 在 Java 11 中检查 `Optional` 中是否存在值。

`Predicate` 通常用于过滤对象集合中的元素。Java 11 添加了静态方法 `Predicate.not(Predicate)`，它返回一个预定义的 `Predicate` 实例，该实例对给定的 `Predicate` 取反。

假设你想过滤字符串流，例如某些月份。新的 `Predicate` 方法对于过滤所有不满足特定条件的月份非常有帮助。在下面的代码中，你会发现一个示例，它过滤掉所有不以字母“M”开头的月份。这比必须将 `(Predicate<String>) month -> month.startsWith("M")).negate()` 传递给 `filter` 方法更具表现力。

```
@Test
void canUsePredicateNotAsFilter() {
    List<String> months = List.of("January", "February", "March");
```

