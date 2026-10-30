

# 表 4-2. 输出类型参数

| 参数 | 描述 | 返回 | 示例 |
|---|---|---|---|
| OutputType.FILE | 将截图保存为 PNG 文件（位于系统临时目录中） | File | File screenshot = ts.getScreenshotAs(OutputType.FILE); |
| OutputType.BASE64 | 以 Base64 格式（即编码为 ASCII 字符串）截图 | String | String screenshot = ts.getScreenshotAs(OutputType.BASE64); |
| OutputType.BYTES | 将截图保存为原始字节数组 | byte[] | byte[] screenshot = ts.getScreenshotAs(OutputType.BYTES); |

> 提示
> 
> `getScreenshotAs()` 方法允许截取浏览器视口的屏幕截图。此外，Selenium WebDriver 4 允许使用不同机制创建整页截图（参见第 5 章的“整页截图”）。

示例 4-9 展示了一个以 PNG 格式截取浏览器屏幕的测试。接着，示例 4-10 展示了另一个以 Base64 字符串创建截图的测试。两种情况下的截图结果如图 4-3 所示。

## 示例 4-9. 以 PNG 文件形式截图的测试

```
@Test
void testScreenshotPng() throws IOException {
    driver.get("https://bonigarcia.dev/selenium-webdriver-java/");
    TakesScreenshot ts = (TakesScreenshot) driver;

    File screenshot = ts.getScreenshotAs(OutputType.FILE); ①
    log.debug("Screenshot created on {}", screenshot);

    Path destination = Paths.get("screenshot.png"); ②
    Files.move(screenshot.toPath(), destination, REPLACE_EXISTING); ③

    log.debug("Screenshot moved to {}", destination);
}
```

2 我们将前缀 `data:image/png;base64,` 附加到 Base64 字符串，并将其记录到标准输出。你可以复制并将结果字符串粘贴到浏览器导航栏中以显示图片。

3 我们断言截图的字符串包含内容。

> 注意
> 如前面的示例所示，在 CI 服务器（例如 GitHub Actions）上运行测试时，我们无法访问其文件系统，此时以 Base64 记录截图对于诊断故障非常有用。

## WebElement 截图

WebElement 接口扩展了 TakesScreenshot 接口。这样，就可以对给定 Web 元素的可见内容进行部分截图。示例 4-11 展示了相关测试。请注意，该测试与之前使用 PNG 文件的测试非常相似，但在本例中，我们直接使用 Web 元素调用 getScreenshotAs() 方法。图 4-4 显示了截图结果。

## 示例 4-11. 以 PNG 文件形式进行部分截图的测试

```
@Test
void testWebElementScreenshot() throws IOException {
    driver.get(
        "https://bonigarcia.dev/selenium-webdriver-java/web-form.html");

    WebElement form = driver.findElement(By.tagName("form"));
    File screenshot = form.getScreenshotAs(OutputType.FILE);
    Path destination = Paths.get("webelement-screenshot.png");
    Files.move(screenshot.toPath(), destination, REPLACE_EXISTING);

    assertThat(destination).exists();
}
```

## 表 4-3. 窗口方法

| 方法 | 返回 | 描述 |
|---|---|---|
| `getSize()` | `Dimension` | 获取当前窗口大小。它返回外部窗口尺寸，而不仅仅是*视口*（即终端用户看到的网页可见区域） |
| `setSize(Dimension targetSize)` | `void` | 更改当前窗口大小（同样是外部尺寸，而不是视口） |
| `getPosition()` | `Point` | 获取当前窗口位置（相对于左上角） |
|  |  | 屏幕的角落） |
| setPosition(Point targetPosition) void | 更改当前窗口位置（同样是相对于屏幕左上角） |
| maximize() void | 最大化当前窗口 |
| minimize() void | 最小化当前窗口 |
| fullscreen() void | 使当前窗口全屏 |

## 示例 4-12. 读取和更改浏览器大小与位置的测试

```
@Test
void testWindow() {
    driver.get("https://bonigarcia.dev/selenium-webdriver-java/");
    Window window = driver.manage().window();

    Point initialPosition = window.getPosition(); ①
    Dimension initialSize = window.getSize(); ②
    log.debug("Initial window: position {} -- size {}",
    initialPosition,
        initialSize);

    window.maximize(); ③

    Point maximizedPosition = window.getPosition();
    Dimension maximizedSize = window.getSize();
    log.debug("Maximized window: position {} -- size {}",
    maximizedPosition,
        maximizedSize);

    assertThat(initialPosition).isNotEqualTo(maximizedPosition); ④
    assertThat(initialSize).isNotEqualTo(maximizedSize);
}
```

+   ① 我们读取窗口位置。
+   ② 我们读取窗口大小。
+   ③ 我们最大化浏览器窗口。

④ 我们验证最大化后的位置（以及下一行中的大小）与原始窗口不同。

## 浏览器历史记录

Selenium WebDriver 允许通过 `Navigation` 接口操纵浏览器历史记录。下面的语句说明了如何从 WebDriver 对象访问该接口。该接口的使用非常简单。**表 4-44** 显示了其公共方法，**示例 4-13** 展示了一个基本示例。请注意，该测试使用这些方法导航到不同的网页，并在测试结束时验证网页 URL 是否符合预期。

```
```java
Navigation navigation = driver.navigate();
```
```

## 表 4-4. 导航方法

| 方法 | 返回 | 描述 |
|---|---|---|
| back() | void | 在浏览器历史记录中后退 |
|  |  | 在浏览器历史记录中前进 |

| 方法 | 返回类型 | 描述 |
|---|---|---|
| `forward()` | `void` | |
| `to(String url)` | `void` | 在当前窗口中加载新的网页 |
| `to(URL url)` |  | |
| `refresh()` | `void` | 刷新当前页面 |

## 示例 4-13. 使用导航方法的测试

```
```java
@Test
void testHistory() {
    String baseUrl = "https://bonigarcia.dev/selenium-webdriver-java/";
    String firstPage = baseUrl + "navigation1.html";
    String secondPage = baseUrl + "navigation2.html";
    String thirdPage = baseUrl + "navigation3.html";

    driver.get(firstPage);

    driver.navigate().to(secondPage);
    driver.navigate().to(thirdPage);
    driver.navigate().back();
    driver.navigate().forward();
    driver.navigate().refresh();

    assertThat(driver.getCurrentUrl()).isEqualTo(thirdPage);
}
```
```

## Cookie

HTTP 1.x 是一种无状态协议，意味着服务器不会跟踪用户状态。换句话说，Web 服务器不会在多个请求之间记住用户。Cookie 机制是 HTTP 的一种扩展，它通过从服务器向客户端发送称为 *cookie* 的小段文本来跟踪用户。这些 cookie 必须由客户端发送回去，这样服务器才能*记住*其客户端。Cookie 允许您维持 Web 会话或个性化网站上的用户体验，以及实现其他功能。

Web 浏览器允许手动管理浏览器 Cookie。Selenium WebDriver 提供了等效的编程式操作。为此，WebDriver API 提供了**表 4-5** 中的可用方法。它们可以通过 `WebDriver` 对象的 `manage()` 方法访问。

## 表 4-5. Cookie 管理方法

d
s

| 方法 | 返回 | 描述 |
|---|---|---|
| addCookie(Cookie cookie) | void | 添加一个新的 Cookie |
| deleteCookieNamed(String name) | void | 按名称删除现有 Cookie |
| deleteCookie(Cookie cookie) | void | 按实例删除现有 Cookie |
| deleteAllCookies() | void | 删除所有 Cookie |
| getCookies() | Set<Cookie> | 获取所有 Cookie |
| getCookieNamed(String name) | Cookie | 按名称获取 Cookie |

如上表所示，Cookie 类在 Java 中提供了对单个 Cookie 的抽象。表 4-6 总结了该类中可用的方法。此外，该类有多个构造函数，它们按位置接受以下参数：

+   - String name：Cookie 名称（必填）。
+   - String value：Cookie 值（必填）。
+   - String domain：Cookie 可见的域（可选）。
+   - String path：Cookie 可见的路径（可选）。
+   - Date expiry：Cookie 的过期日期（可选）。
+   - boolean isSecure：Cookie 是否需要安全连接（可选）。

