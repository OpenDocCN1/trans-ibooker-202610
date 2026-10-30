## 第 5 章. 浏览器特定操作

## 早期发布读者的说明

使用 Early Release 电子书，您可以在这些标题正式发布之前很久就利用这些技术——作者在撰写时提供的原始和未编辑内容，以便您可以在这些技术正式发布之前利用它们。

这将是最终书籍的第 5 章。请注意，GitHub 仓库将在稍后激活。

如果您对如何改进本书的内容和/或示例有任何意见，或者如果您注意到本章中缺少材料，请联系开发编辑 rfernando@oreilly.com。

正如您迄今所见，WebDriver API 的许多功能在浏览器之间兼容，即我们可以使用这些功能以编程方式驱动不同类型的浏览器。WebDriver API 的其他部分在浏览器之间不可互操作。换句话说，有些 WebDriver 特性可用于某些浏览器（例如 Chrome 或 Edge），但对其他浏览器（例如 Firefox）不可用（或不同）。本章回顾这些浏览器特定的功能。

## 浏览器功能

Selenium WebDriver 允许通过使用 *capabilities* 指定浏览器特定的方面。功能示例包括无头模式、页面加载策略、Web 扩展的使用或推送通知管理，



among many others. 如 Figure 5-1 所示，WebDriver API 提供了 一组 Java 类来定义这些 capabilities。`Capabilities` 接口位于此层级 的顶部。内部而言，capabilities 接口使用键/值对来处理数据，这些键/值对封装了浏览器的特定方面。然后，不同的 Java 类实现此接口以指定网络浏览器的 capabilities（Chrome、Edge、Firefox 等）。Table 5-1 总结了 `Capabilities` 层级的主要类及其对应的目标浏览器。

![](img/b67ec8e49249e340add6ed66ab413d78_251_0.png)

*Figure 5-1. Capabilities hierarchy*

| Package | Class | Browser |
|---|---|---|
| org.openqa.selenium | MutableCapabilities | Generic (cross-browser) |
| org.openqa.selenium.chrome | ChromeOptions | Chrome |
| org.openqa.selenium.edge | EdgeOptions | Edge |
| org.openqa.selenium.firefox | FirefoxOptions | Firefox |
| org.openqa.selenium.safari | SafariOptions | Safari |
| org.openqa.selenium.opera | OperaOptions | Opera |
| org.openqa.selenium.ie | InternetExplorerOptions | Internet Explorer |
| org.openqa.selenium.remote | DesiredCapabilities | Remote browsers (see Chapter 6) |

以下小节回顾了本书中考虑的主要网络浏览器，即 Chrome、Edge 和 Firefox 的最相关 capabilities。由于 Chrome 和 Edge 都是基于 Chromium 的浏览器，因此两者可用的 capabilities 相当。这一事实在 Figure 5-1 中有所体现，显示 capabilities 类 ChromeOptions 和 EdgeOptions 都继承自同一个父类（称为 ChromiumOptions）。

## Headless Browser

无需 GUI 即可与 Web 应用程序交互的浏览器称为无头浏览器。这些浏览器的主要用途之一正是端到端测试，即与 Web 应用程序的自动交互。如今，像 Chrome、Edge 或 Firefox 这样的现代 Web 浏览器可以作为无头浏览器运行。WebDriver API 允许使用 capabilities 启动这些浏览器处于无头模式。为此，首先需要创建浏览器 capabilities 的实例。在主要浏览器中，这些对象分别是 ChromeOptions、EdgeOptions 或 FirefoxOptions 的实例。然后，需要通过在浏览器 capabilities 对象上调用方法 setHeadless(true) 来启用无头模式。最后，需要在创建 WebDriver 对象时设置这些 capabilities。

如“WebDriver Creation”所述，我们有不同的方法来创建 WebDriver 对象。首先，我们可以使用 WebDriver 构造函数（例如 new ChromeDriver()）。此外，我们还可以使用 WebDriver API 提供的构建器（即 RemoteWebDriver.builder()）。最后，我们可以使用 WebDriverManager 构建器在一行中解析驱动程序并创建 WebDriver 实例。以下示例展示了这些替代方案，结合浏览器 capabilities 以启用无头浏览器模式，即：

- Example 5-1 使用 Chrome 无头模式。此示例使用所需的构造函数（本例中为 `ChromeDriver`）创建 `WebDriver` 实例。
- Example 5-2 使用 Edge 无头模式。此示例使用 WebDriver API 中提供的构建器创建 `WebDriver` 实例。
- Example 5-3 使用 Firefox 无头模式。此示例使用 WebDriverManager 创建 `WebDriver` 实例。请注意，在这种情况下不需要 setup 方法，因为 WebDriverManager 在 WebDriver 实例化同一行中解析驱动程序。
- Example 5-4 使用 Selenium-Jupiter 通过 Chrome 无头模式。此示例使用 Selenium-Jupiter 提供的参数解析机制，因此我们在测试方法中简单声明一个 `ChromeDriver` 参数。然后，我们使用注解 `@Arguments` 装饰此参数以指定该浏览器的无头模式。

### Example 5-1. Test using Chrome in headless mode

```
class HeadlessChromeJupiterTest {

    WebDriver driver;

    @BeforeAll
    static void setupClass() {
        WebDriverManager.chromedriver().setup(); ①
    }

    @BeforeEach
    void setup() {
        ChromeOptions options = new ChromeOptions(); ②
        options.setHeadless(true); ③

        driver = new ChromeDriver(options); ④
    }

    @AfterEach
    void teardown() {
        driver.quit();
    }
```

```
```java
}

@Test
void testHeadless() {
    driver.get("https://bonigarcia.dev/selenium-webdriver-java/");
    assertThat(driver.getTitle()).contains("Selenium WebDriver");
}

}
```
```

- 1. 我们解析所需的驱动程序（本例中为 chromedriver）。
- 2. 我们使用 `ChromeOptions` 构造函数创建浏览器 capabilities。
- 3. 我们启用无头模式。此行等价于 `options.addArguments("--headless");`。
- 4. 我们通过将 options 作为构造函数参数传递给 `ChromeDriver` 构造函数来设置浏览器 capabilities。

### Example 5-2. Test using Edge in headless mode

```
```java
class HeadlessEdgeJupiterTest {

    WebDriver driver;

    @BeforeAll
    static void setupClass() {
        WebDriverManager.edgedriver().setup();
    }

    @BeforeEach
    void setup() {
        EdgeOptions options = new EdgeOptions();
        options.setHeadless(true);

        driver = RemoteWebDriver.builder().oneOf(options).build();
    }

    @AfterEach
```

```
void teardown() {
    driver.quit();
}

@Test
void testHeadless() {
    driver.get("https://bonigarcia.dev/selenium-webdriver-java/");
    assertThat(driver.getTitle()).contains("Selenium WebDriver");
}
}
```

- 1. 如常，我们需要解析所需的驱动程序（本例中为 msedgedriver）。
- 2. 由于我们打算使用 Edge，因此需要创建 EdgeOptions 实例来指定 capabilities。
- 3. 我们启用无头模式。同样，此行等价于 options.addArguments("--headless");。
- 4. 我们使用 WebDriver 构建器创建 WebDriver 对象，并将 options 作为参数传递。

### Example 5-3. Test using Firefox in headless mode

```
class HeadlessFirefoxJupiterTest {

    WebDriver driver;

    @BeforeEach
    void setup() {
        FirefoxOptions options = new FirefoxOptions(); ①
        options.setHeadless(true); ②

        driver =
        WebDriverManager.firefoxdriver().capabilities(options)
            .create(); ③
    }

    @AfterEach
```

```
void teardown() {
    driver.quit();
}

@Test
void testHeadless() {
    driver.get("https://bonigarcia.dev/selenium-webdriver-java/");
    assertThat(driver.getTitle()).contains("Selenium WebDriver");
}
```

① 我们在此测试中使用 Firefox，因此创建 FirefoxOptions 对象来指定 capabilities。

② 与前面的示例相同，我们启用无头模式。

③ 在此示例中，我们使用 WebDriverManager 解析所需的驱动程序并创建 WebDriver 对象，同时指定之前创建的浏览器 capabilities。

NOTE
在这些示例中用于创建 WebDriver 对象的策略是可以互换的。换句话说，例如，我们也可以对每个浏览器在无头模式下使用 WebDriverManager 构建器。

### Example 5-4. Test using Chrome in headless mode with Selenium-Jupiter

```
@ExtendWith(SeleniumJupiter.class)
class HeadlessChromeSelJupTest {

    @Test
    void testHeadless(@Arguments("--headless") ChromeDriver driver) {
        driver.get("https://bonigarcia.dev/selenium-webdriver-java/");
        assertThat(driver.getTitle()).contains("Selenium
```

```
WebDriver");
    }
}

```

① 我们使用注解 `@Arguments` 来指定浏览器（本例中为 Chrome）的无头模式。

## Page Loading Strategies

Selenium WebDriver 允许配置不同的 Web 页面加载方法。为此，WebDriver API 提供了 `PageLoadStrategy` 枚举。Table 5-2 描述了此枚举的可能值及其用途。Selenium WebDriver 内部使用 DOM API 的属性 `document.readyState` 来检查 Web 页面加载状态。





表 5-2. 页面加载策略值

| 加载策略 | 描述 | 就绪状态 |
|---|---|---|
| PageLoadStrategy.NORMAL | 默认模式。Selenium WebDriver 等待整个页面加载完成（即 HTML 内容和子资源，如样式表、图像、JavaScript 文件等） | "complete" |
| PageLoadStrategy.EAGER | Selenium WebDriver 等待 HTML 文档完成加载和解析，但子资源（脚本、图像、样式表等）仍在加载 | "interactive" |
| PageLoadStrategy.NONE | Selenium WebDriver 仅等待 HTML 文档下载完成 | "loading" |

我们需要调用浏览器功能（例如 ChromeOptions、FirefoxOptions 等）的 `setPageLoadStrategy()` 方法来设置这些策略（NORMAL、EAGER 或 NONE）。示例 5-5 显示了一个使用 Chrome 和 NORMAL 策略的测试。在示例存储库中，你可以找到使用其他策略（EAGER 和 NONE）为 Edge 和 Firefox 的等效示例。在这些示例中，除了在测试设置中指定加载策略外，测试逻辑还会计算加载页面所需的时间，并在标准输出中显示该值。

## 示例 5-5. 在 Chrome 中使用普通页面加载策略的测试

```
class PageLoadChromeJupiterTest {

    static final Logger log = getLogger(lookup().lookupClass());

    WebDriver driver;

    PageLoadStrategy pageLoadStrategy;

    @BeforeEach
    void setup() {
        ChromeOptions options = new ChromeOptions(); ①
        pageLoadStrategy = PageLoadStrategy.NORMAL;
        options.setPageLoadStrategy(pageLoadStrategy); ②

        driver =
WebDriverManager.chromedriver().capabilities(options).create(); ③
}
```

```
```java
}

@AfterEach
void teardown() {
    driver.quit();
}

@Test
void testPageLoad() {
    long initMillis = System.currentTimeMillis();
    driver.get("https://bonigarcia.dev/selenium-webdriver-java/");
    Duration elapsed = Duration
        .ofMillis(System.currentTimeMillis() - initMillis);

    Capabilities capabilities = ((RemoteWebDriver) driver)
        .getCapabilities();
    Object pageLoad = capabilities
        .getCapability(CapabilityType.PAGE_LOAD_STRATEGY);

    String browserName = capabilities.getBrowserName();
    log.debug(
        "The page took {} ms to be loaded using a '{}' strategy in {}",
        elapsed.toMillis(), pageLoad, browserName);

    assertThat(pageLoad).isEqualTo(pageLoadStrategy.toString());
}
```
```

+ ❶ 由于本测试使用 Chrome，我们实例化 `ChromeOptions` 以指定功能。
+ 
+ ❷ 我们将页面加载策略设置为 `NORMAL`。
+ 
+ ❸ 我们使用 WebDriverManager 解析驱动程序，创建 `WebDriver` 实例并指定功能。
+ 
+ ❹ 我们在加载页面之前获取系统时间戳。
+ 
+ ❺ 我们在加载页面之后获取系统时间戳。

+ 6 我们读取 `WebDriver` 对象的功能。
+ 7 我们读取使用的页面加载策略。
+ 8 我们追踪加载网页所需的时间。
+ 9 我们验证加载策略是否与初始配置一致。

## 设备仿真

主要网络浏览器允许使用开发工具模拟移动设备。此功能可通过开发工具使用，即 Chromium 系浏览器的 DevTools 和 Firefox 的 Developer Tools，通常包括：

+ - 模拟移动视口：使用给定移动设备的宽度和高度来减小网页的用户可见区域。
+ - 节流网络：减慢连接速度以模拟移动网络（例如 3G）。
+ - 节流 CPU：减慢处理性能。
+ - 模拟地理位置：设置自定义全球定位系统（GPS）坐标。
+ - 设置方向：旋转屏幕。

图 5-2 显示了 Chrome 使用 DevTools 进行移动仿真的屏幕截图。

## Hands-On Selenium WebDriver with Java

练习网站

该网站包含一系列用于使用 Selenium WebDriver 测试的示例网页。在 GitHub 上查看源代码。

## 第 3 章：WebDriver 基础

+ - Web 表单
+ - 导航
+ - 下拉菜单
+ - 鼠标悬停
+ - 拖放
+ - 在画布中绘制
+ - 加载图像
+ - 慢速计算器

## 第 4 章：浏览器无关功能

+ - 长页面
+ - 无限滚动
+ - Cookie

图 5-2. 使用 DevTools 在 Chrome 中进行移动仿真

截至本文撰写时，此移动设备仿真可以通过 Selenium WebDriver API 在 Chromium 系浏览器（Chrome 和 Edge）中自动化，但 Firefox 不行（因为 geckodriver 中未实现）。为此，我们需要在 `ChromeOptions` 或 `EdgeOptions` 中设置实验性选项 `mobileEmulation`。然后，有两种替代方法来指定要仿真的移动设备。首先，我们可以指定特定的移动设备（例如 Pixel 2、iPad Pro 或 Galaxy Fold，仅举几例）。由于此列表在每次 Chromium 发布时都会更新，最好的检查方法是在 DevTools 中检查可用设备（例如图 5-2 中选择了 iPhone X）。示例 5-6 显示了测试设置，其中我们使用标签 `iPhone 6/7/8` 指定给定的移动设备。

## 示例 5-6. 通过指定特定设备使用移动仿真的测试设置

```
```java
@BeforeEach
void setup() {
    ChromeOptions options = new ChromeOptions();
    Map<String, Object> mobileEmulation = new HashMap<>();
    mobileEmulation.put("deviceName", "iPhone 6/7/8");
    options.setExperimentalOption("mobileEmulation", mobileEmulation);

    driver =
        WebDriverManager.chromedriver().capabilities(options).create();
}
```
```

+ ❶ 我们需要创建一个 `HashMap` 对象来指定移动仿真选项。
+ 
+ ❷ 然后，我们只需要选择设备名称（本例中为 iPhone 6/7/8）。
+ 
+ ❸ 我们使用实验性选项设置设备仿真。
+ 
+ ❹ 像往常一样，我们创建 `WebDriver` 对象并指定这些选项。

设置移动仿真的第二种替代方法是指定仿真设备的各个属性。这些属性如下：

+ - `width`：设备屏幕宽度（以像素为单位）。
+ - `height`：设备屏幕高度（以像素为单位）。
+ - `pixelRatio`：物理像素与逻辑像素的比率。
+ - `touch`：是否仿真触摸事件。默认值是 `true`。

除了这些属性之外，我们还可以指定仿真设备的 *user agent*。在 HTTP 中，用户代理是请求头中指定的字符串，用于唯一标识 Web 浏览器类型。它包含开发代码名称、版本、平台和其他信息。**示例 5-7** 显示了测试设置，说明了此功能的使用。

## 示例 5-7. 通过指定各个属性使用设备仿真的测试设置

```
```java
@BeforeEach
void setup() {
    EdgeOptions options = new EdgeOptions();
    Map<String, Object> mobileEmulation = new HashMap<>();
    Map<String, Object> deviceMetrics = new HashMap<>();
    deviceMetrics.put("width", 360);
    deviceMetrics.put("height", 640);
    deviceMetrics.put("pixelRatio", 3.0);
    deviceMetrics.put("touch", true);
    mobileEmulation.put("deviceMetrics", deviceMetrics);
    mobileEmulation.put("userAgent",
            "Mozilla/5.0 (Linux; Android 4.2.1; en-us; Nexus 5
Build/JOP40D) "
            + "AppleWebKit/535.19 (KHTML, like Gecko) "
            + "Chrome/18.0.1025.166 Mobile Safari/535.19");

    options.setExperimentalOption("mobileEmulation",
            mobileEmulation);

    driver =
        WebDriverManager.edgedriver().capabilities(options).create();
}
```
```

+ 1. 我们创建一个哈希映射来存储仿真移动设备的各个属性，即 `width`、`height`、`pixelRatio` 和 `touch`。
+ 2. 我们通过在移动仿真映射中设置标签 `deviceMetrics` 来设置这些属性。
+ 3. 我们为 Nexus 5 设备中的 Chrome Mobile 18 设置自定义用户代理。

## Web 扩展

Web 扩展（也称为 *add-ons* 或 *plugins*）是可修改或增强 Web 浏览器默认操作的程序。用户通常使用网络商店安装 Web 扩展。这些商店是由浏览器维护者支持的 Web 应用程序，用于托管公共 Web 扩展。表 5-3 总结了 Chrome、Edge 和 Firefox 的网络商店。




## 表 5-3：主要浏览器对应的 Web 商店

| Web 商店 | 浏览器 | URL |
|---|---|---|
| Chrome 网上应用店 | Chrome | https://chrome.google.com/webstore/category/extensions |
| Edge 加载项 | Edge | https://microsoftedge.microsoft.com/addons/Microsoft-Edge-Extensions-Home |
| Firefox 浏览器扩展 | Firefox | https://addons.mozilla.org/en-US/firefox/ |

我们可以使用 capabilities 在 WebDriver 会话中安装 Web 扩展。为此，在基于 Chromium 的浏览器（如 Chrome 和 Firefox）中，我们使用 `ChromeOptions` 或 `EdgeOptions` 对象的 `addExtensions()` 方法。**示例 5-8** 展示了在 Chrome 中安装本地扩展的测试设置。

### 示例 5-8：在 Chrome 中安装 Web 扩展的测试设置

```java
@BeforeEach
void setup() throws URISyntaxException {
    Path extension = Paths
            .get(ClassLoader.getSystemResource("dark-bg.crx").toURI());
    ChromeOptions options = new ChromeOptions();
    options.addExtensions(extension.toFile());

    driver =
WebDriverManager.chromedriver().capabilities(options).create();
}
```

我们安装的是打包为 Chrome 扩展（CRX）文件的 Web 扩展。这个文件是一个测试资源（位于 Java 项目的 `src\test\resources` 文件夹中）。此扩展会改变网站的外观和感觉，使其在深色背景上使用浅色文字。**图 5-3** 展示了使用此扩展的 WebDriver 测试加载练习网站时的截图。

- ② 我们通过将扩展作为 Java `File` 传入，在 Chrome 选项中添加该扩展。

![](img/b67ec8e49249e340add6ed66ab413d78_270_0.png)

*图 5-3：使用 dark-bg.crx 扩展加载的练习网站*

Firefox 在由 WebDriver 控制时也允许加载 Web 扩展。不过，语法有所不同。**示例 5-9** 说明了这种用法。

### 示例 5-9：在 Firefox 中安装 Web 扩展的测试设置

```java
@BeforeEach
void setup() throws URISyntaxException {
    Path extension = Paths
        .get(ClassLoader.getSystemResource("dark-bg.xpi").toURI());
    FirefoxOptions options = new FirefoxOptions();
    FirefoxProfile profile = new FirefoxProfile();
}
```

```java
profile.addExtension(extension.toFile());
options.setProfile(profile);

driver = WebDriverManager.firefoxdriver().capabilities(options)
        .create();
}
```

- 1. 我们使用与 Chrome/Edge 中相同的扩展，但在这种情况下，打包格式是 Firefox 专用的。请注意，文件不同。这次它被打包为 *XPInstall* 文件，即一个包含 Web 扩展源代码、资源（如图像）和元数据的压缩归档。
- 2. 我们需要创建一个自定义的 Firefox 配置文件（即存储自定义设置的地方）。
- 3. 我们将扩展作为 Java `File` 添加到 Firefox 配置文件中。
- 4. 我们在 Firefox 选项中设置该配置文件。

基于 Chromium 的浏览器（如 Chrome、Edge）也允许从其源代码加载扩展（即不打包为 CRX 文件）。这一特性在 Web 扩展开发期间进行自动化测试时非常方便。*示例 5-10* 展示了说明此特性的测试设置。

### 示例 5-10：在 Edge 中从源代码安装 Web 扩展的测试设置

```java
@BeforeEach
void setup() throws URISyntaxException {
    Path extension = Paths
            .get(ClassLoader.getSystemResource("web-extension").toURI());
    EdgeOptions options = new EdgeOptions();
    options.addArguments(
            "--load-extension=" +
            extension.toAbsolutePath().toString());

    driver =
WebDriverManager.edgedriver().capabilities(options).create();
}
```

❶ 本示例中使用的扩展位于 `web-extension` 文件夹中，它是一个测试资源文件夹（存储在 Java 项目的 `src\test\resources` 中）。该扩展遵循 [浏览器扩展 API](...)。它使用 JavaScript 将一级标题（`h1` 标签）的内容更改为自定义消息。**图 5-4** 展示了使用此扩展时练习网站的截图。

❷ 我们使用参数 `--load-extension` 指定扩展路径。

> **注意**
>
> Firefox 目前（写作本文时）不允许从源代码加载 Web 扩展。不过，有一个未解决的问题请求此功能，也许在未来的 Selenium WebDriver 版本中会提供。

![](img/b67ec8e49249e340add6ed66ab413d78_272_0.png)

*图 5-4：使用本地扩展加载的练习网站*

## 地理位置

**地理位置 API** 是一项 W3C 规范，允许访问与 Web 浏览器的主机设备（如笔记本电脑或移动设备）相关的地理位置信息。常见的地理位置数据源包括 GPS 数据以及从网络推断出的位置（如 IP 地址）。地理位置 API 可通过调用 JavaScript 对象 `navigator.geolocation` 在 Web 浏览器中使用。出于隐私原因，使用此语句时，会向用户弹出是否允许报告位置数据的提示。

练习网站包含一个使用地理位置功能的网页。**图 5-5** 显示了该页面的截图。该图显示了用户单击“获取坐标”按钮时显示的权限弹出窗口。要使用 WebDriver API 处理此对话框，我们使用 capabilities。与其他场合一样，授予地理位置数据访问权限所需的 capabilities 在 Chrome/Edge 中与 Firefox 中有所不同。以下代码片段展示了差异。首先，**示例 5-11** 展示了在 Chrome 中授予地理位置访问权限的测试设置。相同的实验性偏好设置（`profile.default_content_setting_values.geolocation`）也用于 Edge（像往常一样，你可以在示例仓库中找到完整测试）。然后，**示例 5-12** 展示了等效的测试设置，但使用 Firefox。

# 使用 Java 的 Selenium WebDriver

Practi

bonigarcia.dev 想要
知道你的位置

允许

阻止

# 地理位置

获取坐标

版权所有 © 2021 Boni García

图 5-5：显示地理位置权限弹出窗口的练习网站

### 示例 5-11：在 Chrome 中允许地理位置的测试设置

```
@BeforeEach
void setup() {
    ChromeOptions options = new ChromeOptions();
    Map<String, Object> prefs = new HashMap<>(); ①
    prefs.put("profile.default_content_setting_values.geolocation", 1); ②
    options.setExperimentalOption("prefs", prefs); ③

    driver =
WebDriverManager.chromedriver().capabilities(options).create();
}
```

① 我们创建一个哈希映射，用于存放实验性选项。

② 我们将实验性选项 profile.default_content_setting_values.geolocation 设置为 1，以允许访问地理位置。其他可能的值有：0 表示默认行为，2 表示阻止访问地理位置数据。

③ 我们在 Chrome 选项中使用标签 prefs 设置实验性选项。

# 注意

假设你需要在 macOS 机器上使用 Chrome 或 Edge 访问地理位置坐标。在这种情况下，你还需要在 macOS 偏好设置（系统偏好设置 → 安全性与隐私 → 定位服务）中为这些浏览器启用定位服务。图 5-6 显示了此配置。

# 安全性与隐私

- 通用
- 文件保险箱
- 防火墙
- 隐私

- 定位服务
- 联系人
- 日历
- 提醒事项
- 照片
- 相机
- 麦克风
- 语音识别
- 辅助功能

启用定位服务

允许以下应用和服务确定你的位置。

- Microsoft Edge
- Siri 与听写
- Google Chrome
- 系统服务  详细信息...

表示某个应用在过去 24 小时内使用过你的位置。

关于定位服务与隐私...

点按锁按钮以防止进一步更改

高级...

## 使用 Java 的 Selenium WebDriver 实战

## 通知

通知我

![](img/b67ec8e49249e340add6ed66ab413d78_278_0.png)

![](img/b67ec8e49249e340add6ed66ab413d78_278_1.png)

图 5-7：显示通知权限弹出窗口的练习网站





# Hands-On Selenium WebDriver with Java

练习网站

## Notifications

- Notify me

![](img/b67ec8e49249e340add6ed66ab413d78_279_0.png)

图 5-8. 在 Linux 桌面上显示通知的练习网站

WebDriver API 允许通过使用 capabilities 来授予通知权限。与其他功能一样，这些 capabilities 的语法在 Chrome/Edge 和 Firefox 中不同。示例 5-13 显示了以 Chrome 选项启用通知的测试设置。我们使用相同的偏好设置（profile.default_content_setting_values.notifications）来允许 Edge 中的通知。示例 5-14 显示了 Firefox 的等效测试设置。在这种情况下，偏好设置标签（permissions.default.desktop-notification）不同，尽管其值（1）对于允许通知是相同的。另一个可能的值是 2，用于阻止通知（在 Chrome/Edge 和 Firefox 中均适用）。

### Example 5-13. Test setup to allow notifications in Chrome

```
@BeforeEach
void setup() {
    ChromeOptions options = new ChromeOptions();
    Map<String, Object> prefs = new HashMap<>();

    prefs.put("profile.default_content_setting_values.notifications", 1);
    options.setExperimentalOption("prefs", prefs);

    driver =
WebDriverManager.chromedriver().capabilities(options).create();
}
```

### Example 5-14. Test setup to allow notifications in Firefox

```
@BeforeEach
void setup() {
    FirefoxOptions options = new FirefoxOptions();
    options.addPreference("permissions.default.desktop-notification", 1);

    driver = WebDriverManager.firefoxdriver().capabilities(options)
        .create();
}
```

示例 5-15 显示了与之前设置一起使用的测试逻辑。像往常一样，你可以在 examples 仓库中找到完整的测试用例。此测试是异步脚本执行的示例。此脚本覆盖了原始的 `Notification` JavaScript 对象。此对象的新实现获取通知消息的标题，该标题在脚本回调中返回给 WebDriver 测试。

### Example 5-15. Test handling notifications

```
```java
@Test
void testNotifications() {
    driver.get(
        "https://bonigarcia.dev/selenium-webdriver-
        java/notifications.html");
    JavascriptExecutor js = (JavascriptExecutor) driver;

    String script = String.join("\n",
        "const callback = arguments[arguments.length - 1];",
        "const OldNotify = window.Notification;",
        "function newNotification(title, options) {",
        "    callback(title);",
        "    return new OldNotify(title, options);",
        "}",
        "newNotification.requestPermission = " +
        "    OldNotify.requestPermission.bind(OldNotify);",
        "Object.defineProperty(newNotification, 'permission',",
        "{",
        "    get: function() {",
        "        return OldNotify.permission;",
        "    }",
        "});",
        "window.Notification = newNotification;",
        "document.getElementById('notify-me').click();");
    log.debug("Executing the following script asynchronously:\n{}",
        script);

    Object notificationTitle = js.executeAsyncScript(script);
    assertThat(notificationTitle).isEqualTo("This is a
    notification");
}
```
```

1. 如异步脚本执行中通常一样，最后一个参数是用于发出脚本终止信号的回调函数。
2. 我们保存原始 `Notification` 构造函数的副本。

3 我们创建了一个新的通知构造函数。
4 我们将消息标题作为参数传递给回调。因此，标题被返回到 WebDriver 调用（本例中为 Java）。
5 我们使用旧构造函数创建原始 Notification 对象。
6 我们点击网页上触发通知的按钮。
7 我们在脚本执行后获取返回的对象。
8 我们验证通知标题是否符合预期。

## Browser Binary

Selenium WebDriver 默认情况下即可检测受控 Web 浏览器的路径（Chrome、Firefox 等）。尽管如此，我们仍可使用 capabilities 指定浏览器可执行文件的自定义路径。此功能在浏览器安装路径非标准时很有帮助（例如在 beta/开发版/canary 浏览器情况下）。

我们使用相同的 capabilities 语法为 Chrome、Edge 和 Firefox 指定二进制路径。示例 5-16 显示了使用 Chrome beta 的测试设置。

### Example 5-16. Test setup setting a custom binary path for Chrome

```
@BeforeEach
void setup() {
    Path browserBinary = Paths.get("/usr/bin/google-chrome-beta");
    assumeThat(browserBinary).exists();
    ChromeOptions options = new ChromeOptions();
    options.setBinary(browserBinary.toFile());
    driver =
WebDriverManager.chromedriver().capabilities(options).create();
}
```

我们使用 Java `Path` 获取浏览器二进制路径（本例中为 Linux 中的 Chrome beta）。

2. 我们使用假设来有条件地跳过此测试，当前一个路径不存在时（例如在 CI 服务器中）。

3. 我们使用 Chrome 选项的 `setBinary` 方法设置二进制路径（作为 Java `File`）。

## Get User Media

WebRTC 是一组标准技术，允许使用 Web 浏览器交换实时媒体。该技术允许使用客户端的 JavaScript API 创建音频和视频会议 Web 应用。练习网站包含一个使用 *getUserMedia* JavaScript API 获取用户媒体（麦克风和网络摄像头）的网页。像其他 API 一样，出于安全和隐私考虑，浏览器弹出窗口会在访问用户媒体前请求权限。图 5-9 显示了提示此对话框的示例网页。

# Hands-On Selenium WebDriver with Java

bonigarcia.dev wants to
Use your microphone
Use your camera

Allow

Block

# Get user media

Start

Copyright © 2021 Boni García

图 5-9. 练习网站提示用户媒体权限弹出窗口

我们使用 capabilities 在 WebDriver API 中授予用户媒体访问权限。这些 capabilities 的语法在 Chrome 和 Edge 中相同（见示例 5-17），但在 Firefox 中不同（见示例 5-18）。

### Example 5-17. Test setup granting synthetic user media in Chrome

```
@BeforeEach
void setup() {
    ChromeOptions options = new ChromeOptions();
    options.addArguments("--use-fake-ui-for-media-stream"); ①
    options.addArguments("--use-fake-device-for-media-stream"); ②
    driver =
WebDriverManager.chromedriver().capabilities(options).create();
}
```

① 参数允许访问用户媒体（音频和视频）。

② 参数使用合成视频（绿色旋转器）和音频（每秒一声哔声）伪造用户媒体。你可以在图 5-10 中看到此视频。

![](img/b67ec8e49249e340add6ed66ab413d78_285_0.png)

图 5-10. Chrome 中使用合成用户媒体的练习网站

### Example 5-18. Test setup granting synthetic user media in Firefox

```
@BeforeEach
void setup() {
    FirefoxOptions options = new FirefoxOptions();
    options.addPreference("media.navigator.permission.disabled", true); ①
    options.addPreference("media.navigator.streams.fake", true); ②

    driver = WebDriverManager.firefoxdriver().capabilities(options)
        .create();
}
```

① 偏好设置用于访问用户媒体。

② 偏好设置使用合成视频（带变化颜色背景）和音频（持续哔声）伪造用户媒体。你可以在图 5-11 中看到此视频。

![](img/b67ec8e49249e340add6ed66ab413d78_286_0.png)

## Loading Insecure Pages

当 Web 浏览器尝试使用 HTTPS（Hypertext Transfer Protocol Secure）加载网页，但服务器端的证书无效时，浏览器会警告用户。无效证书的示例包括自签名、已撤销或密码学上不安全的证书。图 5-12 显示了 Chrome 中此警告的屏幕截图）。

# Your connection is not private

Attackers might be trying to steal your information from self-signed.badssl.com (for example, passwords, messages, or credit cards). Learn more

NET::ERR_CERT_AUTHORITY_INVALID

To get Chrome's highest level of security, turn on enhanced protection

Advanced

Back to safety

图 5-12. 使用不安全证书的网页

此问题并不一定意味着安全问题。它可能发生在网站开发期间，例如在使用自签名



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





```
long initMillis = System.currentTimeMillis(); ③
driver.get("https://bonigarcia.dev/selenium-webdriver-java/");

Duration elapsed = Duration
    .ofMillis(System.currentTimeMillis() - initMillis); ⑤
log.debug("The page took {} ms to be loaded",
elapsed.toMillis());

assertThat(driver.getTitle()).contains("Selenium WebDriver");
}
```

- ① 我们激活网络追踪（不调整任何网络参数）。
- ② 我们模拟移动 3G 网络，下行和上行带宽均为 50 KBps。
- ③ 我们在加载网页前获取系统时间戳。
- ④ 我们加载练习站点的首页。
- ⑤ 我们计算加载此页所需的时间。

## Network Monitoring

我们还可以使用 CDP 在与网页交互时监控网络流量。示例 5-27 显示了一个使用此功能的测试。该测试使用 DevTools 对象的 `addListener()` 方法来追踪 HTTP 请求和响应。

### Example 5-27. Test monitoring HTTP requests and responses

```
@Test
void testNetworkMonitoring() {
    devTools.send(Network.enable(Optional.empty(),
Optional.empty(),
        Optional.empty()));

    devTools.addListener(Network.requestWillBeSent(), request -> {
        log.debug("Request {}", request.getRequestId());
        log.debug("\t Method: {}",
request.getRequest().getMethod());
        log.debug("\t URL: {}", request.getRequest().getUrl());
logHeaders(request.getRequest().getHeaders());
});

 devTools.addListener(Network.responseReceived(), response -> {
     log.debug("Response {}", response.getRequestId());
     log.debug("\t URL: {}", response.getResponse().getUrl());
     log.debug("\t Status: {}",
 response.getResponse().getStatus());
     logHeaders(response.getResponse().getHeaders());
 });

driver.get("https://bonigarcia.dev/selenium-webdriver-java/");
assertThat(driver.getTitle()).contains("Selenium WebDriver");
}

void logHeaders(Headers headers) {
    log.debug("\t Headers:");
    headers.toJson().forEach((k, v) -> log.debug("\t\t{}:{}", k,
v));
}
```

- 1. 我们为 HTTP 请求创建监听器，并在控制台中记录捕获的数据。
- 2. 我们为 HTTP 响应创建监听器，并在控制台中记录捕获的数据。

## Performance Metrics

CDP 允许收集运行时性能指标，例如加载的文档数量、DOM 节点数量、DOM 加载时间、脚本持续时间等。示例 5-28 显示了一个收集这些指标并在标准输出中显示的测试。

### Example 5-28. Test gathering performance metrics

```
@Test
void testPerformanceMetrics() {
    devTools.send(Performance.enable(Optional.empty()));
    driver.get("https://bonigarcia.dev/selenium-webdriver-java/");

    List<Metric> metrics = devTools.send(Performance.getMetrics());

    assertThat(metrics).isNotEmpty();

metrics.forEach(metric -> log.debug("{}: {}", metric.getName(),
    metric.getValue()));
}
```

- ① 我们启用指标收集。
- ② 我们获取所有指标。

## Extra Headers

CDP 允许在 HTTP 层添加额外头部。为此，我们需要在 CDP 会话中发送命令 `Network.setExtraHTTPHeaders()`。示例 5-29 显示了一个使用此命令添加 HTTP 头部 `Authorization` 的测试，用于在需要基本身份验证登录的网页中发送凭据（用户名和密码）。

### Example 5-29. Test adding extra HTTP headers

```
@Test
void testExtraHeaders() {
    devTools.send(Network.enable(Optional.empty(),
Optional.empty(),
        Optional.empty()));

    String userName = "guest";
    String password = "guest";
    Map<String, Object> headers = new HashMap<>();
    String basicAuth = "Basic " + new String(Base64.getEncoder()
        .encode(String.format("%s:%s", userName,
password).getBytes()));
    headers.put("Authorization", basicAuth); ①
    devTools.send(Network.setExtraHTTPHeaders(new
Headers(headers))); ②

    driver.get("https://jigsaw.w3.org/HTTP/Basic/"); ③
    String bodyText =
driver.findElement(By.tagName("body")).getText();
    assertThat(bodyText).contains("Your browser made it!"); ④
}
```

- ① 我们将用户名和密码编码为 Base64。
- ② 我们创建授权头部。
- 3 我们打开一个受基本身份验证保护的网页。
- 4 我们验证页面是否正确显示。

## Block URLs

CDP 提供了在会话中阻止给定 URL 的能力。示例 5-30 提供了一个阻止练习网站 Logo URL 的测试。如果你在执行期间检查浏览器，你会发现该 Logo 未在页面上显示。

### Example 5-30. Test blocking an URL

```
@Test
void testBlockUrl() {
    devTools.send(Network.enable(Optional.empty(),
Optional.empty(),
        Optional.empty()));

    String urlToBlock =
        "https://bonigarcia.dev/selenium-webdriver-java/img/hands-on-icon.png";

devTools.send(Network.setBlockedURLs(ImmutableList.of(urlToBlock)))
; ①

    devTools.addListener(Network.loadingFailed(), loadingFailed ->
{
        BlockedReason reason =
loadingFailed.getBlockedReason().get();
        log.debug("Blocking reason: {}", reason);
        assertThat(reason).isEqualTo(BlockedReason.INSPECTOR);
    }); ②

    driver.get("https://bonigarcia.dev/selenium-webdriver-java/");
    assertThat(driver.getTitle()).contains("Selenium WebDriver");
}
```

- ① 我们阻止给定的 URL。
- ② 我们创建监听器以追踪失败事件。

## Device Emulation

CDP 提供的另一项功能是能够模拟移动设备（例如智能手机、平板电脑）。示例 5-31 说明了此用法。该测试首先通过发送命令 `Network.setUserAgentOverride()` 来覆盖用户代理。然后，它通过发送命令 `Emulation.setDeviceMetricsOverride` 来模拟设备指标。

### Example 5-31. Test emulating a mobile device

```
@Test
void testDeviceEmulation() {
    // 1. Override user agent (Apple iPhone 6)
    String userAgent = "Mozilla/5.0 (iPhone; CPU iPhone OS 8_0 like Mac OS X) "
        + "AppleWebKit/600.1.3 (KHTML, like Gecko) "
        + "Version/8.0 Mobile/12A4345d Safari/600.1.4";
    devTools.send(Network.setUserAgentOverride(userAgent,
        Optional.empty(),
        Optional.empty(), Optional.empty())); ①

    // 2. Emulate device dimension
    Map<String, Object> deviceMetrics = new HashMap<>();
    deviceMetrics.put("width", 375);
    deviceMetrics.put("height", 667);
    deviceMetrics.put("mobile", true);
    deviceMetrics.put("deviceScaleFactor", 2);
    ((ChromeDriver) driver).executeCdpCommand(
        "Emulation.setDeviceMetricsOverride", deviceMetrics); ②

    driver.get("https://bonigarcia.dev/selenium-webdriver-java/");
    assertThat(driver.getTitle()).contains("Selenium WebDriver");
}
```

① 我们覆盖用户代理以模拟 Apple iPhone 6。

② 我们覆盖设备屏幕参数。

## Console Listeners

CDP 允许你实现监听器来监控控制台事件，即网页 JavaScript 日志和错误追踪。示例 5-32 显示了相关测试。该测试使用练习站点中的一个网页，该网页有意追踪多个 JavaScript 消息（使用 `console.log()`、`console.error()` 等命令），并抛出 JavaScript 异常。

### Example 5-32. Test listening to console events

```
```java
@Test
void testConsoleListener() {
    devTools.getDomains().events().addConsoleListener(consoleEvent -> {
        log.debug("{} {}: {}", consoleEvent.getTimestamp(),
                consoleEvent.getType(),
        consoleEvent.getMessages());
    });

    devTools.getDomains().events()
            .addJavascriptExceptionListener(jsException -> {
                log.debug("JavascriptException: {} {}",
                        jsException.getMessage(),
                        jsException.getSystemInformation());
            });

    driver.get(
        "https://bonigarcia.dev/selenium-webdriver-java/console-logs.html");
}
```
```

- 1. 我们为控制台事件创建监听器。
- 2. 我们为 JavaScript 错误创建另一个监听器。

## Geolocation Override

CDP 提供的另一项功能是能够覆盖托管设备处理的地理位置坐标。**Example 5-33** 演示了如何执行此操作。该测试发送命令 `Emulation.setGeolocationOverride()`，该命令接受三个可选参数：纬度、经度和精度。

### Example 5-33. Test overriding location coordinates

```
```java
@Test
void testGeolocationOverride() {
```




```
devTools.send(Emulation.setGeolocationOverride(Optional.of(48.8584),
        Optional.of(2.2945), Optional.of(100)));

driver.get(
        "https://bonigarcia.dev/selenium-webdriver-java/geolocation.html");
driver.findElement(By.id("get-coordinates")).click();

WebDriverWait wait = new WebDriverWait(driver,
        Duration.ofSeconds(5));
WebElement coordinates =
driver.findElement(By.id("coordinates"));
wait.until(ExpectedConditions.visibilityOf(coordinates));
}
```

> ❶ 我们使用埃菲尔铁塔（法国巴黎）的坐标来覆盖地理位置。

> ❷ 我们打开一个访问设备位置并向用户显示坐标的练习网页。

## 管理 Cookie

CDP 还允许管理 Web Cookie。示例 5-34 展示了一个测试，该测试读取一个管理某些 Cookie 的练习页面的 Cookie。

### 示例 5-34. 管理 Cookie 的测试

```
@Test
void testManageCookies() {
    devTools.send(Network.enable(Optional.empty(),
            Optional.empty(),
            Optional.empty()));
    driver.get(
            "https://bonigarcia.dev/selenium-webdriver-java/cookies.html");

    // Read cookies
    List<Cookie> cookies = devTools.send(Network.getAllCookies());

    cookies.forEach(cookie -> log.debug("{} = {}", cookie.getName(),
            cookie.getValue()));
    List<String> cookieName = cookies.stream()
            .map(cookie -> cookie.getName()).sorted()
.collect(Collectors.toList());
Set<org.openqa.selenium.Cookie> seleniumCookie =
driver.manage()
    .getCookies();
List<String> selCookieName = seleniumCookie.stream()
    .map(selCookie -> selCookie.getName()).sorted()
    .collect(Collectors.toList());
assertThat(cookieName).isEqualTo(selCookieName); ②

// Clear cookies
devTools.send(Network.clearBrowserCookies()); ③
List<Cookie> cookiesAfterClearing = devTools
    .send(Network.getAllCookies());
assertThat(cookiesAfterClearing).isEmpty(); ④

driver.findElement(By.id("refresh-cookies")).click();
}
```

- ① 我们读取网页的所有 Cookie。
- ② 我们验证使用 CDP 命令读取的 Cookie 与使用 WebDriver API（使用 getCookies();）读取的 Cookie 相同。
- ③ 我们移除所有 Cookie。
- ④ 我们验证此时没有任何 Cookie。

## 加载不安全页面

CDP 还允许加载不安全的网页（即使用 HTTPS 但证书无效的网页）。示例 5-35 演示了这一特性。

### 示例 5-35. 测试加载不安全网页

```
@Test
void testLoadInsecure() {
    devTools.send(Security.enable()); ①
    devTools.send(Security.setIgnoreCertificateErrors(true)); ②
    driver.get("https://expired.badssl.com/");

    String bgColor = driver.findElement(By.tagName("body"))
.getCssValue("background-color");
Color red = new Color(255, 0, 0, 1);
assertThat(Color.fromString(bgColor)).isEqualTo(red);
}
```

- 1. 我们启用安全跟踪。
- 2. 我们忽略证书错误。
- 3. 我们验证页面是否正确加载。

## 网络拦截器

WebDriver API 包含一组基于 CDP 构建的辅助类。这些类旨在提供友好的 API，以便为 WebDriver 测试启用高级功能。这些功能的缺点是所使用的浏览器必须支持 CDP。因此，它们适用于 Chrome 和 Edge，但在其他浏览器（如 Firefox）中不可用。

此类辅助类的第一个是 `NetworkInterceptor`。该类允许对后端请求进行桩替换、拦截网络流量，并返回预设的响应。该功能可能有助于通过用快速直接的响应模拟外部调用来简化复杂的端到端测试。要实例化 `NetworkInterceptor`，我们需要在其构造函数中指定两个参数（参见示例 5-36）：

- 一个实现 CDP 的 `WebDriver` 对象（即 `ChromeDriver` 或 `EdgeDriver`）。
- 一个 `Route` 对象，用于将网络请求映射到响应。

### 示例 5-36. 使用 NetworkInterceptor 拦截网络流量的测试

```
@Test
void testNetworkInterceptor() throws Exception {
    Path img = Paths
        .get(ClassLoader.getSystemResource("tools.png").toURI());
    byte[] bytes = Files.readAllBytes(img);
try (NetworkInterceptor interceptor = new
NetworkInterceptor(driver,
    Route.matching(req -> req.getUri().endsWith(".png"))
        .to(() -> req -> new HttpResponse()
            .setContent(Contents.bytes(bytes))))) {
    driver.get("https://bonigarcia.dev/selenium-webdriver-java/");
    int width =
Integer.parseInt(driver.findElement(By.tagName("img"))
        .getAttribute("width"));
    assertThat(width).isGreaterThan(80);
}
```

- 1 我们加载一个作为测试资源存储在 Java 项目中的本地图像。
- 2 我们创建一个网络拦截器实例，为所有以 .png 结尾的请求创建路由，并用新的响应桩替换该请求，本例中发送的是前面图片的内容。
- 3 我们打开练习网站。
- 4 如果拦截按预期工作，页面上的图像宽度应大于原始徽标。

> 注意
> 如果使用不同于 Chrome 或 Edge 的浏览器（如 Firefox）运行前面的代码，将抛出 DevToolsException。

## 整页截图

CDP 的另一种可能用法是截取整页截图（即捕获视口之外的页面内容）。示例 5-37 演示了 Chrome 中的这一功能。

### 示例 5-37. 使用 CDP 在 Chrome 中截取整页截图的测试

```
@Test
void testFullPageScreenshotChrome() throws IOException {
    driver.get(
        "https://bonigarcia.dev/selenium-webdriver-java/long-page.html");

    WebDriverWait wait = new WebDriverWait(driver,
        Duration.ofSeconds(10));

    wait.until(ExpectedConditions.presenceOfNestedElementsLocatedBy(
        By.className("container"), By.tagName("p")));

    GetLayoutMetricsResponse metrics = devTools
        .send(Page.getLayoutMetrics());
    Rect contentSize = metrics.getContentSize();
    String screenshotBase64 = devTools
        .send(Page.captureScreenshot(Optional.empty(),
            Optional.empty(),
            Optional.of(new Viewport(0, 0,
                contentSize.getWidth(),
                contentSize.getHeight(), 1)),
            Optional.empty(), Optional.of(true)));
    Path destination = Paths.get("fullpage-screenshot-chrome.png");
    Files.write(destination,
        Base64.getDecoder().decode(screenshotBase64));

    assertThat(destination).exists();
}
```

- 1. 我们加载包含长文本的练习页面（因此其内容超出标准视口）。
- 2. 我们等待段落加载完成。
- 3. 我们获取页面布局指标（以计算页面尺寸）。
- 4. 我们发送 CDP 命令以截取超出页面视口的截图。结果，我们获得一个 Base64 字符串形式的截图。
- 5. 我们将 Base64 内容解码为 PNG 文件。
- 6. 我们断言测试结束时 PNG 文件存在。

与往常一样，此功能在其他实现 CDP 的浏览器（如 Edge）中也可用。然而，它在 Firefox 中不可用。幸运的是，该浏览器通过 FirefoxDriver 对象中可用的 `getFullPageScreenshotAs()` 方法支持同样的特性。示例 5-38 展示了一个使用此方法和 Firefox 的测试。

### 示例 5-38. 使用 Firefox 截取整页截图的测试

```
@Test
void testFullPageScreenshotFirefox() throws IOException {
    driver.get(
        "https://bonigarcia.dev/selenium-webdriver-java/long-page.html");
    WebDriverWait wait = new WebDriverWait(driver,
    Duration.ofSeconds(10));

    wait.until(ExpectedConditions.presenceOfNestedElementsLocatedBy(
        By.className("container"), By.tagName("p")));

    byte[] imageBytes = ((FirefoxDriver) driver)
        .getFullPageScreenshotAs(OutputType.BYTES); ①
    Path destination = Paths.get("fullpage-screenshot-firefox.png");
    Files.write(destination, imageBytes);

    assertThat(destination).exists();
}
```

① 我们截取整个页面。与常规截图类似（参见第 4 章的表 4-2），输出类型可以是 FILE、BASE64 或 BYTES。我们使用后者将截图作为字节数组获取。

## 位置上下文

WebDriver API 提供了 LocationContext 接口，用于模拟所用设备的地理位置坐标。该接口由 ChromeDriver、EdgeDriver 和 OperaDriver 实现。因此，这些驱动程序可以调用 setLocation() 方法来指定自定义坐标（纬度、经度和海拔）。示例 5-39 展示了一个使用此功能的基本测试。





## 示例 5-39. 通过 LocationContext 测试设置自定义地理位置坐标

```java
@Test
void testLocationContext() {
    LocationContext location = (LocationContext) driver; ①
    location.setLocation(new Location(27.5916, 86.5640, 8850)); ②

    driver.get(
        "https://bonigarcia.dev/selenium-webdriver-java/geolocation.html"); ③
    driver.findElement(By.id("get-coordinates")).click();

    WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(5));
    WebElement coordinates =
        driver.findElement(By.id("coordinates"));
    wait.until(ExpectedConditions.visibilityOf(coordinates)); ④
}
```

- ① 我们将 driver 对象强制转换为 LocationContext（仅对 Chrome、Edge 或 Opera 可行）。
- ② 我们打开一个练习页面，在该页面中地理位置坐标会显示给最终用户。
- ③ 我们设置自定义位置，此处为珠穆朗玛峰的坐标（位于尼泊尔-中国边境）。
- ④ 我们断言坐标在页面上可见。

## WebDriver BiDi

WebDriver BiDi 是 W3C 推荐标准，定义了双向 WebDriver 协议。与 WebDriver 协议严格的命令/响应格式不同，BiDi 在驱动程序和浏览器之间引入 WebSocket 连接以实现双向通信。

得益于 BiDi，WebDriver API 允许在不轮询浏览器以获取响应的情况下实现监听器事件。截至本文撰写时，BiDi 仅在基于 Chromium 的浏览器中通过接口 `HasLogEvents` 可用。该接口允许为以下事件实现监听器：

- `domMutation`：用于捕获 DOM 变化的事件。**示例 5-40** 显示了一个实现这些事件监听器的测试。
- `consoleEvent`：用于捕获浏览器控制台变化的事件，例如 JavaScript 轨迹。**示例 5-41** 显示了第二个实现此类型监听器的测试。

## 示例 5-40. 测试实现 DOM 变更事件监听器

```java
@Test
void testDomMutation() throws InterruptedException {
    driver.get("https://bonigarcia.dev/selenium-webdriver-java/");

    HasLogEvents logger = (HasLogEvents) driver;
    JavascriptExecutor js = (JavascriptExecutor) driver;

    AtomicReference<DomMutationEvent> seen = new AtomicReference<>();
    CountDownLatch latch = new CountDownLatch(1);
    logger.onLogEvent(CdpEventTypes.domMutation(mutation -> {
        seen.set(mutation);
        latch.countDown();
    }));

    WebElement img = driver.findElement(By.tagName("img"));
    String newSrc = "img/award.png";
    String script = String.format("arguments[0].src = '%s';", newSrc);
    js.executeScript(script, img);

    assertThat(latch.await(10, TimeUnit.SECONDS)).isTrue();
    assertThat(seen.get().getElement().getAttribute("src"))
        .endsWith(newSrc);
}
```

❶ 我们将 driver 对象强制转换为 `HasLogEvents`。此强制转换仅对 Chrome 和 Edge 可行。

HTTP 提供了两种内置的识别用户身份的机制，称为 *basic* 和 *digest* 认证。两种方法都允许使用一对值指定用户凭据：用户名和密码。它们之间的区别在于如何通信凭据。一方面，摘要认证方法通过对用户名和密码应用哈希函数发送加密的凭据。另一方面，基本认证使用 Base64 对凭据进行编码（而非加密）。

Selenium WebDriver 提供了接口 `HasAuthentication` 以无缝实现基本和摘要认证。该接口由 `ChromiumDriver` 继承。因此，我们只能在 Chrome 和 Edge 中使用它。**示例 5-42** 显示了一个使用 Chrome 和基本认证的测试。你可以使用完全相同的机制在 Edge 中使用摘要认证（请参阅示例仓库中的完整测试）。

## 示例 5-42. 使用 Chrome 测试基本认证

```java
@Test
void testBasicAuth() {
    ((HasAuthentication) driver)
        .register(() -> new UsernameAndPassword("guest", "guest"));

    driver.get("https://jigsaw.w3.org/HTTP/Basic/");

    WebElement body = driver.findElement(By.tagName("body"));
    assertThat(body.getText()).contains("Your browser made it!");
}
```

- 1. 我们将 driver 对象强制转换为 `HasAuthentication` 并注册凭据（用户名和密码）。
- 2. 我们打开一个受基本认证保护的网站。
- 3. 我们验证页面内容是否可用。

在使用其他浏览器（如 Firefox）时，我们无法将 driver 对象强制转换为 `HasAuthentication`。尽管如此，仍有一种通用方法可通过使用语法在 URL 中发送凭据

`protocol://username:password@domain`。

**示例 5-43** 演示了此用法。

## 示例 5-43. 使用基本认证和 Firefox 测试

```java
@Test
void testGenericAuth() {
    driver.get("https://guest:guest@jigsaw.w3.org/HTTP/Basic/");

    WebElement body = driver.findElement(By.tagName("body"));
    assertThat(body.getText()).contains("Your browser made it!");
}
```

## Web Authentication

Web Authentication API（也称为 *WebAuthn*）是 **W3C 规范**，允许服务器使用公钥密码学而不是密码来注册和验证用户。主要浏览器（Chrome、Firefox、Edge 和 Safari）自 2019 年 1 月起支持 WebAuthn。这些浏览器允许使用 U2F（通用第二因子）令牌进行凭证创建和断言，这些令牌是通用串行总线（USB）或近场通信（NFC）安全设备。

在经典的 Web 认证方法中，用户通过 Web 表单将其用户名和密码发送到服务器。在 WebAuthn 中，Web 服务器使用 Web Authentication API 提示用户创建一对私钥-公钥（称为 *credential*）。私钥安全地存储在用户设备上，公钥发送到服务器。然后，服务器可以使用该公钥验证用户身份。

从版本 4 开始，Selenium WebDriver 原生支持 *WebAuthn*。为此，WebDriver API 提供了接口 `HasVirtualAuthenticator`。而不是使用安全的物理设备，该接口允许我们使用虚拟验证器。虽然 `RemoteWebDriver` 类实现了该接口，但截至本文撰写时，该机制仅在基于 Chromium 的浏览器中受支持，即 Chrome 和 Edge。**示例 5-44** 显示了一个使用 Web Authentication API 的测试。

## 示例 5-44. 使用 WebAuthn 测试

```java
@Test
void testWebAuthn() {
    driver.get("https://webauthn.io/");
    HasVirtualAuthenticator virtualAuthenticator =
        (HasVirtualAuthenticator) driver;
    VirtualAuthenticator authenticator = virtualAuthenticator
        .addVirtualAuthenticator(new
        VirtualAuthenticatorOptions());

    String randomId = UUID.randomUUID().toString();
    driver.findElement(By.id("input-email")).sendKeys(randomId);
    WebDriverWait wait = new WebDriverWait(driver,
        Duration.ofSeconds(20));
    driver.findElement(By.id("register-button")).click();
    wait.until(ExpectedConditions.textToBePresentInElementLocated(
        By.className("popover-body"), "Success! Now try logging in"));

    driver.findElement(By.id("login-button")).click();
    wait.until(ExpectedConditions.textToBePresentInElementLocated(
        By.className("main-content"), "You're logged in!"));

    virtualAuthenticator.removeVirtualAuthenticator(authenticator);
}
```

- 1. 我们打开一个受 Web Authentication API 保护的网站。
- 2. 我们将 driver 对象强制转换为 `HasVirtualAuthenticator`。
- 3. 我们创建并注册一个新的虚拟验证器。
- 4. 我们在 Web 表单中发送一个随机标识符。
- 5. 我们提交该标识符并等待其被接收。
- 6. 我们点击登录按钮。
- 7. 我们验证认证是否已正确执行。
- 8. 我们移除虚拟验证器。





## Shadow DOM

如第 3 章“文档对象模型（DOM）”中介绍的，DOM 是一个编程接口，允许使用树结构表示和操作网页。shadow DOM 是该编程接口的一个特性，能够在常规 DOM 树内部创建作用域子树。shadow DOM 允许封装一组 DOM 子树（称为 shadow tree，如图 5-13 所示），该子树可以指定与原始 DOM 不同的 CSS 样式。shadow tree 所附加的常规 DOM 中的节点称为 shadow host。shadow tree 的根节点称为 shadow root。如图 5-13 所示，shadow tree 被展平到原始 DOM 中，形成一个单一的 composed tree 以在浏览器中渲染。

![](img/b67ec8e49249e340add6ed66ab413d78_313_0.png)

图 5-13. shadow DOM 的示意表示

### NOTE

shadow DOM 是标准套件的一部分（与 HTML 模板或自定义元素一起），允许实现 Web Components（即用于 Web 应用的可重用自定义元素）。

shadow DOM 允许创建自包含的组件。换句话说，shadow tree 与原始 DOM 是隔离的。此特性对 Web 设计和组合很有用，但对使用 Selenium WebDriver 进行自动化测试来说具有挑战性（因为常规定位策略无法找到 shadow tree 内的 Web 元素）。幸运的是，Selenium WebDriver 4 提供了 WebElement 方法，允许访问 shadow DOM。示例 5-45 演示了此用法。

### Example 5-45. Test reading the shadow DOM

```
@Test
void testShadowDom() {
    driver.get(
        "https://bonigarcia.dev/selenium-webdriver-java/shadow-dom.html"); ①

    WebElement content = driver.findElement(By.id("content")); ②
    SearchContext shadowRoot = content.getShadowRoot(); ③
    WebElement textElement =
    shadowRoot.findElement(By.cssSelector("p")); ④
    assertThat(textElement.getText()).contains("Hello Shadow DOM");
    ⑤
}
```

- ① 我们打开包含 shadow tree 的练习网页。你可以检查此页面的源代码以查看用于创建 shadow tree 的 JavaScript 方法。
- ② 我们定位 shadow host 元素。
- ③ 我们从 host 元素获取 shadow root。结果我们得到 SearchContext 的一个实例，该接口由 WebDriver 和 WebElement 实现，允许我们使用 findElement() 和 findElements() 方法查找元素。
- ④ 我们在 shadow tree 中找到第一个段落元素。
- ⑤ 我们验证 shadow 元素的文本内容符合预期。

> **WARNING**
> 
> W3C WebDriver 规范的此特性在本文撰写时是最近的，因此可能并非所有驱动程序都已实现（例如，chromedriver、geckodriver）。例如，它从 msedgedriver 96 版本开始可用。

## Web Proxies

在计算机网络中，*proxy* 是充当客户端与服务器之间中介的服务器。web proxy 是浏览器与 Web 服务器之间的代理，可用于多种目的，例如：

- 访问特定区域的信息。代理通常位于与客户端不同的区域，服务器据此向该区域作出响应。
- 规避限制。代理可以帮助访问被阻止的网站，例如由中间防火墙阻止。
- 捕获网络流量。代理可以收集 HTTP 请求和响应。
- 缓存（以加快网站检索）。

我们也可以在 Selenium WebDriver 测试中使用 web proxy 实现这些目的。为此，WebDriver API 提供了名为 `Proxy` 的类，该类使用 capabilities 配置到 `WebDriver` 对象中。**Example 5-46** 说明了如何操作。

### Example 5-46. Test setup to configure a web proxy

```
@BeforeEach
void setup() {
    Proxy proxy = new Proxy();
    String proxyStr = "proxy:port";
    proxy.setHttpProxy(proxyStr);
    proxy.setSslProxy(proxyStr);

    ChromeOptions options = new ChromeOptions();
    options.setAcceptInsecureCerts(true);
}
```

```
options.setProxy(proxy); // ⑥

driver =
WebDriverManager.chromedriver().capabilities(options).create();
}
```

- ❶ 我们创建类 `Proxy` 的一个实例。
- ❷ 指定代理所需的语法是 `host:port`。
- ❸ 我们指定代理用于 HTTP 连接。
- ❹ 我们还指定代理用于 HTTPS 连接。
- ❺ 虽然不是强制性的，但通常需要接受不安全证书。
- ❻ 我们将代理设置为 capability。此行等价于 `options.setCapability(CapabilityType.PROXY, proxy);`。

> **TIP**
> 
> *Chapter 9* 中“Capture Network Traffic”部分展示了如何使用第三方库通过在 Selenium WebDriver 测试中使用 web proxy 来捕获网络流量。

### Log Gathering

WebDriver API 允许收集不同的日志源。此特性使用 capabilities 启用，尽管在本文撰写时仅在基于 Chromium 的浏览器中受支持。**Example 5-47** 展示了一个启用收集浏览器日志（即控制台消息）的测试设置。此片段还包含测试逻辑，其中我们需要调用 `driver.manage().logs()` 来收集日志列表。

### Example 5-47. Test gathering browser log using Chrome

```
@BeforeEach
void setup() {
    LoggingPreferences logs = new LoggingPreferences();
    logs.enable(LogType.BROWSER, Level.ALL); ①

    ChromeOptions options = new ChromeOptions();
    options.setCapability(CapabilityType.LOGGING_PREFS, logs); ②

    driver =
WebDriverManager.chromedriver().capabilities(options).create();
}

@Test
void testBrowserLogs() {
    driver.get(
        "https://bonigarcia.dev/selenium-webdriver-java/console-logs.html"); ③

    LogEntries browserLogs =
    driver.manage().logs().get(LogType.BROWSER); ④
    Assertions.assertThat(browserLogs.getAll()).isNotEmpty(); ⑤
    browserLogs.forEach(l -> log.debug("{}", l)); ⑥
}
```

① 我们启用收集所有级别的浏览器日志。

② 我们设置 loggingPrefs capability。

③ 我们打开一个在浏览器控制台中记录多个跟踪的练习页面。

④ 我们收集所有日志并按浏览器（控制台跟踪）进行过滤。

⑤ 我们验证跟踪数量不为零。

⑥ 我们在标准输出中显示每个日志。

## Print Page

Selenium WebDriver 允许将网页打印为 PDF 文档。为此，WebDriver API 提供了 PrintsPage 接口。此接口由 RemoteWebDriver 类继承，因此对 Selenium WebDriver 支持的所有浏览器都可用。尽管如此，使用不同浏览器时存在细微差异。例如，仅当浏览器以 headless 模式启动时，使用 Chrome 和 Edge 打印页面才可能。关于 Firefox，此限制不需要，我们可以照常使用 Firefox。示例 5-48 显示了将网页打印为 PDF 的测试逻辑。你可以在 examples 仓库中找到 Firefox 和 headless Chrome/Edge 的完整测试。

### Example 5-48. Test printing a web page to PDF

```
@Test
void testPrint() throws IOException {
    driver.get("https://bonigarcia.dev/selenium-webdriver-java/");
    PrintsPage pg = (PrintsPage) driver; ①
    PrintOptions printOptions = new PrintOptions();
    Pdf pdf = pg.print(printOptions); ②

    String pdfBase64 = pdf.getContent(); ③
    assertThat(pdfBase64).contains("JVBER"); ④

    byte[] decodedImg = Base64.getDecoder()
        .decode(pdfBase64.getBytes(StandardCharsets.UTF_8)); ⑤
    Path destinationFile = Paths.get("my-pdf.pdf");
    Files.write(destinationFile, decodedImg); ⑥
}
```

- ① 我们将 driver 对象强制转换为 PrintsPage。
- ② 我们使用默认配置将当前网页打印为 PDF。
- ③ 我们以 Base64 获取 PDF 的内容。
- ④ 我们验证此内容包含文件签名（“魔术词” JVBER）。
- ⑤ 我们将 Base64 转换为原始字节数组。
- ⑥ 我们将 PDF 内容（字节数组）写入本地文件。

## The Input Method Engine (IME) API





输入法编辑器 API 是一项 W3C 推荐标准，允许将罗马字符转换为东亚语言，例如中文或日文。此 API 可在 Selenium WebDriver 中使用，通过在 WebDriver 对象中调用方法 `driver.manage().ime()` 来调用。尽管如此，该功能仍在开发中，截至本文撰写时，尚未得到任何浏览器的支持。

## 摘要与展望

本章对 WebDriver API 中浏览器之间不互操作的那些功能进行了实践性概述。首先，你了解了如何使用 capabilities 以无头模式运行浏览器、更改页面加载策略、使用网页扩展或管理浏览器弹出窗口（例如地理位置、通知或获取用户媒体），以及其他 capabilities。然后，你了解到 Selenium WebDriver 在 CDP 之上提供了一个用户友好的 API，以便更轻松地与 Chrome DevTools 交互。此 API 允许在我们的 WebDriver 测试中集成大量强大的功能，例如模拟网络条件、网络监控、处理 HTTP 头或阻止 URL，仅举几例。最后，你发现了其他浏览器特定的功能，例如网络拦截、位置上下文、基本和摘要身份验证、网页身份验证（WebAuthn）、Shadow DOM、HTTP 代理、日志收集，以及将网页打印为 PDF 文档。

下一章将结束我们沿着 WebDriver API 的旅程。本章解释了如何使用 WebDriver API 来控制远程浏览器。这些浏览器可以托管在 Selenium Grid、云提供商（例如 Sauce Labs、BrowserStack 或 CrossBrowserTesting）上，或在 Docker 容器中执行。
