

这个最后的示例用于用户手势，自动化了一个普遍的用户操作：使用键盘复制和粘贴。在这里，我们使用练习网站上可用的网页表单。示例 3-26 显示了一个模拟复制和粘贴的测试。

## 示例 3-26. 模拟复制和粘贴的测试

```
@Test
void testCopyAndPaste() {
    driver.get(
        "https://bonigarcia.dev/selenium-webdriver-java/web-form.html");
    Actions actions = new Actions(driver);

    WebElement inputText = driver.findElement(By.name("my-text")); ①
    WebElement textarea = driver.findElement(By.name("my-textarea"));

    Keys modifier = SystemUtils.IS_OS_MAC ? Keys.COMMAND :
    Keys.CONTROL; ②
    actions.sendKeys(inputText, "hello world").keyDown(modifier)
        .sendKeys(inputText, "a").sendKeys(inputText, "c")
        .sendKeys(textarea, "v").build().perform(); ③

    assertThat(inputText.getAttribute("value"))
        .isEqualTo(textarea.getAttribute("value")); ④
}
```

① 我们定位网页元素：一个输入文本和一个文本区域。

② 我们使用一个修饰键来发送组合键 ctrl+c 用于复制（在 Windows 和 Linux 中）或 cmd+c 用于粘贴（在 macOS 中）。为此，我们使用类 SystemUtils，该类可在开源库 Apache Commons IO 中获得（此依赖项在 Maven/Gradle 项目中是传递使用的）。

③ 我们实现由以下步骤组成的 actions 链：

- 向输入文本发送字符序列 hello world。
- 按下修饰键（ctrl 或 cmd，具体取决于操作系统）。请记住，此键保持按下状态，直到我们明确释放它。
- 我们向输入文本发送键 a。由于修饰键处于活动状态，因此产生的组合是 ctrl+a（或 cmd+a），结果是输入文本中的所有文本都被选中。
- 我们向输入文本发送键 c。同样，由于修饰键处于活动状态，组合是 ctrl+c（或 cmd+c），并且输入文本被
- 我们向文本区域发送键 `v`。这意味着发送 `ctrl+v`（或 `cmd+v`），剪贴板内容将被粘贴到文本区域。

④ 我们断言测试结束时两个元素（输入文本和文本区域）的内容相同。

## 等待策略

Web 应用程序是客户端-服务器分布式服务，其中客户端是 Web 浏览器，Web 服务器通常是远程主机。中间的网络延迟可能会影响 WebDriver 测试的可靠性。例如，在高延迟网络或服务器过载的情况下，缓慢的响应可能会对 WebDriver 测试的预期条件产生负面影响。此外，现代 Web 应用程序往往是动态和异步的。如今，JavaScript 允许使用不同的机制执行非阻塞（即异步）操作，例如回调、Promise 或 async/await。此外，我们可以异步地从其他服务器检索数据，例如使用 AJAX（异步 JavaScript 和 XML）或 REST（Representational State Transfer）服务。

总之，在我们的 WebDriver 测试中暂停并等待特定条件至关重要。因此，WebDriver API 提供了不同的等待资源。三种主要的等待策略是 *隐式*、*显式* 和 *流畅* 等待。以下小节对其进行解释并展示示例。

> **警告**
> 
> 当谈到 Java 中的等待时，你可能会做的第一件事是在代码中包含 `Thread.sleep()` 命令。一方面，这是一个简单的解决方案，但另一方面，它被认为是一种 *坏味道*（即一个微弱的迹象），可能导致不可靠的测试（因为延迟条件可能会发生变化）。作为一般规则，我强烈建议你不要使用它。相反，请考虑使用上述等待策略。

## SELENIUM WEBDRIVER 中的相关功能

除了之前介绍的等待策略之外，Selenium WebDriver 中还有其他你应该了解的互补特性：

- 加载策略。Selenium WebDriver 允许指定页面加载的不同方法。此功能可通过浏览器特定的功能访问（例如，使用 ChromeOptions、FirefoxOptions 等）。因此，我在第 5 章的“页面加载策略”中解释此功能。
- 超时。Selenium WebDriver 允许指定页面和脚本加载的最大经过时间。我在第 4 章的“超时”中解释此功能。

## 隐式等待

Selenium WebDriver 提供的第一个等待策略称为隐式。此机制允许在抛出异常之前指定一段时间。默认情况下，此等待的值为 0 秒（即根本不等待）。但当我们定义某个值时，Selenium WebDriver 在该时间内每 500 毫秒轮询一次 DOM，例如在尝试查找元素时。如果元素在经过的时间内出现，脚本将继续。否则，它会抛出异常。

示例 3-27 说明了此策略。此测试使用一个练习页面（见图 3-12 的屏幕截图），该页面会动态地将多个图像加载到 DOM 中。由于这些图像在页面加载之前不可用，因此我们需要等待这些图像可用。

## 示例 3-27. 在“加载图像”页面中使用隐式等待的测试

```
@Test
void testImplicitWait() {
    driver.get(
        "https://bonigarcia.dev/selenium-webdriver-java/loading-images.html");
    driver.manage().timeouts().implicitlyWait(Duration.ofSeconds(10));
```

```
WebElement landscape = driver.findElement(By.id("landscape"));
assertThat(landscape.getAttribute("src"))
        .containsIgnoringCase("landscape");
}
```

在与元素交互之前，我们指定隐式等待策略。在这种情况下，我们设置超时为 10 秒。

在以下调用中，我们照常使用 WebDriver API。

> **提示**
> 
> 你可以通过从测试中删除隐式等待（步骤 1）来尝试此功能。如果你这样做，你会注意到测试在步骤 2 中由于 `NoSuchElementException` 而失败。

## Hands-on Selenium WebDriver with Java

练习网站

## 加载图像

请等待图像加载完毕...

![](img/b67ec8e49249e340add6ed66ab413d78_168_0.png)

图 3-12. 加载图像的练习网页

## 显式等待

第二个等待策略称为显式，允许在特定条件发生之前最多暂停测试执行一段时间。要使用此策略，我们需要创建 `WebDriverWait` 的实例，使用 `WebDriver` 对象作为第一个构造函数参数，并使用 `Duration` 的实例作为第二个参数（指定超时）。

Selenium WebDriver 使用类 `ExpectedConditions` 提供了全面的预期条件集。这些条件非常易读，并且不需要进一步解释即可理解其目的。我建议你在你喜欢的 IDE 中使用自动完成功能来发现所有可能性。例如，**图 3-13** 显示了 Eclipse 中的此列表。

![](img/b67ec8e49249e340add6ed66ab413d78_169_0.png)

*图 3-13. Eclipse 中 ExpectedConditions 类的自动完成功能*

**示例 3-28** 显示了一个使用显式等待的测试。在示例中，我们使用 `presenceOfElementLocated` 条件等待练习网页上的其中一个图像可用。

### 示例 3-28. 在“加载图像”页面中使用显式等待的测试

```
@Test
void testExplicitWait() {
    driver.get(
        "https://bonigarcia.dev/selenium-webdriver-java/loading-images.html");
    WebDriverWait wait = new WebDriverWait(driver,
        Duration.ofSeconds(10));

    WebElement landscape = wait.until(ExpectedConditions
        .presenceOfElementLocated(By.id("landscape")));
    assertThat(landscape.getAttribute("src"))
        .containsIgnoringCase("landscape");
}
```

① 我们创建等待实例。在这种情况下，所选超时为 10 秒。

② 我们通过在 WebDriverWait 对象上调用 until() 方法显式等待给定条件。

示例 3-29 显示了另一个使用显式等待的测试。此测试使用另一个名为“慢计算器”的练习网页，其中包含一个基本计算器的 GUI，调整为等待可配置的时间来获取基本算术运算的结果（默认为 5 秒）。图 3-14 显示了此页面的屏幕截图。

## 示例 3-29. 在“慢计算器”页面中使用显式等待的测试

```
@Test
void testSlowCalculator() {
    driver.get(
        "https://bonigarcia.dev/selenium-webdriver-java/slow-calculator.html");

    // 1 + 3
    driver.findElement(By.xpath("//span[text()='1']")).click(); ①
    driver.findElement(By.xpath("//span[text()='+']")).click();
    driver.findElement(By.xpath("//span[text()='3']")).click();
    driver.findElement(By.xpath("//span[text()='=']")).click();

    // ... should be 4, wait for it
    WebDriverWait wait = new WebDriverWait(driver,
    Duration.ofSeconds(10));
    wait.until(ExpectedConditions.textToBe(By.className("screen"),
    "4")); ②
}
```

① 我们使用 XPath 定位器单击对应于运算 1+3 的按钮。

② 由于测试应等待结果准备就绪，因此我们显式等待该结果。在这种情况下，条件是 class 名称为 screen 的元素的文本等于 4。

