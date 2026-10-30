```
@Test
void testBasicMethods() {
    String sutUrl = "https://bonigarcia.dev/selenium-webdriver-java/";
    driver.get(sutUrl); ①

    assertThat(driver.getTitle())
        .isEqualTo("Hands-On Selenium WebDriver with Java"); ②
    assertThat(driver.getCurrentUrl()).isEqualTo(sutUrl); ③
    assertThat(driver.getPageSource()).containsIgnoringCase("</html>"); ④
}
```

+   ① We open the practice website.
+   ② We verify the page title is as expected.
+   ③ We confirm the current URL is still the same.
+   ④ We check that the source HTML of the page contains a given tag.

# CONVENTION FOR TESTS NAMES AND CLASSES IN THE EXAMPLES REPOSITORY

To ease the location of tests in the examples repository, I follow a naming convention. The name of each presented test always starts with the word test followed by some descriptive label. Then, I use this label as the prefix of the Java class containing the test. For instance, you can find the previous test (called testBasicMethods) in the classes BasicMethodsJUnit4Test (using JUnit 4), BasicMethodsJupiterTest (using JUnit 5), BasicMethodsSelJupTest (using JUnit 5 plus Selenium-Jupiter), and BasicMethodsNGTest (using TestNG).

# Session Identifier

Each time we instantiate a WebDriver object, the underlying driver (e.g., chromedriver, geckodriver, etc.) creates a unique identifier called sessionId to track the browser session. We can use this value in our test to univocally