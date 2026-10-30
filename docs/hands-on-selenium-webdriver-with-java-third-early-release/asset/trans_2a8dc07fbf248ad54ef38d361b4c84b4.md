

一旦我们实现了监听器，我们就需要创建装饰器类。有两种方法可以做到这一点。如果我们想装饰一个 WebDriver 对象，可以创建一个 EventFiringDecorator 的实例（将监听器作为构造函数的参数传递），然后调用 decorate() 方法来传递 WebDriver 对象。例如：

```
WebDriver decoratedDriver = new EventFiringDecorator(myListener)
    .decorate(originalDriver);
```

第二种方法是装饰 WebDriver API 的其他对象，即：WebElement、TargetLocator、Navigation、Options、Timeouts、Window、Alert 或 VirtualAuthenticator。在这种情况下，我们需要在 EventFiringDecorator 对象中调用 createDecorated() 方法来获取一个 Decorated<T> 泛型类。以下代码片段展示了使用 WebElement 作为参数的示例：

```
Decorated<WebElement> decoratedWebElement = new
EventFiringDecorator(
    listener).createDecorated(myWebElement);
```

让我们看一个完整的示例。首先，示例 4-29 显示了实现 WebDriverListener 接口的类。请注意，该类实现了两个方法：afterGet() 和 beforeQuit()。这两个方法都调用 takeScreenshot() 来截取浏览器屏幕截图。总的来说，我们在加载网页后（通常在测试开始时）和退出前（通常在测试结束时）收集浏览器屏幕截图。然后，示例 4-30 显示了使用此监听器的测试。

## 示例 4-29. 实现方法 afterGet() 和 beforeQuit() 的事件监听器

```
public class MyEventListener implements WebDriverListener {
    static final Logger log = getLogger(lookup().lookupClass());
    @Override
    public void afterGet(WebDriver driver, String url) { ①
```

```
class EventListenerJupiterTest {

    WebDriver driver;

    @BeforeEach
    void setup() {
        MyEventListener listener = new MyEventListener();
        WebDriver originalDriver =
        WebDriverManager.chromedriver().create();
        driver = new
        EventFiringDecorator(listener).decorate(originalDriver); ①
    }

    @AfterEach
    void teardown() {
        driver.quit();
    }

    @Test
    void testEventListener() {
        driver.get("https://bonigarcia.dev/selenium-webdriver-java/");
        assertThat(driver.getTitle())
            .isEqualTo("Hands-On Selenium WebDriver with Java");
        driver.findElement(By.linkText("Web form")).click(); ②
    }
}
```

① 我们使用 MyEventListener 的一个实例创建一个装饰后的 WebDriver 对象。我们在 @Test 逻辑中使用 resulting driver 来控制浏览器。

② 我们点击一个网页链接以更改页面。因此，监听器中拍摄的两个屏幕截图应该不同。

## WebDriver 异常

WebDriver API 提供的所有异常都继承自 WebDriverException 类，并且是未检查的（如果您不熟悉此术语，请参阅以下侧边栏）。图 4-12 显示了 Selenium WebDriver 4 中的这些异常。正如您在这张图片中看到的那样，有许多不同的异常类型。表 4-10 总结了一些最常见的异常及其常见原因。

![](img/b67ec8e49249e340add6ed66ab413d78_244_0.png)

*图 4-12. Selenium WebDriver 异常*

## JAVA 中的异常

在 Java 中，异常是中断程序执行的事件。标准 Java API 中使用类层次结构来建模不同类型的异常。此层次结构的根是 `Throwable` 类。它有两个子类型：

- `Error`：不可恢复的问题。一般来说，应用程序崩溃而不是尝试处理这些严重错误。一些例子是 `OutOfMemoryError` 或 `StackOverflowError`。
- `Exception`：可恢复的问题。应用程序可以通过使用 `try-catch` 块来处理这些异常。有两种类型的 `Exception` 类：
  - *Checked* 异常：直接继承 `Throwable` 类的类（除 *RuntimeException* 和 *Error* 外）。这些异常在编译时得到验证，因此我们必须使用 `try-catch` 块处理它们或使用 `throws` 重新抛出。检查异常的例子是 `IOException` 或 `MalformedURLException`。
  - *Unchecked* 异常：继承自 `RuntimeException` 类的类，而 `RuntimeException` 是 `Exception` 的子类型。这些异常不需要使用 `try-catch` 处理，也不需要使用 `throws` 重新抛出。未检查异常的一些例子是 `NullPointerException` 或 `ArrayIndexOutOfBoundException`。

## 表 4-10. 常见的 Web Driver 异常

| Exception | Description | Common causes |
|---|---|---|
| `NoSuchElementException` | Web 元素不可用 | - 无效的定位策略<br>- 元素尚未渲染（也许您需要等待它） |
| `NoAlertPresentException` | 对话框（alert、prompt 或 confirm）不可用 | 尝试对不可用的对话框执行操作（例如 `accept()` 或 `dismiss()`） |
| `NoSuchWindowException` | 窗口或标签页不可用 | 尝试切换到不可用的窗口或标签页 |
| `NoSuchFrameException` | Frame 或 iframe 不可用 | 尝试切换到不可用的 frame 或 iframe |
| `InvalidArgumentException` | 调用 WebDriver API 的某些方法时参数不正确 | - 导航方法中的错误 URL<br>- 上传文件时不存在的路径<br>- JavaScript 脚本中的错误参数类型 |
| `StaleElementReferenceException` | 元素已失效，即它不再出现在页面上 | 在尝试与之前定位的元素交互时 DOM 被更新 |
| `UnreachableBrowserException` | 与浏览器通信出现问题 | - 无法建立与远程浏览器的连接<br>- WebDriver 会话中途浏览器崩溃 |
| `TimeoutException` | 页面加载超时 | 某些网页加载时间比预期更长 |
| `ScriptTimeoutException` | 脚本加载超时 | 某些脚本执行时间比预期更长 |
| `ElementNotVisibleException` | 元素在 DOM 上但不可见/不可选/不可点击 | - 在元素显示/可选/可点击之前等待不足（或不存在）<br>- 页面布局（可能是视口变化导致）使得某些元素覆盖了我们尝试交互的元素 |
| `ElementNotSelectableException` |  |  |
| `ElementClickInterceptedException` |  |  |

## 总结与展望

本章全面回顾了在不同 Web 浏览器中可互操作的 WebDriver API 功能。其中，您了解了如何使用 Selenium WebDriver 执行 JavaScript，包括同步、固定（即附加到 WebDriver 会话）的和异步脚本。然后，您学习了超时，用于指定页面加载和脚本执行的时限间隔。此外，您还看到了如何管理浏览器的多个方面，例如其大小和位置、导航历史或 cookie。接下来，您了解了如何与特定 Web 元素交互，例如下拉列表（select 和数据列表）、导航目标（窗口、标签页、框架和 iframe）以及对话框（alerts、prompts、confirms 和 modals）。最后，我们回顾了在 Selenium WebDriver 4 中实现 Web 存储和事件监听器的机制以及最相关的 WebDriver 异常（及其常见原因）。

下一章将继续介绍 WebDriver API 的功能。本章解释了特定于给定浏览器（例如 Chrome、Firefox 等）的方面，包括浏览器功能（例如 `ChromeOptions`、`FirefoxOptions` 等）、Chrome DevTools 协议（CDP）、网络拦截、模拟地理位置坐标、WebDriver 双向（BiDi）协议、身份验证机制、Web 代理、日志收集技术或将网页打印为 PDF 等功能。

## 第 5 章. 浏览器特定操作

## 早期发布读者的说明

使用 Early Release 电子书，您可以在这些标题正式发布之前很久就利用这些技术——作者在撰写时提供的原始和未编辑内容，以便您可以在这些技术正式发布之前利用它们。

这将是最终书籍的第 5 章。请注意，GitHub 仓库将在稍后激活。

如果您对如何改进本书的内容和/或示例有任何意见，或者如果您注意到本章中缺少材料，请联系开发编辑 rfernando@oreilly.com。

正如您迄今所见，WebDriver API 的许多功能在浏览器之间兼容，即我们可以使用这些功能以编程方式驱动不同类型的浏览器。WebDriver API 的其他部分在浏览器之间不可互操作。换句话说，有些 WebDriver 特性可用于某些浏览器（例如 Chrome 或 Edge），但对其他浏览器（例如 Firefox）不可用（或不同）。本章回顾这些浏览器特定的功能。

## 浏览器功能

Selenium WebDriver 允许通过使用 *capabilities* 指定浏览器特定的方面。功能示例包括无头模式、页面加载策略、Web 扩展的使用或推送通知管理，

