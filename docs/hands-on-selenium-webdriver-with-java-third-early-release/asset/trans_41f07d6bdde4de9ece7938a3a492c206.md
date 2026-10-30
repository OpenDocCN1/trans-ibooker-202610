

## Selenium WebDriver 实战（Java 版）

## 练习站点

## 慢速计算器

该计算器需要等待 5 秒才能获得基本算术运算的结果。

![](img/b67ec8e49249e340add6ed66ab413d78_171_0.png)

图 3-14. 带有“慢速计算器”演示的练习网页

## 流畅等待

最后一种策略称为*流畅等待*。该机制是显式等待的泛化。换句话说，我们使用流畅等待来暂停测试，直到满足某些条件；此外，流畅等待还提供了细粒度的配置能力。[表 3-12](#) 汇总了 `FluentWait` 中可用的方法。顾名思义，该类提供了流畅的 API，因此我们可以在同一行中链式调用多个方法。[示例 3-30](#) 展示了一个使用流畅等待的测试。

### 表 3-12. FluentWait 方法

| 方法 | 描述 |
|---|---|
| `withTimeout(Duration timeout)` | 使用 Java Duration 设置超时时间 |
| `pollingEvery(Duration interval)` | 评估条件的频率（默认 500 毫秒） |
| `withMessage(String message)` | 自定义错误消息 |
| `withMessage(Supplier<String> messageSupplier)` |  |
| `ignoring(Class<? extends Throwable> exceptionType)` | 等待条件时忽略特定异常 |
| `ignoring(Class<? extends Throwable> firstType, Class<? extends Throwable> secondType)` |  |
| `ignoreAll(Collection<Class<? extends Throwable>> types)` |  |
| `until(Function<? super T, V> isTrue)` | 预期条件 |

## 示例 3-30. 使用流畅等待的测试

```java
@Test
void testFluentWait() {
    driver.get(
        "https://bonigarcia.dev/selenium-webdriver-java/loading-images.html");
    Wait<WebDriver> wait = new FluentWait<>(driver)
        .withTimeout(Duration.ofSeconds(10))
        .pollingEvery(Duration.ofSeconds(1))
        .ignoring(NoSuchElementException.class); ①

    WebElement landscape = wait.until(ExpectedConditions
        .presenceOfElementLocated(By.id("landscape")));
    assertThat(landscape.getAttribute("src"))
        .containsIgnoringCase("landscape");
}
```

① 如你所见，该测试与示例 3-28 非常相似，不过使用 `FluentWait` 实例时，我们可以指定额外的特性。在此例中，我们将轮询时间改为 1 秒。

## 总结与展望

本章介绍了 Selenium WebDriver API 的基础知识。首先，你学习了如何创建和关闭 WebDriver 实例。这些对象代表由 Selenium WebDriver 控制的浏览器。这样，我们针对 Chrome 使用 ChromeDriver 实例，针对 Firefox 使用 FirefoxDriver 实例，等等。其次，你了解了 WebElement，这是一个表示不同网页元素（例如链接、图片、表单字段等）的类。Selenium WebDriver 提供了多种定位网页元素的策略：按 HTML 属性（id、name 或 class）、标签名、链接文本（完整或部分）、CSS 选择器以及 XPath。我们还了解了 Selenium WebDriver 4 中一种全新的策略，称为相对定位器。然后，我们讨论了用户操作的模拟，包括键盘和鼠标操作。你可以使用这些操作从简单动作（例如点击链接、填写文本框等）到复杂的手势（例如拖放、点击悬停等）。最后，我们考察了在 Selenium WebDriver 测试中等待的能力。由于当今 Web 应用的分布式、动态和异步特性，这一功能至关重要。Selenium WebDriver 中有三种主要的等待策略：隐式等待（为等待元素指定一个总体超时时间）、显式等待（暂停测试执行直到给定条件满足）和流畅等待（显式等待的扩展，带有一些细粒度设置）。

下一章将继续深入探讨 WebDriver API。具体来说，第 4 章将回顾不同浏览器（Chrome、Edge、Firefox 等）中的互操作特性。在这些特性中，你将了解如何执行 JavaScript、指定事件监听器、配置页面和脚本加载的超时时间、管理浏览器历史记录、截屏、操作 Cookie、操作下拉列表（即 select 和 data list）、处理窗口目标（即标签页、框架和 iframe）以及对话框（即警告框、提示框、确认框和模态弹窗）、使用 Web 存储，并理解 WebDriver 异常。

## 第 4 章 跨浏览器特性

> **致早期版本读者**
>
> 通过早期版本电子书，你可以在书籍正式发布之前，以作者写作时的原始未编辑形式获取内容，从而更早地了解这些技术。
>
> 这将是最终书籍的第四章。请注意，GitHub 仓库稍后将启用。
>
> 如果你对本内容和/或本书示例的改进有任何意见，或者发现本章有缺失内容，请联系开发编辑：rfernando@oreilly.com。

本章回顾 Selenium WebDriver 中可在不同 Web 浏览器中互操作的特性。在这组特性中，一个重要的多用途能力是执行 JavaScript。此外，WebDriver API 允许配置页面和脚本加载的超时时间。另一个便捷功能是截取浏览器屏幕或仅截取指定元素对应部分的截图。然后，我们可以使用 WebDriver 管理受控浏览器的不同方面，例如浏览器大小和位置、历史记录或 Cookie。接着，WebDriver 提供了各种工具来控制特定的 Web 元素，例如下拉列表（即 HTML select 字段和数据列表）、导航目标（即窗口、标签页、框架和 iframe）或对话框（即警告框、提示框、确认框和模态对话框）。最后，我们学习如何使用 Web 存储处理本地和会话数据、实现事件监听器，以及了解 WebDriver API 提供的异常。

## 执行 JavaScript

如你所知，JavaScript 是一种所有主流浏览器都支持的高级编程语言。我们可以在 Web 应用的客户端使用 JavaScript 执行各种操作，例如 DOM 操作、用户交互、处理来自远程服务器的请求-响应、使用正则表达式等等众多功能。幸运的是，对于测试自动化，Selenium WebDriver 允许注入并执行任意 JavaScript 片段。为此，Selenium WebDriver API 提供了 `JavascriptExecutor` 接口。表 4-1 介绍了该接口中可用的公共方法，并将其分为三类：同步脚本、固定脚本和异步脚本。以下小节将更详细地介绍它们，并通过不同示例说明其用法。

### 表 4-1. JavascriptExecutor 方法

| 类别 | 方法 | 返回类型 | 描述 |
|---|---|---|---|
| 同步脚本 | `executeScript(String script, Object... args)` | Object | 在当前页面上执行一些 JavaScript 代码 |
| 固定脚本 | `pin(String script)` | ScriptKey | 将某段 JavaScript 附加到 WebDriver 会话。在 WebDriver 会话存续期间，*固定*的脚本可以多次使用 |
|  | `unpin(ScriptKey key)` | void | 从 WebDriver 会话中分离先前固定的脚本 |
|  | `getPinnedScripts()` | Set<ScriptKey> | 收集所有固定的脚本（每个脚本由一个唯一的 ScriptKey 标识） |
| 异步脚本 | `executeAsyncScript(String script, Object... args)` | Object | 在当前页面上执行一些 JavaScript 代码（通常包含异步操作）。与 `executeScript()` 不同，使用 `executeAsyncScript()` 执行的脚本必须通过调用回调函数来显式地发出终止信号。按照惯例，该回调作为最后一个参数注入脚本中 |

任何继承自 `RemoteWebDriver` 类的驱动对象也都实现了 `JavascriptExecutor` 接口。因此，当使用由通用 `WebDriver` 接口声明的主流浏览器（例如 `ChromeDriver`、`FirefoxDriver` 等）时，我们可以将其强制转换为

