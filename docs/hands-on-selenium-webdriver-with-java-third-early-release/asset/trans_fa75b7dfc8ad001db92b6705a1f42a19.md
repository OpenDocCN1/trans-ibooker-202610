

由被测应用程序提供。这些类在不同的测试用例中实例化为页面对象。我们可以使用这些对象中公开的方法来实现端到端测试，同时避免代码重复。

让我们看一个使用页面对象的简单示例。在下面的示例中，我们使用页面对象而不是原生的 Selenium WebDriver，对上一节中解释的测试（即使用登录表单）进行重构。为此，第一步是创建一个建模登录页面的 Java 类。示例 7-3 显示了此页面类的非常基础的实现。

## 示例 7-3. 基础页面类，用于建模练习登录表单

```
public class BasicLoginPage {

    WebDriver driver; ①

    By usernameInput = By.id("username"); ②
    By passwordInput = By.id("password");
    By submitButton = By.cssSelector("button");
    By successBox = By.id("success");

    public BasicLoginPage(WebDriver driver) { ③
        this.driver = driver;

        driver.get(
            "https://bonigarcia.dev/selenium-webdriver-java/login-form.html");
    }

    public void with(String username, String password) { ④
        driver.findElement(usernameInput).sendKeys(username);
        driver.findElement(passwordInput).sendKeys(password);
        driver.findElement(submitButton).click();
    }

    public boolean successBoxPresent() { ⑤
        return driver.findElement(successBox).isDisplayed();
    }
}
```

① 我们声明一个 WebDriver 类属性。此变量用于在页面对象中实现与网页的交互。

2 我们将所有必需的定位器声明为附加属性。在此情况下，我们定位用户名和密码的文本输入框、提交按钮以及成功提示框。

3 此页面类定义的构造函数接受 WebDriver 对象。我们使用构造函数加载被测页面。

4 我们声明一个方法来建模登录所需的交互，即输入用户名和密码，然后点击提交按钮。

5 我们声明另一个方法来检查成功提示框是否可见。

现在，我们可以在测试用例中使用此页面类。示例 7-4 展示了如何使用。请注意，我们在每个测试之前像往常一样创建 WebDriver 实例，并在每个测试之后退出。我们将此 driver 作为参数传递给页面类构造函数。

## 示例 7-4. 使用基础页面类实现成功登录的测试

```
class BasicLoginJupiterTest {

    WebDriver driver;
    BasicLoginPage login; ①

    @BeforeEach
    void setup() {
        driver = WebDriverManager.chromedriver().create();
        login = new BasicLoginPage(driver); ②
    }

    @AfterEach
    void teardown() {
        driver.quit();
    }

    @Test
    void testBasicLoginSuccess() {
        login.with("user", "user"); ③
        assertThat(login.successBoxPresent()).isTrue(); ④
    }

}
```

- 1. 我们在测试类中将页面类声明为属性。
- 2. 我们创建页面对象并传入 WebDriver 实例。
- 3. 我们调用页面类定义的 with 方法来执行登录操作。
- 4. 我们使用页面对象提供的方法验证成功提示框是否出现在结果网页上。

这种方法对于提高测试的可维护性是一个很好的起点，因为现在所有与登录网页相关的逻辑都集中在一个可重用的类中。然而，页面类中的代码仍然很脆弱。例如，想象我们需要为登录页面实现一个负面测试，即使用错误凭据尝试登录。示例 7-5 在当前页面类的实现下似乎是一种合理的做法。然而，如果你运行此测试，你会发现测试由于 NoSuchElementException 异常而失败。下一节将解释如何通过创建更健壮的页面对象来解决此潜在问题。

## 示例 7-5. 使用基础页面类实现失败登录的测试

```
@Test
void testBasicLoginFailure() {
    login.with("bad-user", "bad-password");
    assertThat(login.successBoxPresent()).isFalse();
}
```

## 健壮的页面对象

上一节中介绍的示例改进了代码可维护性，因为页面操作被封装在单个类中，而不是散布在整个测试套件中。然而，有不同的方法可以增强之前的页面类实现。首先，我们的 SUT 很可能有多个网页，而不仅仅是一个。因此，常见的做法是遵循面向对象的思路并创建一个基础页面类，该类封装所有页面类的通用逻辑。示例 7-6 显示了一个实现页面类典型基类的 Java 类。

## 示例 7-6. 页面类的基类示例

```
public class BasePage {

    static final Logger log = getLogger(lookup().lookupClass());

    WebDriver driver;
    WebDriverWait wait;
    int timeoutSec = 5; // wait timeout (5 seconds by default)

    public BasePage(WebDriver driver) {
        this.driver = driver;
        wait = new WebDriverWait(driver,
        Duration.ofSeconds(timeoutSec)); ①
    }

    public void setTimeoutSec(int timeoutSec) { ②
        this.timeoutSec = timeoutSec;
    }

    public void visit(String url) { ③
        driver.get(url);
    }

    public WebElement find(By element) {
        return driver.findElement(element);
    }

    public void click(By element) {
        find(element).click();
    }

    public void type(By element, String text) {
        find(element).sendKeys(text);
    }

    public boolean isDisplayed(By locator) { ④
        try {
        wait.until(ExpectedConditions.visibilityOfElementLocated(locator));
        } catch (TimeoutException e) {
            log.warn("Timeout of {} wait for {}", timeoutSec,
        locator);

return false;
}
return true;
}
}
```

① 我们在基类中定义一个显式等待（WebDriverWait）属性。我们在构造函数中使用默认超时值（本示例中为 5 秒）实例化此属性。

② 我们创建一个 setter 方法来更改等待超时的默认值。例如，我们可能需要根据系统响应时间调整此超时。

③ 我们创建页面类可以重用的多个通用方法，例如 visit()（打开网页）、find()（定位网页元素）或 type()（向可写元素（如输入字段）发送数据）。

④ 我们实现一个方法来检查网页元素是否显示或不显示。请注意，此方法隐藏了等待该元素的复杂性，返回一个简单的布尔值供测试使用。

我们将之前的基类用作特定页面类的父类。例如，示例 7-7 显示了一个 Java 类，它扩展了此基类以实现练习站点中登录示例页面的页面类。

## 示例 7-7. 使用之前基类的登录页面类

```
public class LoginPage extends BasePage {

    By usernameInput = By.id("username"); ①
    By passwordInput = By.id("password");
    By submitButton = By.cssSelector("button");
    By successBox = By.id("success");

    public LoginPage(WebDriver driver, int timeoutSec) { ②
        this(driver);
        setTimeoutSec(timeoutSec);
    }

```java
public LoginPage(WebDriver driver) {
    super(driver);
    visit("https://bonigarcia.dev/selenium-webdriver-java/login-form.html");
}

public void with(String username, String password) {
    type(usernameInput, username);
    type(passwordInput, password);
    click(submitButton);
}

public boolean successBoxPresent() {
    return isDisplayed(successBox);
}

}
```
```

- 1. 我们将页面定位器定义为类属性。
- 2. 我们定义一个具有两个参数的构造函数：WebDriver 对象和超时值（以秒为单位）。
- 3. 我们定义另一个打开被测网页的构造函数。
- 4. 我们包含一个使用用户名和密码作为凭据登录的方法。此方法使用父类中定义的方法（`type()` 和 `click()`）。
- 5. 我们包含另一个方法来检查成功提示框是否可见或不可见（使用基类中定义的 `isDisplayed()` 方法）。

最后，我们可以使用页面类来实现 Selenium WebDriver 测试。**示例 7-8** 显示了一个使用 JUnit 5 的测试（通常，你可以在示例仓库中找到 JUnit 4、TestNG 和 Selenium-Jupiter 版本）。

## 示例 7-8. 使用页面类实现成功和失败登录的测试

```
```java
class LoginJupiterTest {

    WebDriver driver;
```

