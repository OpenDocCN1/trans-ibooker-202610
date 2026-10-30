

1. 我们附加了一段 JavaScript 代码片段，用于定位网页中的元素。请注意，使用标准 WebDriver API 也可以完成相同的操作。不过，为了演示，我们采用了这种方式。
2. 我们附加了另一段 JavaScript 代码，该代码会返回传递给它的第一个参数。
3. 我们读取已固定脚本的集合。
4. 我们断言已固定脚本的数量符合预期（即 2 个）。
5. 我们执行第一个已固定脚本。结果是，我们获得了网页中的第三个链接，并将其作为 Java 中的 WebElement。
6. 我们单击该链接，它应该对应实践网页的链接。结果是，浏览器应导航到该页面。
7. 我们断言当前 URL 与初始 URL 不同。
8. 我们执行第二个已固定脚本。请注意，即使浏览器中的页面已发生变化，仍然可以运行已固定脚本（因为脚本附加到会话，而不是单个页面）。
9. 我们断言返回的消息符合预期。
10. 我们取消固定其中一个脚本。
11. 我们验证已固定脚本的数量符合预期（此时为 1 个）。

## 异步脚本

`JavascriptExecutor` 接口的 `executeAsyncScript()` 方法允许使用 Selenium WebDriver 在网页上下文中执行异步脚本。与 `executeScript()` 相同，`executeAsyncScript()` 也会阻塞 Java 控制流，直到传入的脚本执行完毕。两者的区别在于，使用 `executeAsyncScript()` 时，我们必须通过调用一个由系统提供的回调函数来显式地表示脚本终止。该回调函数会作为最后一个参数注入到被执行的函数中。换句话说，我们需要调用下面的 JavaScript 代码片段来调用终止回调。**示例 4-6** 展示了使用该方法的测试。

```js
arguments[arguments.length - 1]
```

### 示例 4-6. *执行异步 JavaScript 的测试*

```java
@Test
void testAsyncScript() {
    driver.get("https://bonigarcia.dev/selenium-webdriver-java/");
    JavascriptExecutor js = (JavascriptExecutor) driver;

    Duration pause = Duration.ofSeconds(2);
    String script = "const callback = arguments[arguments.length - 1];"
        + "window.setTimeout(callback, " + pause.toMillis() + ");";

    long initMillis = System.currentTimeMillis();
    js.executeAsyncScript(script);
    Duration elapsed = Duration
        .ofMillis(System.currentTimeMillis() - initMillis);
    log.debug("The script took {} ms to be executed",
        elapsed.toMillis());
    assertThat(elapsed).isGreaterThanOrEqualTo(pause);
}
```

1. 我们定义一个 2 秒的暂停时间。
2. 我们定义要执行的脚本。在第一行中，我们为回调函数定义一个常量（即最后一个脚本参数）。之后，我们使用 JavaScript 函数 `window.setTimeout()`，让脚本暂停执行指定的时间。
3. 我们获取当前系统时间（以毫秒为单位）。
4. 我们执行脚本。如果一切如预期运行，测试执行将在此行阻塞 2 秒（如第 1 步所定义）。
5. 我们计算执行上一行代码所需的时间。
6. 我们断言经过的时间符合预期（通常会比定义的暂停时间多出几毫秒）。

> 提示
> 你可以在第 5 章的“通知”部分找到另一个执行异步脚本的示例。

## 超时

Selenium WebDriver 允许指定三种类型的超时。我们可以通过调用 WebDriver API 中的 `manage().timeouts()` 方法来使用它们。第一种超时是隐式等待，已在第 4 章的“隐式等待”部分（作为等待策略的一部分）中介绍。其他选项是页面加载超时和脚本加载超时，下面将分别介绍。

### 页面加载超时

页面加载超时提供了一个时间限制，用于中断导航尝试。换句话说，此超时限制了加载网页所允许的时间。当超过该超时时间（默认值为 30 秒）时，将抛出异常。示例 4-7 展示了这种超时的用法。如你所见，这段代码是负向测试的一个简单实现。换句话说，它用于检查被测系统中的非预期条件。

### 示例 4-7. 使用页面加载超时的测试

```java
@Test
void testPageLoadTimeout() {
    driver.manage().timeouts().pageLoadTimeout(Duration.ofMillis(1)); ①

    assertThatThrownBy(() -> driver
        .get("https://bonigarcia.dev/selenium-webdriver-java/"))
        .isInstanceOf(TimeoutException.class); ②
}
```

① 我们指定了最短的页面加载超时时间，即 1 毫秒。

② 我们加载一个网页。由于不可能在不到 1 毫秒的时间内加载该网页，因此此调用（以 Java lambda 的形式实现）将失败。出于这个原因，我们预期 lambda 中会抛出 `TimeoutException`，并使用 AssertJ 方法 `assertThatThrownBy` 进行验证。

> 注意
> 你可以通过移除超时声明（即第 1 步）来尝试此测试。如果这样做，测试将失败，因为预期会抛出异常，但实际上没有抛出异常。

### 脚本加载超时

*脚本加载超时*为正在执行的脚本提供了一个时间限制，用于中断脚本执行。该超时的默认值为 300 秒。示例 4-8 展示了使用脚本加载超时的测试。

### 示例 4-8. 使用脚本加载超时的测试

```java
@Test
void testScriptTimeout() {
    driver.get("https://bonigarcia.dev/selenium-webdriver-java/");
    JavascriptExecutor js = (JavascriptExecutor) driver;

    driver.manage().timeouts().scriptTimeout(Duration.ofSeconds(3)); ①

    assertThatThrownBy(() -> {
        long waitMillis = Duration.ofSeconds(5).toMillis();
        String script = "const callback = arguments[arguments.length - 1];" +
            "window.setTimeout(callback, " + waitMillis +
            " ); ";
        js.executeAsyncScript(script);
    }).isInstanceOf(ScriptTimeoutException.class);
}
```

① 我们定义了 3 秒的脚本超时时间。这意味着，执行时间超过该时长的脚本将抛出异常。

② 我们执行一个会暂停 5 秒的异步脚本。

③ 脚本执行时间超过了配置的脚本超时时间，因此产生了 `ScriptTimeoutException`。同样，这是一个负向测试，即用于预期该异常的测试。

## 屏幕截图

如你所知，Selenium WebDriver 主要用于对 Web 应用程序执行端到端功能测试。换句话说，我们通过与 Web 应用程序的用户界面交互（即使用 Web 浏览器）来验证其行为是否符合预期。这种方式非常适合自动化高级用户场景，但也会带来各种困难。端到端测试面临的主要挑战之一，是诊断测试失败的根本原因。假设此次失败是合理的（即并非由实现不当的测试引起），其根本原因可能多种多样：客户端问题（例如，JavaScript 逻辑错误）、服务器端问题（例如，内部异常），或与其他组件的集成问题（例如，无法适当访问数据库），除此之外还可能有其他原因。Selenium WebDriver 中用于失败分析的最普遍机制之一，就是截取浏览器屏幕截图。本节将介绍 WebDriver API 为此提供的机制。

> 提示
> 第 8 章的“失败分析”部分介绍了特定于框架的技术，用于确定测试何时失败，并随后执行各种失败分析操作，例如截取屏幕截图、录制视频和收集日志。

Selenium WebDriver 提供了 `TakesScreenshot` 接口，用于截取浏览器屏幕截图。任何继承自 `RemoteWebDriver` 的驱动程序对象（参见第 2 章中的图 2-2）也实现了该接口。因此，我们可以将实例化主要浏览器之一（例如 `ChromeDriver`、`FirefoxDriver` 等）的 `WebDriver` 对象转换如下：

```java
WebDriver driver = new ChromeDriver();
TakesScreenshot ts = (TakesScreenshot) driver;
```

`TakesScreenshot` 接口只提供了一个名为 `getScreenshotAs(OutputType<X> target)` 的方法，用于截取屏幕截图。参数 `OutputType<X> target` 决定了屏幕截图的类型以及返回值。表 4-2 展示了该参数可用的选项。

