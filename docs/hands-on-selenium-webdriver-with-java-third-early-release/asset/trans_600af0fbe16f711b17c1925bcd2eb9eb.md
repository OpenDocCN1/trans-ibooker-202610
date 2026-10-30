

![](img/b67ec8e49249e340add6ed66ab413d78_466_0.png)

图 9-5. 执行 Selenium WebDriver 测试后由 ZAP 生成的报告

## 无障碍性

数字无障碍性是指残障用户能够有效使用软件系统（如网站、移动应用等）的能力。该领域的一个重要参考是《网页内容无障碍指南》（WCAG），这是由 W3C 网页无障碍倡议（WAI）制定的一系列标准建议，解释了如何让网页内容对残障人士更加无障碍。

测试 Web 应用无障碍性的方法有很多种。最常见的方法是检查 WCAG 建议。为此，我们可以使用自动化无障碍扫描器，例如 Axe，它是一个开源引擎，用于按照 WCAG 规则对 Web 应用进行自动化无障碍测试。Axe 通过一个辅助库与 Selenium WebDriver 的 Java 绑定无缝集成。示例 9-8 展示了一个使用该库的测试。

## 示例 9-8. 使用 Axe 生成无障碍报告的测试

```java
@Test
void testAccessibility() {
    driver.get("https://bonigarcia.dev/selenium-webdriver-java/");
    assertThat(driver.getTitle()).contains("Selenium WebDriver");

    Results result = new AxeBuilder().analyze(driver);
    List<Rule> violations = result.getViolations();
    violations.forEach(rule -> {
        log.debug("{}", rule.toString());
    });
    AxeReporter.writeResultsToJsonFile("testAccessibility", result);
}
```

1. 我们使用 Axe 分析当前的 WebDriver 会话。
2. 我们获取发现的无障碍违规报告。
3. 我们将每个违规记录到标准输出中。在此示例中，发现的问题如下：
   - *color-contrast*：元素必须具有足够的颜色对比度。
   - *heading-order*：标题级别应每次只增加一级。
   - *image-alt*：图片必须包含替代文本。
   - *link-name*：链接必须具有可识别的文本。
4. 我们将结果写入本地 JSON 文件。

## A/B 测试

A/B 测试是一种可用性评估形式，它比较同一应用的不同变体，以发现哪种变体对最终用户更有效。不同的商业产品为 Selenium WebDriver 测试中的 A/B 测试提供了高级功能。例如，Applitools Eyes 提供了对多个网页变体的自动视觉比较。另一种选择是 Optimizely，这是一家提供 A/B 测试定制和实验工具的公司。

进行 A/B 测试的另一种方式是使用原生的 Selenium WebDriver API，并为网页的不同变体自定义条件。示例 9-9 展示了一个针对多变体网页采用手动方式的基本测试。

## 示例 9-9. 使用 Selenium WebDriver 进行基本 A/B 测试

```java
@Test
void testABTesting() {
    driver.get(
        "https://bonigarcia.dev/selenium-webdriver-java/ab-
        testing.html");
    WebDriverWait wait = new WebDriverWait(driver,
        Duration.ofSeconds(10));
    WebElement header = wait.until(
        ExpectedConditions.presenceOfElementLocated(By.tagName("h6")));

    if (header.getText().contains("variation A")) {
        assertBodyContains(driver, "Lorem ipsum");
    } else if (header.getText().contains("variation B")) {
        assertBodyContains(driver, "Nibh netus");
    } else {
        fail("Unknown variation");
    }
}

void assertBodyContains(WebDriver driver, String text) {
    String bodyText =
        driver.findElement(By.tagName("body")).getText();
    assertThat(bodyText).contains(text);
}
```

1. 我们打开一个多变体的练习网页。该页面的内容有 50% 的时间是随机加载的。
2. 我们检查页面变体是否符合预期。

## 流式 API

如第 1 章所述，Selenium 是其他框架和库的基础技术。例如，我们可以找到几个封装 Selenium WebDriver 的库，以提供用于创建 Web 应用端到端测试的流式 API。这类库的一个例子是 Selenide，它是一个开源（MIT 许可证）库，在 Selenium WebDriver 之上定义了一个简洁的流式 API。Selenide 提供了若干好处，例如对 Web 元素的自动等待或对 AJAX 应用的支持。

与 Selenium WebDriver 相比，Selenide 测试的一个相关区别是，Selenide 在内部处理 WebDriver 对象。为此，它使用 WebDriverManager 来解析所需的驱动程序（例如 chromedriver、geckodriver 等），并将 WebDriver 实例保存在一个单独的线程中，该线程在测试结束时关闭。因此，与创建和终止 WebDriver 对象相关的测试样板代码就不再需要了。示例 9-10 通过展示一个基本的 Selenide 测试来演示这一特性。

## 示例 9-10. 使用 Selenide 的测试

```java
class SelenideJupiterTest {
    @Test
    void testSelenide() {
        open("https://bonigarcia.dev/selenium-webdriver-java/login-form.html"); ①

        $(By.id("username")).val("user"); ②
        $(By.id("password")).val("user");
        $("button").pressEnter(); ③
        $(By.id("success")).shouldBe(visible)
            .shouldHave(text("Login successful")); ④
    }
}
```

① 我们使用 Selenide 提供的静态方法 `open` 来导航到给定的 URL。默认情况下，Selenide 使用本地 Chrome，但也可以通过配置类（例如 `Configuration.browser = "firefox";`）或 Java 系统属性（例如 `-Dselenide.browser=firefox`）来更改浏览器。

② Selenide 的 `$` 方法允许我们通过 CSS 选择器或使用 Selenium WebDriver 的 `By` 定位器来定位 Web 元素。这行代码使用后者向输入字段键入一些文本。

③ 我们定位另一个 Web 元素，这次使用 CSS 选择器，并点击它。

④ 我们验证成功 Web 元素存在于页面上，并包含预期的文本。

## 测试数据

任何测试用例的一个相关部分是测试数据，即用于运行被测系统（SUT）的输入数据。选择合适的测试数据对于实现有效的测试至关重要。经典测试理论中有多种测试数据选择技术，例如：

- *边界测试*，即在输入值的极端端点之间或分区之间进行测试的过程。这种方法的基本思想是选择输入域中的代表性极限值（例如，低于最小值、最小值、略高于最小值、正常值、略低于最大值、最大值和高于最大值）。
- *等价类划分*，即将所有可能的输入测试数据划分为若干值集进行测试的过程，我们假设这些值集会以相同方式被处理。

在端到端测试中，这些方法可能不切实际，因为执行这些策略所需的测试数量（以及由此产生的执行时间）可能非常巨大。相反，我们通常会手动选择一些有代表性的测试数据来验证*快乐路径*（即*正向测试*），并可选择地，为意外情况选取一些测试数据（*负向测试*）。

选择测试数据的另一种替代方案是使用*伪造*数据，即不同领域的随机数据，例如个人姓名、姓氏、地址、国家/地区、电子邮件、电话号码等。实现这一目标的一个简单替代方案是使用 [Java Faker]，它是流行的 [faker] Ruby gem 的移植版本。[示例 9-11] 展示了一个使用该库的测试。该测试使用伪造数据提交练习网站上可用的一个 Web 表单。[图 9-6] 展示了在提交带有该伪造数据的表单后的网页。

## 示例 9-11. 使用 JavaFaker 生成不同类型伪造数据的测试

```java
@Test
void testFakeData() {
    driver.get(
        "https://bonigarcia.dev/selenium-webdriver-java/data-
        types.html");

    Faker faker = new Faker();

    driver.findElement(By.name("first-name"))
          .sendKeys(faker.name().firstName());
    driver.findElement(By.name("last-name"))
          .sendKeys(faker.name().lastName());
    driver.findElement(By.name("address"))
          .sendKeys(faker.address().fullAddress());
    driver.findElement(By.name("zip-code"))
          .sendKeys(faker.address().zipCode());

    driver.findElement(By.name("city")).sendKeys(faker.address().city());
    driver.findElement(By.name("country"))
          .sendKeys(faker.address().country());
    driver.findElement(By.name("e-mail"))
          .sendKeys(faker.internet().emailAddress());
    driver.findElement(By.name("phone"))
          .sendKeys(faker.phoneNumber().phoneNumber());
    driver.findElement(By.name("job-position"))
          .sendKeys(faker.job().position());

    driver.findElement(By.name("company")).sendKeys(faker.company().name());

    driver.findElement(By.tagName("form")).submit();

    List<WebElement> successElement = driver
          .findElements(By.className("alert-success"));
    assertThat(successElement).hasSize(10);

    List<WebElement> errorElement = driver
          .findElements(By.className("alert-danger"));
    assertThat(errorElement).isEmpty();
}
```

1. 我们创建一个 Java Faker 实例。
2. 我们发送不同类型（姓名、地址、国家/地区等）的随机数据。
3. 我们验证数据是否正确提交。
4. 我们检查页面上没有错误。

