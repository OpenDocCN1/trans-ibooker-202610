

1. 如果系统不可用 Safari，则跳过此测试。
2. 如果系统不可用 Docker，则跳过此测试。
3. 如果 Selenium Server URL 不在线，则跳过此测试。如果是这样，则执行测试，并使用之前的 URL 创建 `RemoteWebDriver` 的实例。为了指定所需的功能，我们在此测试中使用注解 `@DriverCapabilities`（如第 6 章所述）。

## Summary and Outlook

本章介绍了本书中使用的测试框架（即 JUnit 4、TestNG、JUnit 5 和 Selenium-Jupiter）用于开发 Selenium WebDriver 测试的一些最相关特定功能。首先，你学习了如何实现参数化测试。此功能可用于跨浏览器测试（即使用不同的浏览器进行 Web 测试）十分方便。然后，你学习了如何对测试进行分类并使用这些类别将其包含或排除在测试执行之外。你继续理解了故障分析机制（例如在测试失败时截取浏览器屏幕截图）、重试测试或并行执行测试。最后，你发现了如何实现测试监听器以及禁用测试的不同机制。

在下一章中，你将学习如何将 Selenium WebDriver 与不同的第三方实用程序集成以实现高级端到端测试。你将发现如何从 Web 应用程序下载文件、在不使用 CDP 的情况下捕获流量（例如在 Firefox 中）、测试非功能性需求（例如性能、安全或可访问性）、处理不同的输入数据、改进测试报告，以及与现有框架（如 Spring 或 Cucumber）集成。

## Chapter 9. Third-Party Integrations

> **A NOTE FOR EARLY RELEASE READERS**
> 
> With Early Release ebooks, you get books in their earliest form—the author’s raw and unedited content as they write—so you can take advantage of these technologies long before the official release of these titles.
> 
> This will be the 9th chapter of the final book. Please note that the GitHub repo will be made active later on.
> 
> If you have comments about how we might improve the content and/or examples in this book, or if you notice missing material within this chapter, please reach out to the editor at rfernando@oreilly.com.

本章介绍可与 Selenium WebDriver 一起使用的不同第三方技术（例如库或框架）。当 Selenium WebDriver API 不足以执行特定任务时，我们需要使用其中一些技术。这就是文件下载的情况，我们需要使用第三方实用程序等待文件正确下载，或者 alternatively 使用 HTTP 客户端来控制下载。我们还使用第三方代理捕获 HTTP 流量。另一个需要使用外部实用程序来 Selenium WebDriver 的场景是在实现非功能性测试时，例如性能、安全、可访问性或 A/B 测试。我们也可以使用第三方库以流畅 API 开发 Selenium WebDriver 测试、生成假测试数据或改进测试报告。最后，我们可以集成相关框架，例如用于行为驱动开发（BDD）的 Cucumber 或 Spring Framework（用于开发 Web 应用程序）。我们将在本章中介绍我提到的所有用途。

> **TIP**
> 
> 要使用本章介绍的第三方实用程序，您必须首先在项目中包含所需的依赖项。您可以在附录 C 中找到使用 Maven 和 Gradle 解析每个依赖项的详细信息。

## File Download

Selenium WebDriver 对文件下载的支持有限，因为其 API 未公开下载进度。换句话说，我们可以使用 Selenium WebDriver 从 Web 应用程序下载文件，但我们无法控制将这些文件复制到本地文件系统的所需时间。因此，我们可以使用第三方库来增强使用 Selenium WebDriver 的 Web 下载体验。有不同的替代方案来实现这一目标。以下小节说明了如何操作。

### Using Browser-Specific Capabilities

我们可以使用浏览器特定功能（正如我们在第 5 章中所做的那样）来配置文件下载的多个参数，例如目标文件夹。由于这些功能在 Selenium WebDriver API 中开箱即用，因此这种方法很方便，但也存在几个缺点。首先，它与不同浏览器类型（Chrome、Firefox 等）不兼容。换句话说，所需的功能因各个浏览器而异。其次，更重要的是，我们无法控制以跟踪下载进度。为了解决这个问题，我们需要使用第三方库。在本书中，我建议使用开源库 Awaitility。

Awaitility 是一个流行的库，提供处理异步操作的功能。这样，它提供了一个流畅的 API 来管理线程、超时和并发问题。在使用 Selenium WebDriver 下载文件的情况下，我们使用 Awaitility API 等待下载的文件存储在文件系统中。示例 9-1 显示了使用 Chrome 和 Awaitility 的示例。然后，示例 9-2 显示了使用 Firefox 时等效的测试设置。

### Example 9-1. Test download files using Chrome and Awaitility

```
class DownloadChromeJupiterTest {

    WebDriver driver;

    File targetFolder;

    @BeforeEach
    void setup() {
        targetFolder = new File(System.getProperty("user.home"), "Downloads"); ①
        Map<String, Object> prefs = new HashMap<>();
        prefs.put("download.default_directory", targetFolder.toString()); ②
        ChromeOptions options = new ChromeOptions();
        options.setExperimentalOption("prefs", prefs);

        driver =
        WebDriverManager.chromedriver().capabilities(options).create();
    }

    @AfterEach
    void teardown() {
        driver.quit();
    }

    @Test
    void testDownloadChrome() {
        driver.get(
            "https://bonigarcia.dev/selenium-webdriver-java/download.html"); ③

        driver.findElement(By.xpath("//a[2]")).click(); ④
        driver.findElement(By.xpath("//a[3]")).click();

        ConditionFactory await = Awaitility.await()
            .atMost(Duration.ofSeconds(5)); ⑤
        File wdmLogo = new File(targetFolder, "webdrivermanager.png");
        await.until(() -> wdmLogo.exists()); ⑥

        File wdmDoc = new File(targetFolder, "webdrivermanager.pdf");
        await.until(() -> wdmDoc.exists()); ⑦
    }
}
```

① 我们指定一个文件夹来保存下载的文件。然而，你应该注意 Chrome 仅允许某些目录用于下载。例如，它允许下载目录（和子文件夹），但禁止使用其他路径，例如桌面文件夹或主目录。

② 我们使用 Chrome 首选项指定目标文件夹。

③ 我们使用练习站点上可用的网页通过单击按钮下载不同的文件（参见图 9-1）。

④ 我们单击页面上可用的两个按钮。结果，浏览器开始下载两个文件：PNG 图片和 PDF 文档。

⑤ 我们使用 Awaitility 配置 5 秒的等待超时。

⑥ 我们等待第一个文件进入文件系统。

⑦ 我们也等待第二个文件下载完成。

### Example 9-2. Test setup for downloading files using Firefox

```
@BeforeEach
void setup() {
    FirefoxOptions options = new FirefoxOptions();
    targetFolder = new File("."); ①
    options.addPreference("browser.download.dir",
        targetFolder.getAbsolutePath()); ②
    options.addPreference("browser.download.folderList", 2); ③
    options.addPreference("browser.helperApps.neverAsk.saveToDisk",
        "image/png, application/pdf"); ④
    options.addPreference("pdfjs.disabled", true); ⑤

    driver = WebDriverManager.firefoxdriver().capabilities(options)
        .create();
}
```

