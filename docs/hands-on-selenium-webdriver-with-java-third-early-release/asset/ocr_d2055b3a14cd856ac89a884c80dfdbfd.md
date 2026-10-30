+   1 This browser scenario contains three browsers. The first one is a local Chrome.

+   2 The second browser is a local Edge in headless mode.

+   3 The third browser is Firefox 93, executed in a Docker container.

# Example 8-9. Cross-browser testing using test templates in JUnit 5 with Selenium-Jupiter

```
@EnabledIfDockerAvailable ①
@ExtendWith(SeleniumJupiter.class)
class CrossBrowserJsonSelJupTest {
    @TestTemplate ②
    void testCrossBrowser(WebDriver driver) { ③
        driver.get("https://bonigarcia.dev/selenium-webdriver-java/");
        assertThat(driver.getTitle()).contains("Selenium WebDriver");
    }
}
```

+   1 We use this Selenium-Jupiter annotation to skip the test when Docker is unavailable (since one of the browsers defined in the scenario uses Docker).

+   2 We need to decorate the test method using @TestTemplate instead of the usual @Test annotation.

+   3 We use the generic WebDriver to inject the driver instances. Alternatively, RemoteWebDriver is also valid for test templates.

# Categorizing and Filtering Tests

A common need when having a test suite based on Selenium WebDriver (especially when the number of tests is high) is to execute only a group of