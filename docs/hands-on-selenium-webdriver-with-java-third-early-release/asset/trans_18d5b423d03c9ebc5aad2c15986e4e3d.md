
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

