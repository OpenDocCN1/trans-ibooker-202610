

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

