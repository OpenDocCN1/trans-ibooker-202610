A convenient extension point for implementing failure analysis is `AfterTestExecutionCallback`, since it allows including custom logic immediately after an individual test has been executed. Example 8-21 provides a Jupiter test using a custom annotation (see Example 8-22) implementing this extension point.

# *Example 8-21. Analysing failed tests using JUnit 5*

```
class FailureJupiterTest {

    static WebDriver driver;

    @RegisterExtension
    FailureWatcher failureWatcher = new FailureWatcher(driver);

    @BeforeAll
    static void setup() {
        driver = WebDriverManager.chromedriver().create();
    }

    @AfterAll
    static void teardown() {
        driver.quit();
    }

    @Test
    void testFailure() {
        driver.get("https://bonigarcia.dev/selenium-webdriver-java/");
        fail("Forced error");
    }
}
```

+   ❶ We use the `FailureWatcher` extension for the tests available in this class. We pass the driver instance as an argument.
+   
+   ❷ We force a failure to make the extension take the browser screenshot.