## 第 8 章 测试框架详解

## 给早期版本读者的说明

早期版本电子书让你在书籍最初阶段就能获取——即作者写作时的原始未编辑内容——这样你就可以在正式发布之前很久就利用这些技术。

这将是最终书的第 8 章。请注意，GitHub 仓库稍后将激活。

如果你对如何改进本书内容和/或示例有任何意见，或者发现本章缺少内容，请联系编辑 rfernando@oreilly.com。

在本书的所有示例中，我建议使用不同的单元测试框架：JUnit 4、JUnit 5（单独使用或与 Selenium-Jupiter 扩展结合）或 TestNG，将 Selenium WebDriver API 调用嵌入到带有 `@Test` 注解的 Java 方法中。执行常规测试时，使用不同测试框架之间的差异很小。然而，每个测试框架都有针对不同用例的特定功能。本章总结了实现 Selenium WebDriver 测试的部分功能。和往常一样，你可以在本书的示例仓库中找到本章的源代码。你可以使用这些示例来比较并选择最符合你特定需求的单元测试框架。

## 参数化测试

创建*参数化测试*是单元测试框架普遍支持的一项广泛功能。此功能可以使用不同参数多次执行测试。虽然我们可以使用 JUnit（4 和 5）和 TestNG 实现参数化测试，但每种实现之间差异显著。

### JUnit 4

我们需要使用名为 `Parameterized` 的*测试运行器*在 JUnit 4 中实现参数化测试。JUnit 4 中的测试运行器是负责运行测试的 Java 类。我们使用 JUnit 4 注解 `@RunWith` 装饰 Java 类来指定测试运行器。然后，我们需要使用 JUnit 4 注解 `@Parameters` 装饰提供测试参数的方法。有两种方法可以将这些参数注入测试类：在测试类构造函数中，或者作为使用注解 `@Parameter` 装饰的类属性。示例 8-1 展示了一个使用第二种技术注入测试参数的测试用例。此示例使用不同的凭据（用户名和密码）为登录练习站点执行相同的测试。结果，网页提供的消息不同（登录成功或凭据无效）。

示例 8-1. 使用 JUnit 4 的参数化测试

```
@RunWith(Parameterized.class) ①
public class ParameterizedJUnit4Test {

    WebDriver driver;

    @Parameter(0) ②
    public String username;

    @Parameter(1)
    public String password;

    @Parameter(2)
    public String expectedText;

    @Before
    public void setup() {
        driver = WebDriverManager.chromedriver().create();
    }

    @After
public void teardown() {
    driver.quit();
}

@Parameters(name = "{index}: username={0} password={1} expectedText={2}")
public static Collection<Object[]> data() {
    return Arrays
        .asList(new Object[][] { { "user", "user", "Login successful" },
            { "bad-user", "bad-passwd", "Invalid credentials" } });
}

@Test
public void testParameterized() {
    driver.get(
        "https://bonigarcia.dev/selenium-webdriver-java/login-form.html");

    driver.findElement(By.id("username")).sendKeys(username);
    driver.findElement(By.id("password")).sendKeys(password);
    driver.findElement(By.cssSelector("button")).click();

    String bodyText =
    driver.findElement(By.tagName("body")).getText();
    assertThat(bodyText).contains(expectedText);
}
```

① 我们为这个 Java 类指定 Parameterized 测试运行器。

② 我们将三个测试参数作为类属性注入：username（索引 0）、password（索引 1）和 expected text（索引 2）。

③ 我们在一个返回泛型参数集合（Collection<Object[]>）的方法中指定测试参数。

④ 我们返回一个包含三组 String 值的集合作为测试参数。每个条目的值将被注入到先前声明的三个参数（username、password 和 expected text）中。

+   ⑤ 在测试逻辑中（该逻辑将执行两次，每个数据条目一次），我们尝试使用作为参数提供的用户名和密码登录练习站点。

+   ⑥ 我们断言预期数据（根据作为参数提供的凭据而不同）在页面正文中可用。

> **警告**
>
> JUnit 4 最显著的局限之一是我们每个 Java 类只能使用一个测试运行器。换句话说，测试运行器在 JUnit 4 中不可组合。为了克服这一限制（以及其他限制），JUnit 团队于 2017 年发布了 JUnit 5。

### TestNG

在参数化 TestNG 测试中，我们可以使用注解 `@DataProvider` 装饰提供测试参数的方法。如示例 8-2 所示，该方法返回一个普通 Java 对象的二维数组。注解 `@DataProvider` 应提供一个名称作为属性。该名称稍后在 `@Test` 方法中用于指定数据提供者。最后，参数被注入测试方法。

示例 8-2. 使用 TestNG 的参数化测试

```
public class ParameterizedNGTest {

    WebDriver driver;

    @BeforeMethod
    public void setup() {
        driver = WebDriverManager.chromedriver().create();
    }

    @AfterMethod
    public void teardown() {
        driver.quit();
    }

    @DataProvider(name = "loginData")
    public static Object[][] data() {
return new Object[][] { { "user", "user", "Login\nsuccessful" },
                        { "bad-user", "bad-passwd", "Invalid credentials" } };

@Test(dataProvider = "loginData")
public void testParameterized(String username, String password,
                              String expectedText) {
    // Same test logic than the example before
}
```

+   1. 我们创建一个用作数据提供者的方法。
+   2. 我们指定此测试将使用之前称为 `loginData` 的数据提供者。
+   3. 关于参数化测试，JUnit 4 和 TestNG 的一个显著区别是参数（本例中的 username、password 和 expected text）在 TestNG 中作为测试方法参数注入。

### JUnit 5

Jupiter（JUnit 5 的编程和扩展模型）提供了创建参数化测试的强大机制。简而言之，在 JUnit 5 中实现这些测试需要两个元素：

- 一个参数提供者，即参数化测试的数据源。表 8-1 提供了这些参数提供者的全面总结。
- 注解 `@ParameterizedTest`（代替通常的 `@Test` 注解），用于装饰注入参数的测试方法。

表 8-1. `JUnit` 中提供给 `ArgumentProvider` 的参数

| 注解 | 描述 | 示例 | 示例输出 |
| --- | --- | --- | --- |
| `@ValueSource` | 字面值数组 | `<pre>@ParameterizedTest<br>@ValueSource(string s = { "Hi", "Bye" })<br>void test(String argument) {<br>    log.debug("arg: {}", argument);<br>}</pre>` | `<pre>arg: Hi<br>arg: Bye</pre>` |
| `@EnumSource` | Java 枚举的常量 | `<pre>@ParameterizedTest<br>@EnumSource(TimeUnit.class)<br>void test(TimeUnit argument) {<br>    log.debug("{}", argument);<br>}</pre>` | `<pre>NANOSECONDS<br>MICROSECONDS<br>MILLISECONDS<br>SECONDS<br>MINUTES<br>HOURS<br>DAYS</pre>` |
| `@MethodSource` | 该类中提供值流的静态方法 | `<pre>static IntStream intProvider() {<br>    return IntStream.of(0, 1);<br>}<br><br>@ParameterizedTest<br>@MethodSource("intProvider")<br>void test(int argument) {<br>    log.debug("arg: {}", argument);<br>    assertNotNull(argument);<br>}</pre>` | `<pre>arg: 0<br>arg: 1</pre>` |
| `@CsvSource` | 注解内的逗号分隔值（CSV） | `<pre>@ParameterizedTest<br>@CsvSource({ "hello, 1", "world, 2"})<br>void test(String first, int second)<br>{<br>    log.debug("{} and {}", first, second);<br>}</pre>` | `<pre>hello and 1<br>world and 2</pre>` |
| `@CsvFileSource` | 位于类路径的文件中的 CSV 格式值 | `<pre>@ParameterizedTest<br>@CsvFileSource(resources = "/input.csv")<br>void test(String first, int second)<br>{<br>    log.debug("{} and {}", first, second);<br>}</pre>` | `<pre>hi and 3<br>there and 4</pre>` |
| `@ArgumentsSource` | 实现 ArgumentsProvider 接口的类 | `<pre>@ParameterizedTest<br>@ArgumentsSource(MyArgs.class)<br>void test(String first, int second)</pre>` | `<pre>hi and 5<br>there and 6</pre>` |



```java
@EmptySource
@ParameterizedTest
@ValueSource(strings = { "three", "four" })
void test(String argument) {
    log.debug("arg: {}", argument);
}
```

一个 null 加一个空参数

```java
@NullAndEmptySource
@ParameterizedTest
@ValueSource(strings = { "five", "six" })
void test(String arg) {
    log.debug("arg: {}", arg);
}
```

示例 8-3 展示了前面示例中相同参数化测试的 Jupiter 版本。我们可以使用不同的参数提供者来实现这个参数化测试。在这种情况下，我们使用 @MethodSource 返回参数流。另一个可能很适合此测试的替代方案是使用 @CsvSource 将输入数据和预期结果以 CSV 形式嵌入。

## 示例 8-3. 使用 JUnit 5 的参数化测试

```java
class ParameterizedJupiterTest {

    WebDriver driver;

    @BeforeEach
    void setup() {
```

```java
driver = WebDriverManager.chromedriver().create();
}

@AfterEach
void teardown() {
    driver.quit();
}

static Stream<Arguments> loginData() {
    return Stream.of(Arguments.of("user", "user", "Login successful"),
                     Arguments.of("bad-user", "bad-passwd", "Invalid credentials"));
}

@ParameterizedTest
@MethodSource("loginData")
void testParameterized(String username, String password,
                       String expectedText) {
    // Same test logic than the examples before
}

}
```

- 我们定义一个静态方法，用作 `@MethodSource` 中的参数提供者。
- 我们实现的是参数化测试，而不是常规的 `@Test`。
- 参数提供者与 `loginData` 方法提供的数据相关联。
- 参数被注入到测试方法中。

## Selenium-Jupiter

使用 Selenium-Jupiter 时，你可以采用相同的方法来实现 JUnit 5 参数化测试。唯一的区别在于，你将 `WebDriver` 对象的创建和销毁委托给 Selenium-Jupiter。**示例 8-4** 演示了如何使用 Selenium-Jupiter 实现前面章节中讲解的相同测试（即参数化登录）。

## 示例 8-4. 结合 Selenium-Jupiter 使用 JUnit 5 的参数化测试

```
@ExtendWith(SeleniumJupiter.class)
class ParameterizedSelJupTest {

    static Stream<Arguments> loginData() {
        return Stream.of(Arguments.of("user", "user", "Login successful"),
            Arguments.of("bad-user", "bad-passwd", "Invalid credentials"));
    }

    @ParameterizedTest
    @MethodSource("loginData")
    void testParameterized(String username, String password, String expectedText, ChromeDriver driver) { ①
        // Same test logic than the examples before
    }
}
```

① 在 Jupiter 测试中使用不同的参数解析器时，按照惯例，我们必须首先声明由 @ParameterizedTest 注入的参数，然后再声明由扩展（此处为 Selenium-Jupiter，用于 WebDriver 对象）注入的参数。

## 跨浏览器测试

跨浏览器测试是一种功能测试，我们通过不同类型的 Web 浏览器来验证 Web 应用是否按预期工作。实现跨浏览器测试的一种可行方式是通过参数化测试，将浏览器类型（即 Chrome、Firefox、Edge 等）作为测试参数。以下章节介绍如何将单元测试框架的参数化测试能力应用于跨浏览器测试。在这些示例中，我们将使用本地浏览器（Chrome、Firefox 和 Edge）。另一种执行跨浏览器测试的方式是使用远程浏览器（来自 Selenium Server、云提供商或 Docker），如第 6 章所述。

## JUnit 4

示例 8-5 展示了一个使用 JUnit 4 实现的跨浏览器测试。我们使用 WebDriverManager 来简化参数化。如附录 B 所述，WebDriverManager 可以根据参数的值使用不同的管理器。该参数可以是 WebDriver 类、枚举或浏览器名称。在以下示例中，我们使用后者（尽管你可以在示例仓库中找到替代方法）。

## 示例 8-5. 使用 JUnit 4 的跨浏览器测试

```java
@RunWith(Parameterized.class)
public class CrossBrowserJUnit4Test {

    WebDriver driver;

    @Parameter(0)
    public String browserName;

    @Parameters(name = "{index}: browser-{0}")
    public static Collection<Object[]> data() {
        return Arrays.asList(
                new Object[][] { { "chrome" }, { "edge" }, { "firefox" } });
    }

    @Before
    public void setup() {
        driver = WebDriverManager.getInstance(browserName).create();
    }

    @After
    public void teardown() {
        driver.quit();
    }

    @Test
    public void testCrossBrowser() {
        driver.get("https://bonigarcia.dev/selenium-webdriver-java/");
        assertThat(driver.getTitle()).contains("Selenium WebDriver");
    }
}
```

- 我们使用名称指定了三个浏览器。
- 我们使用这些浏览器名称作为参数，使用 WebDriverManager 的通用管理器。选择不同浏览器的另一种方式是使用无参数的通用管理器（即使用 `.getInstance()` 方法，如附录 B 所述），然后通过 Java 系统属性 `wdm.defaultBrowser` 对测试（或测试套件）进行参数化（例如，在使用 Maven 或 Gradle 运行时）。
- 该测试会执行三次，每次使用不同的浏览器（Chrome、Edge 和 Firefox）。

## TestNG

示例 8-6 展示了相同的跨浏览器测试，这次使用 TestNG。在这种情况下，测试参数（浏览器名称）会被注入到测试方法中。

## 示例 8-6. 使用 TestNG 的跨浏览器测试

```
public class CrossBrowserNGTest {

    WebDriver driver;

    @DataProvider(name = "browsers")
    public static Object[][] data() {
        return new Object[][] { { "chrome" }, { "edge" }, { "firefox" } };
    }

    @AfterMethod
    public void teardown() {
        driver.quit();
    }

    @Test(dataProvider = "browsers")
    public void testCrossBrowser(String browserName) {
        driver = WebDriverManager.getInstance(browserName).create(); ①

        driver.get("https://bonigarcia.dev/selenium-webdriver-java/");
        assertThat(driver.getTitle()).contains("Selenium WebDriver");
    }
}
```

① 由于使用 TestNG 时测试参数被注入到测试方法中，我们需要在测试逻辑中创建 WebDriver 实例。

## JUnit 5

示例 8-7 展示了遵循 Jupiter 模型的相同跨浏览器测试。我们再次使用 WebDriverManager，以浏览器名称作为参数来创建 WebDriver 实例。由于这些参数是字符串，我们使用 @ValueSource 作为参数提供者。

## 示例 8-7. 使用 JUnit 5 的跨浏览器测试

```
class CrossBrowserJupiterTest {

    WebDriver driver;

    @AfterEach
    void teardown() {
        driver.quit();
    }

    @ParameterizedTest
    @ValueSource(strings = { "chrome", "edge", "firefox" })
    void testCrossBrowser(String browserName) {
        driver = WebDriverManager.getInstance(browserName).create();  ①

        driver.get("https://bonigarcia.dev/selenium-webdriver-java/");
        assertThat(driver.getTitle()).contains("Selenium WebDriver");
    }
}
```

① 在 Jupiter 中，参数化测试的参数被注入到测试方法中。因此，我们需要在测试逻辑中创建 driver 实例。

## Selenium-Jupiter

Selenium-Jupiter 提供了一项用于创建跨浏览器测试的补充特性，称为*测试模板*。测试模板是 Jupiter 支持的一种特殊参数化测试，由某个扩展收集参数。Selenium-Jupiter 利用此特性提供了一种全面的方式，可以使用自定义 JSON 记法（在 Selenium-Jupiter 术语中称为*浏览器场景*）来指定浏览器的不同方面（如类型、版本、启动参数和能力）。你可以从 Selenium-Jupiter 文档中找到有关此特性的更多细节。

示例 8-8 展示了一个示例浏览器场景。这个 JSON 存储在一个名为 `browsers.json` 的文件中，这是模板测试使用的默认名称。示例 8-9 展示了使用此浏览器场景的模板测试。

## 示例 8-8. Selenium-Jupiter 中测试模板的浏览器场景

```json
{
  "browsers": [
    [
      {
        "type": "chrome"
      }
    ],
    [
      {
        "type": "edge",
        "arguments": [
          "--headless"
        ]
      }
    ],
    [
      {
        "type": "firefox-in-docker",
        "version": "93"
      }
    ]
  ]
}
```



1. 此浏览器场景包含三个浏览器。第一个是本地 Chrome。
2. 第二个是处于无头模式的本地 Edge。
3. 第三个是 Firefox 93，在 Docker 容器中执行。

## 示例 8-9：使用 Selenium-Jupiter 在 JUnit 5 中通过测试模板进行跨浏览器测试

```java
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

1. 我们使用这个 Selenium-Jupiter 注解，在 Docker 不可用时跳过测试（因为场景中定义的一个浏览器使用了 Docker）。
2. 我们需要使用 `@TestTemplate` 而不是通常的 `@Test` 注解来修饰测试方法。
3. 我们使用通用的 `WebDriver` 来注入驱动实例。另外，`RemoteWebDriver` 对测试模板也有效。

## 分类与筛选测试

在使用基于 Selenium WebDriver 的测试套件时（尤其是在测试数量较多的情况下），一个常见需求是只执行一组测试。实现单个或分组测试执行有多种方式。使用 IDE 运行测试时，我们可以选择要执行的特定测试。使用命令行时，还有其他机制可以用来选择这些测试。

乍一看，我们可以使用构建工具提供的筛选机制。例如，Maven 和 Gradle 允许根据测试类和方法名包含或排除测试。这些命令的基本语法在附录 C 中介绍。**表 8-2** 展示了使用这些命令的几个常见示例。请注意，这些示例中使用通配符 `*` 来匹配测试类名中的任意字符。

### 表 8-2：Maven 和 Gradle 命令示例

| 描述 | Maven | Gradle |
|---|---|---|
| 运行以单词 *Hello* 开头的测试 | `mvn -B test -Dtest=Hello*` | `gradle test --tests Hello*` |
| 运行包含 *Basic* 或 *Timeout* 的测试 | `mvn test -Dtest=*Basic*,*Timeout*` | `gradle test --tests *Basic* --tests *Timeout*` |
| 运行除以 *Firefox* 开头的测试以外的测试 | `mvn test -Dtest=!Firefox*` | `gradle test -PexcludeTests=**/Firefox*` |
| 运行除以 *Docker* 开头或包含 *Remote* 的测试以外的测试 | `mvn test -Dtest=!Docker*,!*Remote*` | `gradle test -PexcludeTests=**/Docker*,**/*Remote*` |

除了构建工具，我们还可以使用单元测试框架提供的内置功能进行分类（也称为分组或标记），并根据这些类别筛选测试。以下小节将解释具体方法。

## JUnit 4

JUnit 4 提供了 `@Category` 注解来对测试进行分组。我们需要在此注解中指定一个或多个 Java 类作为属性。然后，我们可以使用这些类来选择并执行属于一个或多个类别的测试。示例 8-10 展示了一个使用该特性的基本类。

## 示例 8-10：使用类别和 JUnit 4 的测试

```java
public class CategoriesJUnit4Test {

    WebDriver driver;

    @Before
    public void setup() {
        driver = WebDriverManager.chromedriver().create();
    }

    @After
    public void teardown() {
        driver.quit();
    }

    @Test
    @Category(WebForm.class) ①
    public void testCategoriesWebForm() {
        driver.get(
            "https://bonigarcia.dev/selenium-webdriver-java/web-form.html");
        assertThat(driver.getCurrentUrl()).contains("web-form");
    }

    @Test
    @Category(HomePage.class) ②
    public void tesCategoriesHomePage() {
        driver.get("https://bonigarcia.dev/selenium-webdriver-java/");
        assertThat(driver.getCurrentUrl()).doesNotContain("web-form");
    }
}
```

① WebForm 是示例仓库中提供的一个空接口。

② HomePage 是示例仓库中提供的另一个空接口。

然后，我们可以使用命令行根据分组执行测试。例如，以下命令展示了用于运行属于 HomePage 类别的测试的 Maven 和 Gradle 命令。

```bash
mvn test -Dgroups=io.github.bonigarcia.webdriver.junit4.ch08.categories.HomePage
gradle test -Pgroups=io.github.bonigarcia.webdriver.junit4.ch08.categories.HomePage
```

我们可以将此筛选与 Maven 和 Gradle 根据类名选择测试的支持结合起来。例如，以下命令将执行属于 HomePage 类别的测试，但仅限于测试类 `CategoriesJUnit4Test` 中。

```bash
mvn test -Dtest=CategoriesJUnit4Test -DexcludedGroups=io.github.bonigarcia.webdriver.junit4.ch08.categories.HomePage
gradle test --tests CategoriesJUnit4Test -PexcludedGroups=io.github.bonigarcia.webdriver.junit4.ch08.categories.HomePage
```

## TestNG

TestNG 也允许对测试进行分组。示例 8-11 演示了该特性的基本用法。总之，`@Test` 注解允许为这些分组指定字符串标签。

### 示例 8-11：使用分组和 TestNG 的测试

```java
public class CategoriesNGTest {

    WebDriver driver;

    @BeforeMethod(alwaysRun = true) ①
    public void setup() {
        driver = WebDriverManager.chromedriver().create();
    }

    @AfterMethod(alwaysRun = true)
    public void teardown() {
        driver.quit();
    }

    @Test(groups = { "WebForm" }) ②
    public void testCategoriesWebForm() {
        driver.get(
            "https://bonigarcia.dev/selenium-webdriver-java/web-form.html");
        assertThat(driver.getCurrentUrl()).contains("web-form");
    }

    @Test(groups = { "HomePage" }) ③
    public void tesCategoriesHomePage() {
        driver.get("https://bonigarcia.dev/selenium-webdriver-java/");
        assertThat(driver.getCurrentUrl()).doesNotContain("web-form");
    }
}
```

① 我们将 `alwaysRun` 设置为 true，以表示 setup 和 teardown 方法在测试执行期间不会被筛选。

② 我们将分组名称 WebForm 分配给此类的第一个测试。

③ 我们将分组名称 HomePage 设置为第二个测试。

然后，我们可以使用命令行根据这些类别筛选测试执行。以下代码片段首先展示了如何执行属于 HomePage 组的测试。第二部分说明了如何将此分组与 Maven 和 Gradle 基于类名的筛选机制结合使用。

```bash
mvn test -Dgroups=HomePage
gradle test -Pgroups=HomePage

mvn test -Dtest=CategoriesNGTest -DexcludedGroups=HomePage
gradle test --tests CategoriesNGTest -PexcludedGroups=HomePage
```

## JUnit 5

Jupiter 编程模型提供了一种基于称为 *tags* 的自定义标签对测试进行分组的方法。我们使用 `@Tag` 注解来实现这一目的。示例 8-12 演示了该特性。

## 示例 8-12：使用标签和 JUnit 5 的测试

```java
class CategoriesJupiterTest {

    WebDriver driver;

    @BeforeEach
    void setup() {
        driver = WebDriverManager.chromedriver().create();
    }

    @AfterEach
    void teardown() {
        driver.quit();
    }

    @Test
    @Tag("WebForm") ①
    void testCategoriesWebForm() {
        driver.get(
            "https://bonigarcia.dev/selenium-webdriver-java/web-form.html");
        assertThat(driver.getCurrentUrl()).contains("web-form");
    }

    @Test
    @Tag("HomePage") ②
    void testCategoriesHomePage() {
        driver.get("https://bonigarcia.dev/selenium-webdriver-java/");
        assertThat(driver.getCurrentUrl()).doesNotContain("web-form");
    }
}
```

① 我们使用标签 WebForm 标记第一个测试。

② 我们使用 HomePage 标签对第二个测试进行分类。

使用命令行执行测试时，我们可以使用这些标签来包含或排除测试。以下命令展示了 Maven 和 Gradle 的几个示例。

```bash
mvn test -Dgroups=HomePage
gradle test -Pgroups=HomePage
```

```bash
mvn test -Dtest=CategoriesNGTest -DexcludedGroups=HomePage
gradle test --tests CategoriesNGTest -PexcludedGroups=HomePage
```

## 测试排序

在本书使用的单元测试框架中，测试执行顺序事先是未知的。不过，有一些机制可以选择特定的执行顺序。在 Selenium WebDriver 领域，该特性的一个可能用途是让不同测试复用同一个浏览器会话（即使用同一个 WebDriver 实例），以给定的顺序与被测系统（SUT）交互。以下示例演示了 JUnit 4、TestNG、JUnit 5 以及 JUnit 5 加 Selenium-Jupiter 的用例。

## JUnit 4

JUnit 4 提供了 `@FixMethodOrder` 注解来设定测试执行顺序。该注解接受一个名为 `MethodSorters` 的枚举，由以下值组成：

- `NAME_ASCENDING`：按方法名称的字典序对测试方法排序。
- `JVM`：保持由 JVM 返回的测试方法顺序。
- `DEFAULT`：以确定性但不可预测的顺序对测试方法排序。

示例 8-13 展示了一个完整的测试用例，其中测试按方法名称执行。

### 示例 8-13：使用 JUnit 4 对测试排序





```java
@FixMethodOrder(MethodSorters.NAME_ASCENDING) ①
public class OrderJUnit4Test {

    static WebDriver driver;

    @BeforeClass ②
    public static void setup() {
        driver = WebDriverManager.chromedriver().create();
    }

    @AfterClass ③
    public static void teardown() {
        driver.quit();
    }

    @Test ④
    public void testA() {
        driver.get(
            "https://bonigarcia.dev/selenium-webdriver-java/navigation1.html");
        assertBodyContains("Lorem ipsum");
    }

    @Test
    public void testB() {
        driver.findElement(By.linkText("2")).click();
        assertBodyContains("Ut enim");
    }

    @Test
    public void testC() {
        driver.findElement(By.linkText("3")).click();
        assertBodyContains("Excepteur sint");
    }

    void assertBodyContains(String text) {
        String bodyText =
        driver.findElement(By.tagName("body")).getText();
        assertThat(bodyText).contains(text);
    }

}
```

① 我们在类级别使用 `@FixMethodOrder` 注解来固定该类中可用测试的执行顺序。

- ② 我们在所有测试之前创建驱动实例（因为我们希望在所有测试中使用 `WebDriver` 会话）。

- ③ 我们在所有测试之后退出驱动实例。因此，我们在该类的最后一个测试之后结束会话。

- ④ 由于测试名称按字典序排列（`testA`、`testB` 和 `testC`），测试将按此顺序执行。

## TestNG

在 TestNG 中，对测试排序的一种简单方法是为每个测试使用递增优先级。*示例 8-14* 通过 `@Test` 注解中的 `priority` 属性演示了这一特性。

```java
public class OrderNGTest {

    static WebDriver driver;

    @BeforeClass
    public static void setup() {
        driver = WebDriverManager.chromedriver().create();
    }

    @AfterClass
    public static void teardown() {
        driver.quit();
    }

    @Test(priority = 1)
    public void testA() {
        // Test logic
    }

    @Test(priority = 2)
    public void testB() {
        // Test logic
    }

    @Test(priority = 3)
    public void testC() {
        // Test logic
    }
}
```

## JUnit 5

Jupiter 提供了用于测试排序的 `@TestMethodOrder` 注解。该注解可以通过以下排序实现进行配置：

- `DisplayName`：根据显示名称按字母数字顺序对测试方法进行排序。
- `MethodName`：根据方法名称按字母数字顺序对测试方法进行排序。
- `OrderAnnotation`：根据使用 `@Order` 注解指定的数值对测试方法进行排序。示例 8-15 展示了使用此方法的测试。
- `Random`：以伪随机方式对测试方法进行排序。

## 示例 8-15. 使用 JUnit 5 对测试排序

```java
@TestMethodOrder(OrderAnnotation.class)
class OrderJupiterTest {

    static WebDriver driver;

    @BeforeAll
    static void setup() {
        driver = WebDriverManager.chromedriver().create();
    }

    @AfterAll
    static void teardown() {
        driver.quit();
    }

    @Test
    @Order(1)
    void testA() {
        // Test logic
    }

    @Test
    @Order(2)
    void testB() {
        // Test logic
    }

    @Test
    @Order(3)
    void testC() {
        // Test logic
    }

}
```

## Selenium-Jupiter

与往常一样，基于 Selenium-Jupiter 的测试也使用 Jupiter 编程模型，因此这些特性（如测试排序）同样适用于 Selenium-Jupiter 测试。示例 8-16 展示了与之前相同的测试，但使用 Selenium-Jupiter 来实例化驱动。默认情况下，驱动对象在每个测试之前创建，并在每个测试之后终止。Selenium-Jupiter 提供了 `@SingleSession` 注解来改变这一行为，即在所有测试之前创建驱动实例，并在所有测试之后关闭会话。

## 示例 8-16. 使用 JUnit 5 和 Selenium-Jupiter 对测试排序

```java
@ExtendWith(SeleniumJupiter.class)
@TestMethodOrder(OrderAnnotation.class)
@SingleSession
class OrderSelJupTest {

    WebDriver driver;

    OrderSelJupTest(ChromeDriver driver) {
        this.driver = driver;
    }

    @Test
    @Order(1)
    void testA() {
        // Test logic
    }

    @Test
    @Order(2)
    void testB() {
        // Test logic
    }

    @Test
    @Order(3)
    void testC() {
        // Test logic
    }
}
```

## 故障分析

*故障分析*（也称为*故障排查*）是收集和分析数据以发现故障原因的过程。对于 Selenium WebDriver 测试而言，这一过程可能具有挑战性，因为测试的是整个系统，而且测试失败的根本原因可能有多个。例如，端到端测试失败的原因可能是客户端（前端）逻辑、服务端（后端），甚至与其他组件（如数据库或外部服务）的集成。

我们可以使用不同的技术来帮助开发人员和测试人员进行故障分析。一种典型做法是检测测试何时失败，并在终止驱动会话之前收集一些数据以发现原因。例如，以下资源可以在这一过程中提供帮助：

- 屏幕截图。测试失败后的 Web 应用程序界面截图可能有助于确定失败原因。第4章中的“屏幕截图”解释了如何使用 Selenium WebDriver API 进行截图。
- 浏览器日志。当错误发生时，JavaScript 控制台可能是另一个潜在的信息来源。第5章中的“日志收集”解释了如何执行日志收集。
- 会话录制。当在 Docker 容器中使用浏览器时，我们可以轻松录制浏览器会话。第6章中的“Docker 容器中的浏览器”解释了如何使用 WebDriverManager 和 Selenium-Jupiter 实现这一点。

以下小节提供了为失败的测试生成浏览器截图的基本示例。为此，我们需要依靠单元测试特有的特性来检测失败的测试。

## JUnit 4

JUnit 允许通过使用*规则*来调整测试的默认行为。测试类通过在类属性上标注 `@Rule` 注解来定义规则。表 8-3 总结了 JUnit 4 开箱即用的规则。

## 表 8-3. JUnit 4 中的规则

| 规则 | 描述 | 示例 |
|---|---|---|
| ErrorCollector | 它允许在发生异常时继续执行测试（同时收集这些异常） | `@Rule`<br>`public ErrorCollector collector = new ErrorCollector();`<br><br>`@Ignore`<br>`@Test`<br>`public void test()` |
| TemporaryFolder | 它允许创建临时文件和文件夹 | `@Rule`<br>`public TemporaryFolder folder = new TemporaryFolder();`<br><br>`@Test`<br>`public void test() throws IOException {`<br>`    File file = folder.newFile("myfile.txt");`<br>`}` |
| Timeout | 它将超时应用于类中的所有测试方法 | `@Rule`<br>`public Timeout timeout = new Timeout(10, SECONDS);`<br><br>`@Test`<br>`public void test() {`<br>`    while (true);`<br>`}` |
| TestWatcher | 它允许捕获测试的几个执行阶段：`starting`、`succeeded`、`failed`、`skipped` 和 `finished`。 | `@Rule`<br>`public TestWatcher watcher = new TestWatcher() {`<br>`    @Override`<br>`    protected void succeeded(Description d) {`<br>`        log.debug("Test succeeded: {}", d.getMethodName());`<br>`    }`<br><br>`    @Override`<br>`    protected void failed(Throwable e, Description d) {`<br>`        log.debug("Test failed: {}", d.getMethodName());`<br>`    }`<br>`};` |

我们可以使用 `TestWatcher` 规则收集数据，以便在 JUnit 4 中进行故障分析。示例 8-17 展示了在测试失败时捕获屏幕截图的测试。示例 8-18 包含此规则的实现。如前所述，我们进行浏览器截图。该截图的逻辑见示例 8-19。

## 示例 8-17. 使用 JUnit 4 分析失败的测试

```java
public class FailureJUnit4Test {

    static WebDriver driver;

    @Rule
    public TestRule testWatcher = new FailureWatcher(driver); ①
```





```java
@BeforeClass
public static void setup() {
    driver = WebDriverManager.chromedriver().create();
}

@AfterClass
public static void teardown() {
    driver.quit();
}

@Test
public void testFailure() {
    driver.get("https://bonigarcia.dev/selenium-webdriver-java/");
    fail("Forced error");
}
```

+   1. 我们在类级别定义规则，将 driver 实例作为参数传递。
+   2. 我们强制该测试失败以便使用规则截取浏览器截图。

## *示例 8-18. 使用 JUnit 4 分析失败的测试*

```java
public class FailureWatcher extends TestWatcher {

    FailureManager failureManager;

    public FailureWatcher(WebDriver driver) {
        failureManager = new FailureManager(driver);
    }

    @Override
    public void failed(Throwable throwable, Description description) {
        failureManager.takePngScreenshot(description.getDisplayName());
    }
}
```

+   ❶ 我们将失败分析的逻辑封装在单独的类中。
+   
+   ❷ 我们重写测试失败时触发的方法。在此情况下，我们只需使用失败管理器实例来截取屏幕截图。

## 示例 8-19. 使用 JUnit 4 分析失败的测试

```java
public class FailureManager {

    static final Logger log = getLogger(lookup().lookupClass());

    WebDriver driver;

    public FailureManager(WebDriver driver) {
        this.driver = driver;
    }

    public void takePngScreenshot(String filename) {
        TakesScreenshot ts = (TakesScreenshot) driver;
        File screenshot = ts.getScreenshotAs(OutputType.FILE);
        Path destination = Paths.get(filename + ".png");

        try {
            Files.move(screenshot.toPath(), destination);
        } catch (IOException e) {
            log.error("Exception moving screenshot from {} to {}",
                screenshot,
                destination, e);
        }
    }
}
```

+   ❶ 我们将截图保存为 PNG 文件，文件名作为参数传入。

## TestNG

TestNG 开箱即用地提供了多个 *监听器*。这些监听器是捕获测试生命周期不同事件的类。例如，`ITestResult` 监听器允许监控测试的状态和结果。

如示例 8-20 所示，我们可以轻松地使用此监听器在 Selenium WebDriver 测试中实现失败分析。

## 示例 8-20. 使用 TestNG 分析失败的测试

```java
public class FailureNGTest {

    WebDriver driver;

    @BeforeMethod
    public void setup() {
        driver = WebDriverManager.chromedriver().create();
    }

    @AfterMethod
    public void teardown(ITestResult result) { ①
        if (result.getStatus() == ITestResult.FAILURE) { ②
            FailureManager failureManager = new
FailureManager(driver); ③
            failureManager.takePngScreenshot(result.getName());
        }

        driver.quit();
    }

    @Test
    public void testFailure() {
        driver.get("https://bonigarcia.dev/selenium-webdriver-java/");
        fail("Forced error");
    }
}
```

+   ① 我们在测试 teardown 方法中声明一个 ITestResult 参数。
+   ② 我们读取测试的状态。
+   ③ 在失败的情况下，我们创建失败管理器实例（我们使用示例 8-19 中描述的相同逻辑）以创建屏幕截图。

## JUnit 5

在 JUnit 5 中，Jupiter 扩展模型取代并改进了基于规则的 JUnit 4 测试生命周期管理。如第 2 章介绍的那样，Jupiter 提供的扩展模型允许在 Jupiter 编程模型之上添加新功能。这样，Jupiter 扩展就是一个 Java 类，它实现一个或多个 *扩展点*，这些接口允许在 Jupiter 编程模型中进行不同类型的操作。表 8-4 提供了 Jupiter 提供的扩展点的简要总结。

## 表-4. Unit5 扩展点

| Category | Description | Extension point(s) |
|---|---|---|

| Test lifecycle callbacks | To include custom logic during the test lifecycle | `BeforeAllCallback` `BeforeEachCallback` `BeforeTestExecutionCallback` `AfterTestExecutionCallback` `AfterEachCallback` `AfterAllCallback` |
| Parameter resolution | To inject parameters in test methods or constructors | `ParameterResolver` |
| Test templates | To implement tests using `@TestTemplate` | `TestTemplateInvocationContextProvider` |
| Conditional test execution | To enable or disable tests depending on custom conditions | `ExecutionCondition` |
| Exception handling | To handle exceptions during the test and its lifecycle | `TestExecutionExceptionHandler` `LifecycleMethodExecutionExceptionHandler` |
| Test instance | To create and process test class instances | `TestInstanceFactory` `TestInstancePostProcessor` `TestInstancePreDestroyCallback` |
| Intercepting invocations | To intercept calls to test code (and decide whether or not these calls proceed) | `InvocationInterceptor` |

实现失败分析的一个方便的扩展点是 `AfterTestExecutionCallback`，因为它允许在单个测试执行后立即包含自定义逻辑。示例 8-21 提供了一个使用自定义注解（见示例 8-22）实现此扩展点的 Jupiter 测试。

## *示例 8-21. 使用 JUnit 5 分析失败的测试*

```java
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

+   ❶ 我们为此类中的测试使用 `FailureWatcher` 扩展。我们将 driver 实例作为参数传递。
+   
+   ❷ 我们强制失败以使扩展截取浏览器屏幕截图。

## 示例 8-22. 使用 JUnit 5 分析失败的测试

```java
public class FailureWatcher implements AfterTestExecutionCallback {
    FailureManager failureManager;
    public FailureWatcher(WebDriver driver) {
        failureManager = new FailureManager(driver);
    }
    @Override
    public void afterTestExecution(ExtensionContext context) throws Exception {
        if (context.getExecutionException().isPresent()) {
            failureManager.takePngScreenshot(context.getDisplayName());
        }
    }
}
```

① 该扩展实现了一个扩展点：AfterTestExecutionCallback。

② 该扩展点必须重写此方法，该方法在每个测试后立即执行。

③ 我们检查是否存在执行异常。

④ 如果存在，我们使用 WebDriver 实例截取屏幕截图。

## Selenium-Jupiter

Selenium-Jupiter 是一个 Jupiter 扩展，其中包括其他功能，可以轻松地截取浏览器屏幕截图。示例 8-23 演示了此功能。

## 示例 8-23. 使用 Selenium-Jupiter 分析 JUnit 5 中的失败

```java
class FailureSelJupTest {
    @RegisterExtension
    static SeleniumJupiter seleniumJupiter = new SeleniumJupiter();
```

```java
@BeforeAll
static void setup() {
    seleniumJupiter.getConfig().enableScreenshotWhenFailure();
}

@Test
void testFailure(ChromeDriver driver) {
    driver.get("https://bonigarcia.dev/selenium-webdriver-java/");
    fail("Forced error");
}
}
```

① Selenium-Jupiter 只需使用此配置功能即可在测试失败时截取浏览器屏幕截图。

## 重试测试

如第 7 章所述，测试不稳定（即缺乏可靠性）是端到端测试中众所周知的问题。作为测试人员，我们有时需要识别不稳定测试（即在相同条件下通过或失败的测试），为此，我们重试给定测试以检查其结果是否一致。因此，我们可能希望在失败时重试测试的机制。本节解释如何使用不同的单元测试框架执行此过程。

## JUnit 4

我们需要使用自定义 JUnit 4 规则来重试失败的测试。示例 8-24 显示了一个使用此类规则示例的测试，示例 8-25 包含该规则的源代码。

## 示例 8-24. 使用 JUnit 4 重试测试

```java
public class RandomCalculatorJUnit4Test {

    static WebDriver driver;

    @Rule
    public RetryRule retryRule = new RetryRule(5); ①
```


@BeforeClass
public static void setup() {
    driver = WebDriverManager.chromedriver().create();
}

@AfterClass
public static void teardown() {
    driver.quit();
}

@Test
public void testRandomCalculator() {
    driver.get(
        "https://bonigarcia.dev/selenium-webdriver-java/random-calculator.html");
    // 1 + 3
    driver.findElement(By.xpath("//span[text()='1']")).click();

    driver.findElement(By.xpath("//span[text()='+']")).click();
    driver.findElement(By.xpath("//span[text()='3']")).click();
    driver.findElement(By.xpath("//span[text()='=']")).click();

    // ... should be 4
    String result =
    driver.findElement(By.className("screen")).getText();
    assertThat(result).isEqualTo("4");
}
}

① 我们将重试规则声明为测试属性。

② 我们对所有重复使用相同的浏览器。

③ 我们打开一个名为随机计算器的练习网页。该页面被设计为以给定的百分比产生错误结果（默认为 50%）。然后，计算器在可配置的次数后工作正常（默认为 5 次）。

④ 我们使用计算器 GUI 进行基本的算术运算。

⑤

我们验证结果。首次五次尝试中有 50% 的概率得到错误结果。

## 示例 8-25. 用于重试失败测试的 JUnit 4 规则

```
public class RetryRule implements TestRule {

    static final Logger log = getLogger(lookup().lookupClass());

    int maxRetries;

    public RetryRule(int maxRetries) {
        this.maxRetries = maxRetries;
    }

    @Override
    public Statement apply(Statement base, Description description) {
        return new Statement() {
            @Override
            public void evaluate() throws Throwable {
                Throwable throwable = null;
                for (int i = 0; i < maxRetries; i++) {
                    try {
                        base.evaluate();
                        return;
                    } catch (Throwable t) {
                        throwable = t;
                        log.debug("{}: run {} failed", description.getDisplayName(), i + 1);
                    }
                }
                log.debug("{}: giving up after {} failures", description.getDisplayName(), maxRetries);
                throw throwable;
            }
        };
    }
}
```

① 我们实现 JUnit 4 规则的通用接口，即 TestRule。

② 该规则在其构造函数中接受一个整数值，用于确定最大重试次数。

- ③ 我们需要重写 `apply` 方法，该方法允许操作测试生命周期。
- ④ 我们在循环中重复测试执行，重复的最大次数等于重试次数。
- ⑤ 如果测试执行期间出现错误，我们获取异常对象并重复测试执行。
- ⑥ 如果执行到此行，意味着测试已重复最大次数。

## TestNG

TestNG 提供了自定义功能来实现测试重试。如 *示例 8-26* 所示，我们使用 `@Test` 注解的 `retryAnalyzer` 属性来启用此功能。*示例 8-27* 显示了该重试分析器的实现。

*示例 8-26. 使用 TestNG 重试测试*

```java
@Test(retryAnalyzer = RetryAnalyzer.class)
public void testRandomCalculator() {
    // Same logic than the example before
}
```

*示例 8-27. TestNG 的测试分析器*

```java
public class RetryAnalyzer implements IRetryAnalyzer {

    static final int MAX_RETRIES = 5;

    int retryCount = 0;

    @Override
    public boolean retry(ITestResult result) {
        if (retryCount <= MAX_RETRIES) {
            retryCount++;
            return true;
        }
        return false;
    }
}
```

- 1. 我们需要实现一个名为 `IRetryAnalyzer` 的 TestNG 监听器来实现重试分析器。
- 2. 我们无法参数化此类，因此在类中声明最大重试次数（在此情况下为常量）。
- 3. 我们需要重写 `retry` 方法。此方法返回一个布尔值，用于确定测试在失败时是否重试。
- 4. 确定此值的逻辑是一个累加器，检查是否达到重试阈值。

## JUnit 5

我们需要使用前面解释的扩展模型（参见 **表 8-4**）来重试失败的测试。与其重新发明轮子，我们可以使用现有的开源 Jupiter 扩展来实现此目的。关于重试测试，正如 **第 2 章** 中介绍的那样，有多种替代方案：**JUnit Pioneer** 或 **rerunner-jupiter**。**示例 8-28** 显示了使用后者的测试。

*示例 8-28. 使用 JUnit 5 重试测试*

```java
@RepeatedIfExceptionsTest(repeats = 5)
void testRandomCalculator() {
    // Same logic than the example before
}
```

- 1. 只需用此注解装饰测试，我们就会在失败时重复测试最多 5 次（本例中为 5 次）。

## Selenium-Jupiter

使用 Selenium-Jupiter 的测试也可以使用其他扩展。**示例 8-29** 显示了如何在 Selenium-Jupiter 测试中使用 rerunner-jupiter。

*示例 8-29. 使用 Selenium-Jupiter 的 JUnit 5 重试测试*

```java
@SingleSession
@ExtendWith(SeleniumJupiter.class)
class RandomCalculatorSelJupTest {
```

```java
@RepeatedIfExceptionsTest(repeats = 5)
void testRandomCalculator(ChromeDriver driver) {
    // Same logic than the example before
}
}
```

① 我们对所有可能的重复重用相同的浏览器。

## 并行测试执行

执行 Selenium WebDriver 测试套件（尤其是测试数量很多时）所需的时间可能相当可观。造成这种缓慢的原因是常规的 Selenium WebDriver 测试每次都会启动一个新浏览器，从而导致整体执行时间增加。解决此问题的可能方案是并行执行测试。实现这种并行化有不同的方法。首先，我们可以使用构建工具（Maven 或 Gradle）提供的内置并行执行功能。其次，我们可以使用单元测试框架（JUnit 4 或 5，以及 TestNG）提供的功能来实现此目的。以下小节解释了所有这些选项。

## Maven

Maven 提供了不同的并行执行机制。首先，Maven 允许并行构建多模块项目的模块。为此，我们需要使用选项 -T 从命令行调用 Maven 命令。此选项接受两种类型的并行化参数：使用固定数量的线程或使用乘以系统可用 CPU 核心数的因子。以下代码片段显示了每种类型的示例：

```
mvn test -T 4 ①
mvn test -T 1C ②
```

① 它使用四个线程并行执行多模块项目（例如 examples 仓库）的测试。

② 它使用与 CPU 核心数相同的线程数执行多模块项目的测试（例如在四核系统上使用四个线程）。

此外，Maven 中用于执行单元测试的插件（称为 *Surefire*）提供了两种并行运行测试的方法。第一种是在单个 JVM 进程内多线程。为启用此模式，我们需要指定不同的配置参数，例如：

- `parallel`: 配置并行化的粒度级别。此参数的可能值有 `methods`（在单独线程中执行测试方法）、`classes`（测试类）、`suites`（测试套件）、`suitesAndClasses`（测试套件和类）、`suitesAndMethods`（测试套件和方法）以及 `all`（在单独线程中执行每个测试）。
- `threadCount`: 定义并行化的最大线程数。
- `useUnlimitedThreads`: 允许无限数量的线程。

指定这些配置参数有两种方法。首先，我们可以直接在 Maven 配置文件（即 `pom.xml` 文件）上配置它们。示例 8-30 演示了如何操作。此外，我们也可以在使用命令行时将这些参数指定为系统属性，例如如下所示：

```
mvn test -Dparallel=classesAndMethods -DthreadCount=4
```

## 示例 8-30. 用于并行执行的 Maven Surefire 配置示例

```
<build>
    <plugins>
```




```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-surefire-plugin</artifactId>
    <version>${maven-surefire-plugin.version}</version>
    <configuration>
        <parallel>classesAndMethods</parallel>
        <threadCount>4</threadCount>
    </configuration>
</plugin>
</plugins>
</build>
```

使用 Maven Surefire 实现并行性的第二种方式是 *forking*，即创建多个 JVM 进程。如果我们需要防止线程级并发问题，这种选项会很有帮助，因为不同进程不共享内存空间，而多线程中会共享。作为缺点，forking 会消耗更多内存且性能更低。要启用 forking，我们需要将 `forkCount` 配置属性（同样在 `pom.xml` 中或作为系统属性）设置为大于一的值，即要创建的 JVM 进程数。例如，以下命令使用四个 JVM 进程执行 Maven 项目的测试：

```bash
mvn test -DforkCount=4
```

## Gradle

Gradle 也提供了多种并行执行测试的方式。首先，它允许在多模块项目中并行执行任务。有两种方式可以启用此模式。第一种是在配置文件 `gradle.properties` 中设置属性 `org.gradle.parallel=true`。第二种是在命令中使用选项 `--parallel`，例如如下：

```bash
gradle test --parallel
```

此外，我们可以在 Gradle 配置文件中使用配置属性 `maxParallelForks` 来指定并行启动的最大测试进程数。默认情况下，Gradle 每次只执行一个测试类。我们通过为此参数设置大于一的值来改变这种默认行为。除了固定值之外，我们还可以按如下方式指定系统可用 CPU 核心数：

```groovy
maxParallelForks = Runtime.runtime.availableProcessors()
```

在示例仓库中，此属性使用名为 `parallel` 的 profile 有条件地启用（参见附录 C）。因此，我们可以使用命令行按如下方式使用此 profile：

```bash
gradle test -Pparallel
```

## JUnit 4

JUnit 通过类 `ParallelComputer` 提供了一种基本方式来并行执行测试。此类的构造函数接受两个布尔参数，分别用于启用类和方法的并行测试执行。示例 8-31 显示了使用此类的测试。

### 示例 8-31. 使用 JUnit 4 并行测试执行

```java
public class ParallelJUnit4Suite {

    @Test
    public void runInParallel() {
        Class<?>[] classes = { ParallelJUnit4Test.class,
                ParallelJUnit4Test.class };
        JUnitCore.runClasses(new ParallelComputer(true, true),
                classes);
    }
}
```

- 我们指定哪些测试类并行执行。
- 我们为测试类和方法启用并行测试执行。

## TestNG

在 TestNG 中指定测试并行执行的一种常见方式是通过配置文件 `testng.xml`。启用此模式在 TestNG 中最相关的属性是：

- `parallel`：它指定并行运行测试的模式。可选值为 `methods`、`tests` 和 `classes`。
- `thread-count`：它设置并行运行测试的默认最大线程数。

示例 8-32 显示了 `testng.xml` 用于测试并行的基本配置。

### 示例 8-32. TestNG 的并行测试配置

```xml
<!DOCTYPE suite SYSTEM "https://testng.org/testng-1.0.dtd">
<suite name="parallel-suite" parallel="classes" thread-count="2">
    <test name="parallel-tests">
        <classes>
            <class name="io.github.bonigarcia.webdriver.testng.ch08.parallel.Parallel1NGTest"/>
            <class name="io.github.bonigarcia.webdriver.testng.ch08.parallel.Parallel2NGTest"/>
        </classes>
    </test>
</suite>
```

我们可以使用 Maven 或 Gradle 在命令行上按如下方式运行之前的并行测试套件：

```bash
mvn test -Dsurefire.suiteXmlFiles=src/test/resources/testng.xml
gradle test -Psuite=src/test/resources/testng.xml
```

## JUnit 5

JUnit 5 允许以不同方式并行执行测试。以下列表总结了用于此目的最相关的配置参数：

- `junit.jupiter.execution.parallel.enabled`：启用测试并行的布尔标志（默认为 `false`）。
- `junit.jupiter.execution.parallel.mode.classes.default`：并行运行测试类。可选值为单线程执行的 `same_thread`（默认）和并行执行的 `concurrent`。
- `junit.jupiter.execution.parallel.mode.default`：并行运行测试方法。可选值与测试类相同。

指定这些参数有两种方式。首先，在配置文件 `junit-platform.properties` 中（应可在项目类路径中获得）。**示例 8-33** 显示了此文件的内容示例。其次，通过使用系统属性和命令行。以下命令（Maven/Gradle）展示了如何操作：

```bash
mvn test -Djunit.jupiter.execution.parallel.enabled=true
gradle test -Djunit.jupiter.execution.parallel.enabled=true
```

### 示例 8-33. 使用 JUnit 5 并行测试执行

```properties
junit.jupiter.execution.parallel.enabled = true
junit.jupiter.execution.parallel.mode.default = same_thread
junit.jupiter.execution.parallel.mode.classes.default = same_thread
```

此外，Jupiter 编程模型提供了注解 `@Execution` 来更改测试类或方法的并行化模式。为此目的，此注解可用于类级别或方法级别，并接受两个值：`ExecutionMode.CONCURRENT`（用于并行执行）和 `ExecutionMode.SAME_THREAD`（用于单线程执行）。**示例 8-34** 显示了示例仓库中包含的测试类的结构。假设并行测试执行已启用（如 **示例 8-33** 中所示），此类将与其他允许并行化的测试一起并行执行。

### 示例 8-34. 使用 JUnit 5 并行测试执行

```java
@Execution(ExecutionMode.CONCURRENT)
class ParallelJupiterTest {

    // Test logic
```

## 用于轻量级单图像超分辨率的多注意力融合网络

Zhen Huang, Panpan Wang, Zunkai Huang, Jiandong Gan, Yifan Zhang

合肥工业大学计算机与信息学院，合肥 230009，中国  
663442489@qq.com, wangpanpan@hfut.edu.cn

**摘要—** 基于深度学习的单图像超分辨率（SISR）算法取得了显著成就，但由于参数数量巨大和计算开销高，其实际应用受到限制。本文提出了一种轻量级但高效的多注意力融合网络（MAFN），以粗到精的方式提取特征。具体而言，我们提出了一个特征融合组（FFG），由特征提取块（FEB）、注意力融合块（AFB）和特征细化块（FRB）组成。FEB 使用具有不同感受野的两个不同分支来提取粗略特征。AFB 设计用于通过融合通道注意力和空间注意力机制来细化不同分支的特征。FRB 用于从融合特征中进一步提取精细特征。我们还引入了跨层特征注意力融合（CFAF）模块来融合来自不同 FFG 的特征，这可以有效提高网络的表示能力。此外，我们提出了一个轻量级上采样模块以减少上采样模块引入的参数。大量实验表明，与其他最先进的轻量级 SISR 方法相比，我们的 MAFN 在重建质量和视觉效果方面均实现了优越的性能。

*关键词—* 超分辨率、轻量级网络、注意力机制、多注意力融合

## I. 引言

单图像超分辨率（SISR）是计算机视觉领域的一个经典问题，其目标是从对应的低分辨率（LR）图像重建高分辨率（HR）图像。SISR 技术在现实场景中具有广泛的应用，例如医学成像、遥感和视频监控。近年来，基于深度学习的方法在 SISR 中取得了显著成功。Dong 等人 [1] 首先将卷积神经网络（CNN）引入 SISR 任务并提出了超分辨率卷积神经网络（SRCNN）。此后，提出了一系列基于深度学习的 SISR 方法，如超深超分辨率（VDSR）[2]、增强型深度超分辨率（EDSR）[3] 和深度残差通道注意力网络（RCAN）[4]。这些方法极大地提高了 SISR 的重建性能。

然而，这些方法的优异性能往往伴随着大量的参数和高计算复杂度，这阻碍了它们在资源受限设备（如手机和嵌入式设备）上的部署。为解决此问题，研究人员提出了各种轻量级 SISR 方法。例如，Ahn 等人 [5] 提出了级联残差网络（CARN），该网络使用级联机制来重用特征并减少参数。Hui 等人 [6] 提出了信息多蒸馏网络（IMDN），该网络使用信息多蒸馏机制高效提取特征。高效超分辨率网络（ESRN）[7] 和残差特征蒸馏网络（RFDN）[8] 进一步提高了轻量级 SISR 的性能。此外，一些方法 [9]–[12] 设计了高效的注意力机制，通过建模特征的重要性来提高轻量级网络的表示能力。

尽管这些方法取得了进展，但现有的轻量级 SISR 方法仍存在一些局限性。一方面，它们中的大多数使用单一类型的注意力机制，无法充分捕捉图像中的复杂特征信息。另一方面，多尺度信息在轻量级网络中经常被忽略，限制了网络的表示能力。为克服这些局限性，我们提出了一种用于轻量级 SISR 的多注意力融合网络（MAFN）。本文的主要贡献总结如下：





// Other listeners
}

在 JUnit 4 中，注册测试监听器的一种常见方式是创建自定义运行器，并在测试类中使用该运行器。示例 8-36 展示了一个注册了前面监听器的自定义测试运行器。然后，示例 8-35 展示了一个使用此运行器的测试骨架。

## 示例 8-36：使用 JUnit 4 的测试监听器

```
public class MyTestRunner extends BlockJUnit4ClassRunner { ①

    public MyTestRunner(Class<?> clazz) throws InitializationError
    {
        super(clazz);
    }

    @Override
    public void run(RunNotifier notifier) {
        notifier.addListener(new MyTestListener()); ②
        super.run(notifier); ③
    }
}
```

- ① 我们继承了 JUnit 4 中的默认测试运行器 `BlockJUnit4ClassRunner`。
- ② 我们注册自定义测试监听器。
- ③ 我们调用父类以继续使用默认测试运行器。

## 示例 8-37：使用 JUnit 4 的测试监听器

```
@RunWith(MyTestRunner.class) ①
public class ListenersJUnit4Test {

    // Test logic

}
```

我们使用 JUnit 4 的注解 @RunWith 和我们自定义的运行器来装饰测试类。

## TestNG

TestNG 提供了用于实现测试监听器的接口 `ITestListener`。实现该接口的类可以针对 TestNG 生命周期的不同阶段重写方法，例如 `onTestSuccess`、`onTestFailure`、`onTestSkipped` 等。示例 8-38 展示了一个实现该接口的示例类。在此示例中，监听器方法会在标准输出中记录一条消息。然后，示例 8-38 展示了一个使用此监听器的测试。

## 示例 8-38：使用 TestNG 的测试监听器

```
public class MyTestListener implements ITestListener {

    static final Logger log = getLogger(lookup().lookupClass());

    @Override
    public void onTestStart(ITestResult result) {
        ITestListener.super.onTestStart(result);
        log.debug("onTestStart {}", result.getName());
    }

    @Override
    public void onTestFailure(ITestResult result) {
        ITestListener.super.onTestFailure(result);
        log.debug("onTestFailure {}", result.getThrowable());
    }

    // Other listeners

}
```

## 示例 8-39：使用 TestNG 的测试监听器

```
@Listeners(MyTestListener.class) ①
public class ListenersNGTest {

    // Test logic

}
```

① 我们使用 TestNG 的注解 @Listeners 来指定该类中的所有测试都使用我们的自定义测试监听器。

## JUnit 5

如前所述（参见表 8-4），Jupiter 提供了多种扩展点，用于在 JUnit 5 测试生命周期中加入自定义逻辑。除了这种扩展模型外，JUnit 5 还允许实现测试监听器，以跟踪多个测试执行阶段，例如测试开始、跳过或结束。此功能可通过 JUnit Launcher API 使用，该 API 是用于在 JUnit Platform 中发现、过滤和执行测试的 API（参见第 2 章的图 2-4）。

要在 JUnit 5 中创建测试监听器，我们需要实现 `TestExecutionListener` 接口。实现该接口的类可以重写不同的方法，以便在测试执行期间收到事件通知。示例 8-40 包含一个实现该接口的基本类。这类监听器在 JUnit 5 中通过标准的 Java 服务加载器机制进行注册。为此，我们需要在项目类路径中创建一个名为 `/META-INF/services/org.junit.platform.launcher.TestExecutionListener` 的文件，并写入我们要注册的测试监听器的完全限定名（例如，对于示例 8-40，为 `io.github.bonigarcia.webdriver.jupiter.ch08.listeners.MyTestListener`）。请注意，该文件未包含在示例仓库中，以避免干扰整个测试套件。

## 示例 8-40：使用 JUnit 5 的测试监听器

```
public class MyTestListener implements TestExecutionListener {
    static final Logger log = getLogger(lookup().lookupClass());
    @Override
    public void executionStarted(TestIdentifier testIdentifier) {
    TestExecutionListener.super.executionStarted(testIdentifier);
```

```
log.debug("Test execution started {}",
testIdentifier.getDisplayName());
}

@Override
public void executionSkipped(TestIdentifier testIdentifier,
String reason) {

TestExecutionListener.super.executionSkipped(testIdentifier,
reason);
    log.debug("Test execution skipped: {}", reason);
}

@Override
public void executionFinished(TestIdentifier testIdentifier,
    TestExecutionResult testExecutionResult) {

TestExecutionListener.super.executionFinished(testIdentifier,
    testExecutionResult);
    log.debug("Test execution finished {}",
    testExecutionResult.getStatus());
}
}
```

> **注意**
> 
> `TestExecutionListener` 接口属于 JUnit Platform Launcher API，因此要使用它，我们需要在项目中包含该 API 作为额外依赖。附录 C 说明了所需的 Maven 和 Gradle 配置。

## 禁用测试

单元测试框架允许以编程方式禁用（即在测试执行中跳过）整个测试类或单个测试方法。以下小节将说明 JUnit 4、TestNG、JUnit 5 和 Selenium-Jupiter 之间的差异。

### JUnit 4

JUnit 4 提供了 `@Ignore` 注解来禁用测试。该注解可用于类或方法级别。我们还可以在注解中包含一条消息，以指定禁用原因。示例 8-41 包含一个禁用的测试。

## 示例 8-41：使用 JUnit 4 禁用测试

```
@Ignore("Optional reason for disabling")
@Test
public void testDisabled() {
    // Test logic
}
```

### TestNG

TestNG 允许以两种不同的方式禁用测试。首先，我们可以对测试类或方法使用 @Ignore 注解。其次，我们可以使用 @Test 注解的 enabled 属性。示例 8-42 演示了这两种方法。

## 示例 8-42：使用 TestNG 禁用测试

```
@Ignore("Optional reason for disabling")
@Test
public void testDisabled1() {
    // Test logic
}

@Test(enabled = false)
public void testDisabled2() {
    // Test logic
}
```

### JUnit 5

Jupiter 编程模型提供了多种基于不同条件禁用测试的注解。表 8-5 总结了这些注解，示例 8-43 提供了一个使用其中部分注解的基本示例。

表 8-5. Jupiter 用于禁用测试的注解

| 注解 | 描述 |
|---|---|
| @Disabled | 禁用测试类或方法 |
| @DisabledOnJre<br>@EnabledOnJre | 根据 Java 版本禁用/启用 |
| @DisabledOnJreRange<br>@EnabledOnJreRange | 根据 Java 版本范围禁用/启用 |
| @DisabledOnOs<br>@EnabledOnOs | 根据操作系统（如 Windows、Linux、Mac 等）禁用/启用 |
| @DisabledIfSystemProperty<br>@DisabledIfSystemProperties<br>@EnabledIfSystemProperty<br>@EnabledIfSystemProperties | 根据系统属性的值禁用/启用 |
| @DisabledIfEnvironmentVariable<br>@DisabledIfEnvironmentVariables<br>@EnabledIfEnvironmentVariable<br>@EnabledIfEnvironmentVariables | 根据环境变量的值禁用/启用 |
| @DisabledIf<br>@EnabledIf | 根据自定义方法的布尔返回值禁用/启用 |

## 示例 8-43：使用 JUnit 5 禁用测试

```
@Disabled("Optional reason for disabling")
@Test
public void testDisabled1() {
    // Test logic
}

@DisabledOnJre(JAVA_8)
@Test
public void testDisabled2() {
    // Test logic
}
```

```
@EnabledOnOs(MAC) ③
@Test
public void testDisabled3() {
    // Test logic
}
```

① 我们始终跳过此测试。
② 如果使用 Java 8，我们跳过此测试。
③ 在任何非 macOS 的操作系统上，我们跳过此测试。

### Selenium-Jupiter

Selenium-Jupiter 提供了额外的注解，可根据 Selenium WebDriver 测试的特定条件有条件地禁用测试。这些条件包括浏览器可用性、Docker 可用性以及 URL 在线（即使用 GET HTTP 方法请求该 URL 时返回 200 响应码）。示例 8-44 展示了几个使用这些注解的测试。

## 示例 8-44：使用 Selenium-Jupiter 的 JUnit 5 禁用测试

```
@EnabledIfBrowserAvailable(SAFARI) ①
@Test
void testDisabled1(SafariDriver driver) {
    // Test logic
}

@EnabledIfDockerAvailable ②
@Test
void testDisabled2(@DockerBrowser(type = CHROME) WebDriver driver)
{
    // Test logic
}

@EnabledIfDriverUrlOnline("http://localhost:4444/") ③
@Test
void testDisabled3(
    @DriverCapabilities("browserName=chrome") WebDriver driver)
{
    // Test logic
}
```



1. 如果系统不可用 Safari，则跳过此测试。
2. 如果系统不可用 Docker，则跳过此测试。
3. 如果 Selenium Server URL 不在线，则跳过此测试。如果是这样，则执行测试，并使用之前的 URL 创建 `RemoteWebDriver` 的实例。为了指定所需的功能，我们在此测试中使用注解 `@DriverCapabilities`（如第 6 章所述）。

## Summary and Outlook

本章介绍了本书中使用的测试框架（即 JUnit 4、TestNG、JUnit 5 和 Selenium-Jupiter）用于开发 Selenium WebDriver 测试的一些最相关特定功能。首先，你学习了如何实现参数化测试。此功能可用于跨浏览器测试（即使用不同的浏览器进行 Web 测试）十分方便。然后，你学习了如何对测试进行分类并使用这些类别将其包含或排除在测试执行之外。你继续理解了故障分析机制（例如在测试失败时截取浏览器屏幕截图）、重试测试或并行执行测试。最后，你发现了如何实现测试监听器以及禁用测试的不同机制。

在下一章中，你将学习如何将 Selenium WebDriver 与不同的第三方实用程序集成以实现高级端到端测试。你将发现如何从 Web 应用程序下载文件、在不使用 CDP 的情况下捕获流量（例如在 Firefox 中）、测试非功能性需求（例如性能、安全或可访问性）、处理不同的输入数据、改进测试报告，以及与现有框架（如 Spring 或 Cucumber）集成。
