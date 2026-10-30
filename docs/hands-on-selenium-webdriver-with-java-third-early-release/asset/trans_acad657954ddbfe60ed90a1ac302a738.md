

证书。为此，WebDriver API 允许使用 `acceptInsecureCerts` 能力来禁用证书检查。此能力在 Chrome、Edge 和 Firefox 中相同。**示例 5-19** 显示了使用 Chrome 启用此能力的测试设置。此片段还包含打开不安全网站的测试。

## *示例 5-19.* 测试使用不安全证书的 Web 应用程序

```
```java
@BeforeEach
void setup() {
    ChromeOptions options = new ChromeOptions();
    options.setAcceptInsecureCerts(true); // ❶

    driver =
    WebDriverManager.chromedriver().capabilities(options).create();
}

@Test
void testInsecure() {
    driver.get("https://self-signed.badssl.com/"); // ❷

    String bgColor = driver.findElement(By.tagName("body"))
        .getCssValue("background-color");
    Color red = new Color(255, 0, 0, 1);
    assertThat(Color.fromString(bgColor).isEqualTo(red)); // ❸
}
```
```

+   ❶ 我们启用该能力以允许不安全证书。
+   
+   ❷ 我们打开一个使用不安全证书的网站（本例中为自签名）。
+   
+   ❸ 如果网站已加载，则 body 背景应为红色。

## 本地化

在软件工程中，*本地化* 指的是将应用程序适配以满足其终端用户的文化和语言（称为 *locale*）的过程。本地化有时写作 *l10n*，其中 10 是英文单词中 *l* 和 *n* 之间字母的数量。最常见的本地化活动是将应用程序 UI 中显示的文本翻译成不同的语言。此外，还可以根据 locale 调整其他 UI 方面，例如货币（欧元、美元等）、度量系统（例如公制或英制系统）或数字和日期格式。

L10n 是更广泛概念 *国际化*（i18n）的一部分，国际化是设计和开发应用程序以便为异构目标受众轻松进行 l10n 的过程。实现 i18n 的常见做法是使用 Unicode 进行文本编码或为垂直文本或非拉丁文字添加 CSS 支持。

*本地化测试* 是一种非功能性测试形式，其中 SUT 会针对特定 locale 设置进行验证。WebDriver API 允许我们通过设置能力 `intl.accept_languages` 基于浏览器语言进行本地化测试。此能力允许指定 locale 标识符，例如 *en_US* 表示美国英语或 *es_ES* 表示欧洲西班牙语，等等。**示例 5-20** 显示了在 Chrome 中配置此能力的测试设置。我们可以在 Edge 中使用相同的语法，尽管我们在 Firefox 中将此能力指定为首选项（参见 **示例 5-21**）。

## *示例 5-20.* 测试使用 Chrome 首选 locale

```
String lang;

@BeforeEach
void setup() {
    lang = "es-ES";
    ChromeOptions options = new ChromeOptions();
    Map<String, Object> prefs = new HashMap<>();
    prefs.put("intl.accept_languages", lang);
    options.setExperimentalOption("prefs", prefs);

    driver =
WebDriverManager.chromedriver().capabilities(options).create();
}

@Test
void testAcceptLang() {
    driver.get(
        "https://bonigarcia.dev/selenium-webdriver-
java/multilanguage.html");

    ResourceBundle strings = ResourceBundle.getBundle("strings",
```

```
Locale.forLanguageTag(lang)); ③
String home = strings.getString("home");
String content = strings.getString("content");
String about = strings.getString("about");
String contact = strings.getString("contact");

String bodyText =
driver.findElement(By.tagName("body")).getText();

assertThat(bodyText).contains(home).contains(content).contains(about)
    .contains(contact); ④
}
```

+   ① 我们在 Chrome 中指定西班牙语（西班牙）为首选语言。
+   ② 我们打开一个支持多语言（英语和西班牙语）的练习页面。
+   ③ 我们使用资源包读取文本翻译。你可以在项目文件夹 src/test/resources 中的文件 strings_es.properties（以及 strings_en.properties）中找到这些字符串。
+   ④ 我们断言文档 body 包含所有预期的字符串。

## 示例 5-21. 测试设置指定 Firefox 的首选 locale

```
@BeforeEach
void setup() {
    lang = "es-ES";
    FirefoxOptions options = new FirefoxOptions();
    options.addPreference("intl.accept_languages", lang);

    driver = WebDriverManager.firefoxdriver().capabilities(options)
        .create();
}
```

使用 Selenium WebDriver 进行本地化测试的第二种替代方法。不是更改首选语言（这决定了 HTTP 头 accept-language），我们可以更改 Web 浏览器的默认语言。多语言应用程序，如果该 HTTP

```
options.addArguments("-private");

driver = WebDriverManager.firefoxdriver().capabilities(options)
    .create();
}
```

## Chrome DevTools 协议

Chrome DevTools 是一组用于 Chromium 基于 Web 浏览器的 Web 开发工具，例如 Chrome 和 Edge。这些工具允许检查、调试或分析这些浏览器，等等。Chrome DevTools 协议（CDP）是一种通信协议，允许外部客户端操作 Chrome DevTools。从版本 4 开始，Selenium WebDriver 提供了 CDP 的接口。这样，我们就可以在 WebDriver 测试中利用 Chrome DevTools 提供的功能。

Chrome DevTools 在 Selenium WebDriver 中仅限于继承自 WebDriver 层次结构中 ChromiumDriver 的浏览器（参见第 2 章中的图 2-2），即 Chrome 和 Edge。我们首先需要使用 DevTools 实例的 createSession() 方法打开一个 CDP 会话（即客户端与浏览器之间的 WebSocket 连接）才能使用它。示例 5-25 显示了我推荐用于测试的 Chrome DevTools 结构。如你所见，CDP 会话在测试设置中创建，并在拆卸时关闭。每个测试都将使用类属性 devTools 与 Chrome DevTools 交互。

## 示例 5-25. 使用 Chrome DevTools 的测试结构

```
WebDriver driver;

DevTools devTools; ①

@BeforeEach
void setup() {
    driver = WebDriverManager.chromedriver().create();
    devTools = ((ChromeDriver) driver).getDevTools(); ②
    devTools.createSession(); ③
}

@AfterEach
void teardown() {
```

```
devTools.close(); ④
driver.quit();
}
```

① 我们声明一个 DevTools 类属性。
② 我们从 driver 对象获取 dev tools 实例。在此示例（以及其余示例）中，我使用 ChromeDriver（尽管 EdgeDriver 实例也有效）。
③ 我们在测试逻辑中创建 CDP 会话以与 Chrome DevTools 交互。
④ 在每次测试之后并在退出 WebDriver 会话之前，我们终止 CDP 会话。

以下小节展示了几个示例，说明 DevTools 在 WebDriver 测试中的潜力。在这些示例中，我们使用 DevTools 实例的 send() 方法发送 CDP 命令。WebDriver API 提供了各种命令，允许对 Web 应用程序进行不同的测试操作，例如模拟网络条件、处理 HTTP 头、阻止 URL 等。

## 模拟网络条件

CDP 允许模拟不同的网络（例如移动 2G/3G/4G、Wi-Fi 或蓝牙等）和条件（例如延迟或吞吐量）。此功能有助于在特定连接参数下测试 Web 应用程序的行为。示例 5-26 显示了使用该功能的测试。如你所见，此测试发送两个 CDP 命令：

+   1. Network.enable() : 激活网络跟踪。此命令需要三个可选参数：
+   - Optional<Integer> maxTotalBufferSize: 网络负载的最大缓冲区大小（以字节为单位）。
+   - `Optional<Integer> maxResourceBufferSize` : 单个资源的最大缓冲区大小（以字节为单位）。
+   - `Optional<Integer> maxPostDataSize`: 最长 post body 大小（以字节为单位）
2. `Network.emulateNetworkConditions()` : 激活网络模拟。模拟条件使用以下参数指定：
+   - `Boolean offline`: 模拟无 Internet 连接。
+   - `Number latency`: 请求到响应的最小延迟（以 ms 为单位）。
+   - `Number downloadThroughput`: 最大下载吞吐量（以字节/秒为单位）。-1 禁用下载限流。
+   - `Number uploadThroughput`: 最大上传吞吐量（以字节/秒为单位）。-1 禁用上传限流。
+   - `Optional<ConnectionType>` `connectionType`: 模拟的连接技术。枚举 `ConnectionType` 接受以下选项：NONE、CELLULAR2G、CELLULAR3G、CELLULAR4G、BLUETOOTH、ETHERNET、WIFI、WIMAX 和 OTHER。

## 示例 5-26. 测试模拟网络条件

```
@Test
void testEmulateNetworkConditions() {
    devTools.send(Network.enable(Optional.empty(),
Optional.empty(),
        Optional.empty())); ①
    devTools.send(Network.emulateNetworkConditions(false, 100, 50 *
1024,
        50 * 1024, Optional.of(ConnectionType.CELLULAR3G))); ②

