+   - Example 5-1 uses Chrome in headless mode. This example creates a `WebDriver` instance using the required constructor (`ChromeDriver` in this case).
+   - Example 5-2 uses Edge in headless mode. This example creates a `WebDriver` instance using the builder available in the WebDriver API.
+   - Example 5-3 uses Firefox in headless mode. This example creates a `WebDriver` instance using WebDriverManager. Notice that the setup method is not required in this case since WebDriverManager resolves the driver in the same line as the WebDriver instantiation.
+   - Example 5-4 uses Chrome in headless mode through Selenium-Jupiter. This example uses the parameter resolution mechanism provided by Selenium-Jupiter, and so, we simply declare a `ChromeDriver` parameter in the test method. Then, we decorate this parameter using the annotation `@Arguments` to specify the headless mode for this browser.

# Example 5-1. Test using Chrome in headless mode

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