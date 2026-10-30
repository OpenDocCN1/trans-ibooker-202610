There are different ways to instantiate a RemoteWebDriver object. The most common way is to invoke its constructor by passing two arguments: the URL of the Selenium Server and the required capabilities. As shown in Figure 5-1 in Chapter 5, these capabilities are objects that inherit from the Capabilities interface (e.g., ChromeOptions, FirefoxOptions, DesiredCapabilities, etc.). Example 6-1 shows a test setup. As usual, you can find the complete test in the code repository accompanying this book.

NOTE
There is a second RemoteWebDriver constructor, which only accepts one parameter for the desired capabilities. In this case, the Selenium Server URL is read from the Java system property webdriver.remote.server. You can find an example of this feature in the examples repository.

# Example 6-1. Instantiation of a RemoteWebDriver object using the constructor

```
@BeforeEach
void setup() throws MalformedURLException {
    URL seleniumServerUrl = new URL("http://localhost:4444/"); ①
    assumeThat(isOnline(seleniumServerUrl)).isTrue(); ②

    ChromeOptions options = new ChromeOptions(); ③
    driver = new RemoteWebDriver(seleniumServerUrl, options); ④
}
```

+   ① We create a Java URL object with the Selenium Server address.
+   
+   ② We assume this URL is online. For that, we create an AssertJ assumption by invoking the static method isOnline available on WebDriverManager. As a result, the test is skipped when the Selenium Server is offline.
+   
+   ③ We instantiate a ChromeOptions object to specify the required capabilities.