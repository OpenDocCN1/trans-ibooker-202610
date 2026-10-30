
# Table 4-2. Output Type parameters

| Parameter | Description | Return | Example |
|---|---|---|---|

| OutputType.FILE | Make screenshot as a PNG file (located in a system temporary directory) | File | File screenshot = ts.getScreenshotAs(OutputType.FILE); |
| OutputType.BASE64 | Make a screenshot in Base64 format (i.e., encoded as a ASCII string) | String | String screenshot = ts.getScreenshotAs(OutputType.BASE64); |
| OutputType.BYTES | Make a screenshot as a raw byte array | byte[] | byte[] screenshot = ts.getScreenshotAs(OutputType.BYTES); |

> TIP
> 
> The method getScreenshotAs() allows making screenshots of the browser viewport. In addition, Selenium WebDriver 4 allows creating full-page screenshots using different mechanisms (see “Full Page Screenshot” in Chapter 5).

Example 4-9 shows a test for taking a browser screenshot in PNG format. Then, Example 4-10 shows another test for creating a screenshot as a Base64 string. In both cases, the resulting screenshot is as shown in Figure 4-3.

## Example 4-9. Test making a screenshot as a PNG file

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

2 We append the prefix data:image/png;base64, to the Base64 string and log it in the standard output. You can copy and paste this resulting string in a browser navigation bar to display the picture.

3 We assert that the screenshot string has content.

> NOTE
> Logging the screenshot in Base64 as presented in the previous example could be very useful for diagnosing failures when running tests in CI servers in which we do not have access to its file system (e.g., GitHub Actions).

## WebElement Screenshots

The WebElement interface extends the TakesScreenshot interface. This way, it is possible to make partial screenshots of the visible content of a given web element. Example 4-11 shows an test about that. Notice that this test is very similar to the previous one using PNG files, but in this case, we invoke the method getScreenshotAs() directly using a web element. Figure 4-4 shows the resulting screenshot.

## Example 4-11. Test making a partial screenshot as a PNG file

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

## Table 4-3. Window methods

| Method | Return | Description |
|---|---|---|
| `getSize()` | `Dimension` | Get the current window size. It returns the outer window dimension, not just the *viewport* (i.e., the visible area of a web page for end-users) |
| `setSize(Dimension targetSize)` | `void` | Change the current window size (again, its outer dimension, and not the viewport) |
| `getPosition()` | `Point` | Get current window position (relative to the upper left) |
|  |  | corner of the screen) |
| setPosition(Point targetPosition) void | Change the current window position (again, relative to the screen upper left corner) |
| maximize() void | Maximize the current window |
| minimize() void | Minimize the current window |
| fullscreen() void | Fullscreen the current window |

## Example 4-12. Test reading and changing the browser size and position

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

+   ① We read the window position.
+   ② We read the window size.
+   ③ We maximize the browser window.

④ We verify that the maximized position (and size, in the following line) is different from the original window.

## Browser History

Selenium WebDriver allows manipulating the browser history through the `Navigation` interface. The following statement illustrates how to access this interface from a WebDriver object. The usage of this interface is quite simple. **Table 4-44** shows its public methods, and **Example 4-13** shows a basic example. Notice that this test navigates into different web pages using these methods, and at the end of the test, it verifies the webpage URL is as expected.

```
```java
Navigation navigation = driver.navigate();
```
```

## Table 4-4. Navigation methods

| Method | Return | Description |
|---|---|---|
| back() | void | Go back in the browser history |
|  |  | Go forward in the browser history |

| Method | Return type | Description |
|---|---|---|
| `forward()` | `void` | |
| `to(String url)` | `void` | Load a new web page in the current window |
| `to(URL url)` |  | |
| `refresh()` | `void` | Refresh the current page |

## Example 4-13. Test using navigation methods

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

## Cookies

HTTP 1.x is a stateless protocol, meaning that the server does not track the user state. In other words, web servers do not remember users across different requests. The cookies mechanism is an extension to HTTP that allows tracking users by sending small pieces of text called *cookies* from server to client. These cookies must be sent back by clients, and this way, servers *remember* their clients. Cookies allow you to maintain web sessions or personalize the user experience on the website, among other functions.

Web browsers allow managing the browser cookies manually. Selenium WebDriver enables an equivalent manipulation, but programmatically. To accomplish this, the WebDriver API provides the methods available on **Table 4-5**. They are accessible through the method `manage()` of a `WebDriver` object.

## Table 4-5. Cookies management method

d
s

| Method | Return | Description |
|---|---|---|
| addCookie(Cookie cookie) | void | Add a new cookie |
| deleteCookieNamed(String name) | void | Delete an existing cookie by name |
| deleteCookie(Cookie cookie) | void | Delete an exisiting cookie by instance |
| deleteAllCookies() | void | Delete all cookies |
| getCookies() | Set<Cookie> | Get all cookies |
| getCookieNamed(String name) | Cookie | Get a cookie by name |

As you can see in the previous table, the Cookie class provides an abstraction to a single cookie in Java. Table 4-6 summarizes the methods available in this class. In addition, this class has several constructors, which positionally accept the following parameters:

+   - String name: Cookie name (mandatory).
+   - String value: Cookie value (mandatory).
+   - String domain: Domain in which the cookie is visible (optional).
+   - String path: Path in which the cookie is visible (optional).
+   - Date expiry: Cookie expiration date (optional).
+   - boolean isSecure: Whether the cookie requires a secure connection (optional).
