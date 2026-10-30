
- Dynamic grid. To start Docker containers on demand.
- Deploying to Kubernetes. Kubernetes is an open-source container-orchestration system that automates the deployment and management of containerized applications. We can use Kubernetes to deploy the Selenium Docker containers.
- Advanced container configuration. For instance, to specify Selenium or Java custom configuration.
- Access to the remote session using Virtual Network Computing (VNC) (a graphical desktop sharing system) and noVNC (an open-source web-based VNC client).

## Selenoid

Selenoid is an open-source Golang implementation of a Selenium Hub. Selenoid can be seen as a lightweight Selenium Server that provides a browser infrastructure based on Docker. The Selenoid team also maintains the Docker images used by Selenoid. These images include multiple web browsers and Android devices, such as Chrome, Firefox, Edge, Opera, Safari (WebKit engine), or Chrome Mobile.

There are different ways to use Selenoid and its Docker images. A straightforward way is to use the configuration manager (a binary called cm) provided by the project. The following snippet shows how to start Selenoid and its UI (a web-based dashboard to monitor Selenoid):

```
```bash
./cm selenoid start ❶
./cm selenoid-ui start ❷
```
```

❶ We start Selenoid. The configuration manager downloads the Docker image for Selenoid and the two latest versions of several browsers (Chrome, Firefox, and Opera). Once it is started, Selenoid listens to Selenium WebDriver requests in the URL `http://localhost:4444/wd/hub`.

❷

Optionally, we can start the Selenoid UI. This UI is a web application accessible in the URL *http://localhost:8080/*. Figure 6-9 shows a screenshot of this UI during the execution of a Selenium WebDriver test. Example 6-10 shows the setup of a test that uses a Chrome browser served by Selenoid.

![](img/b67ec8e49249e340add6ed66ab413d78_356_0.png)

Figure 6-9. Selenoid UI during a test execution using VNC

### Example 6-10. Instantiation of a RemoteWebDriver object using the constructor

```
@BeforeEach
void setup() throws MalformedURLException {
    URL seleniumServerUrl = new URL("http://localhost:4444/wd/hub");
    assumeThat(isOnline(seleniumServerUrl)).isTrue();

    ChromeOptions options = new ChromeOptions();
    Map<String, Object> selenoidOptions = new HashMap<>();
    selenoidOptions.put("enableVNC", true); ①
    options.setCapability("selenoid:options", selenoidOptions); ②
    driver = new RemoteWebDriver(seleniumServerUrl, options);
}
```

① The capability enableVNC is Selenoid-specific and allows to start the dockerized browser with VNC support (this way, we can visualize the browser session in the Selenoid UI, as illustrated in Figure 6-9).

② Since this capability is vendor-specific, the W3C WebDriver compatible way to set this capability is using a custom namespace

(selenoid:options in this case).

## Further Features

Selenoid provides different features and configuration capabilities. You can check its documentation for more details. These features include video recording, custom configuration, log management, or access to browser developer tools, to name a few.

## WebDriverManager

As of version 5, WebDriverManager allows using web browsers in Docker containers in an effortless manner. To that aim, each manager (e.g., chromedriver(), firefoxdriver(), etc.) provides the method browserInDocker(). WebDriverManager internally pulls the Docker images and runs the container, creating a RemoteWebDriver instance when invoking the method create(). WebDriverManager uses the Docker images maintained by the Selenoid team. This way, you can use Chrome (desktop and mobile), Firefox, Edge, Opera, and Safari as Docker containers, out-of-the-box through WebDriverManager. Example 6-11 illustrates a basic test using this feature.

### Example 6-11. Complete test using WebDriverManager and Chrome in Docker

```
class DockerChromeJupiterTest {

    WebDriver driver;

    WebDriverManager wdm =
    WebDriverManager.chromedriver().browserInDocker(); ①

    @BeforeEach
    void setupTest() {
        assumeThat(isDockerAvailable()).isTrue(); ②
        driver = wdm.create(); ③
    }

    @AfterEach
    void teardown() {
        wdm.quit(); ④
    }
```

```
```java
@Test
void testDockerChrome() {
    driver.get("https://bonigarcia.dev/selenium-webdriver-java/");
    assertThat(driver.getTitle()).contains("Selenium WebDriver");
}
```
```

- 1. We get an instance of the manager for Chrome (`chromedriver()`). Then, using the WebDriverManager fluent API, we specify that the future `WebDriver` objects created with this instance (called `wmd`) will use Docker to execute the corresponding browser (Chrome, in this case).
- 2. We assume a Docker engine is available in the machine running this test. For that, we create an AssertJ assumption by invoking the static method `isDockerAvailable` available on WebDriverManager. This way, when Docker is not available, the test is skipped.
- 3. In the test setup, we create the `WebDriver` instance. Internally, WebDriverManager will connect to Docker Hub to discover the latest version of Chrome available as a Docker image. This image is pulled to the local machine, the Docker container is executed, and the corresponding `RemoteWebDriver` instance is returned to the test logic.
- 4. WebDriverManager allows quitting the previously created `WebDriver` instances through the method `quit()`. This method has the same effect of directly quitting the instance (`driver.quit()` in this case), and also, the used Docker containers are gracefully terminated.

WebDriverManager provides a fluent API to configure different aspects of the dockerized web browsers. The following snippet shows several possibilities. As usual, you can find the complete tests using these features in the examples repository for this book.

```
WebDriverManager wdm =
WebDriverManager.firefoxdriver().browserInDocker(); ①
```

```
WebDriverManager wdm =
WebDriverManager.chromedriver().browserInDocker()
    .browserVersion("beta"); ②
```

```
WebDriverManager wdm =
WebDriverManager.chromedriver().browserInDocker()
    .enableVnc(); ③
```

```
WebDriverManager wdm =
WebDriverManager.chromedriver().browserInDocker()
    .enableRecording(); ④
```

① We select a given manager to use the corresponding dockerized browser (Firefox in this case). In addition to Chrome and Firefox, the other alternatives are Edge, Opera, Safari, and Chrome Mobile.

② By default, WebDriverManager uses the latest version available in Docker Hub for the dockerized browser. Nevertheless, we can force to use a given version (e.g., 94.0). Moreover, different wildcards are valid to specify the following versions, namely:

- latest: To use the latest version (default option).
- latest-N: To use a previous version to the stable. For example, if we specify latest-1 (i.e., latest version minus one), the former version to the stable release is used.
- beta: To use the beta version. This version is only available for Chrome and Firefox, using a fork of the Aerokube Docker images for these browsers maintained by Twilio.
- dev: To use the development version (again, for Chrome and Firefox).

③ To connect to the remote desktop session using VNC or noVNC. By default, WebDriverManager prints the noVNC URL in the log traces. In addition, this URL is accessible invoking the method

As explained in its documentation, you can configure WebDriverManager in multiple ways. For instance, you can specify fine-grained aspects of dockerized browsers, such as the timezone, network, shared memory, volumes, environment variables, screen resolution, or recording output, among others. In addition, WebDriverManager can be used as a Selenium Server. This server uses the container images pulled from Docker Hub to support the browser infrastructure.

## Selenium-Jupiter

Selenium-Jupiter internally uses WebDriverManager to manage and handle web browsers in Docker containers. Regarding dockerized browsers, Selenium-Jupiter provides the annotation @DockerBrowser. You can use this annotation with WebDriver or RemoteWebDriver parameters in test methods. Example 6-12 demonstrate this feature. In this example, we use Chrome in Docker.

### Example 6-12. Complete test using Selenium-Jupiter and Chrome in Docker

```
@EnabledIfDockerAvailable ①
@ExtendWith(SeleniumJupiter.class)
class DockerChromeSelJupTest {

    @Test
    void testDockerChrome(@DockerBrowser(type = CHROME) WebDriver driver) {
        driver.get("https://bonigarcia.dev/selenium-webdriver-java/");
        assertThat(driver.getTitle()).contains("Selenium WebDriver");
    }

}
```

① We decorate the test class with the Selenium-Jupiter annotation @EnabledIfDockerAvailable. This annotation disables the test when Docker is not installed in the machine running the test.
