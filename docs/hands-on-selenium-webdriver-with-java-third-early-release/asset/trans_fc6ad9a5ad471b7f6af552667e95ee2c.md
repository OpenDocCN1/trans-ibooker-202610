

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
