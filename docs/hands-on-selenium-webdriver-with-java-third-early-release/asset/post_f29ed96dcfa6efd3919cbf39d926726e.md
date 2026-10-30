
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

> ❶ We override the geographical location using the coordinates of the Eiffel Tower (Paris, France).

> ❷ We open a practice web page that accesses the device location and displays the coordinates to the user.

## Manage Cookies

CDP also allows managing web cookies. Example 5-34 shows a test reading the cookies of a practice page that manages some cookies.

### Example 5-34. Test managing cookies

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

- ① We read all the cookies of a web page.
- ② We verify that the cookies read using the CDP command and the cookies read with the WebDriver API (using getCookies();) are the same.
- ③ We remove all cookies.
- ④ We verify there are no cookies at this point.

## Load Insecure Pages

CDP also allows to load insecure web pages (i.e., web pages that use HTTPS, but the certificate is not valid). Example 5-35 illustrates this feature.

### Example 5-35. Test loading an insecure web page

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

- 1. We enable tracking security.
- 2. We ignore certificate errors.
- 3. We verify the page is correctly loaded.

## Network Interceptor

The WebDriver API contains a group of helper classes built on top of CDP. These classes are aimed to provide a friendly API enabling advanced features for WebDriver tests. The drawback of these features is that the used browser should support CDP. Therefore, they are available in Chrome and Edge but unavailable in other browsers, such as Firefox.

The first helper class of this nature is `NetworkInterceptor`. This class allows stubbing out the backend requests, intercepting network traffic, and returning pre-canned responses. This feature might help to simplify complex end-to-end tests by mocking external calls with fast and straightforward responses. To instantiate `NetworkInterceptor`, we need to specify two parameters in its constructor (see Example 5-36):

- A `WebDriver` object that implements the CDP (i.e., `ChromeDriver` or `EdgeDriver`).
- A `Route` object to map the network requests to responses.

### Example 5-36. Test intercepting network traffic using NetworkInterceptor

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

- 1 We load a local image stored as a test resource in the Java project.
- 2 We create a network interceptor instance, creating a route for all the requests ending in .png, and stubbing this request with a new response, in this case sending the content of the previous picture.
- 3 We open the practice site.
- 4 If the interception works as expected, the image on the page should have a width higher than the original logo.

> NOTE
> A DevToolsException will be thrown if a browser different than Chrome or Edge (such as Firefox) is used with previous code.

## Full Page Screenshot

Another possible usage of the CDP is making screenshots of a full page (i.e., capture the content page beyond the viewport). Example 5-37 demonstrate this feature in Chrome.

### Example 5-37. Test making a full page screenshot using CDP in Chrome

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

- 1. We load the practice page containing a long text (and therefore, its content goes beyond the standard viewport).
- 2. We wait until the paragraphs are loaded.
- 3. We get the page layout metrics (to calculate the page dimension).
- 4. We send the CDP command to make a screenshot beyond the page viewport. As a result, we obtain the screenshot as a string in Base64.
- 5. We decode the Base64 content into a PNG file.
- 6. We assert the PNG file exists at the end of the test.

As usual, this feature is available in other browsers implementing CDP, such as Edge. Nevertheless, it is not available in Firefox. Luckily, this browser supports the same characteristic through the method getFullPageScreenshotAs() available in FirefoxDriver objects. Example 5-38 shows a test using this method and Firefox.

### Example 5-38. Test making a full page screenshot using Firefox

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

① We make the whole page screenshot. Like for the regular screenshots (see Table 4-2 in Chapter 4), the output type can be FILE, BASE64, or BYTES. We use the latter type to get the screenshot as a byte array.

## Location Context

The WebDriver API provides the interface LocationContext for mocking the geolocation coordinates of the used device. This interface is implemented by ChromeDriver, EdgeDriver, and OperaDriver. Therefore, these drivers can invoke the method setLocation() to specify custom coordinates (latitude, longitude, and altitude). Example 5-39 shows a basic test using this feature.
