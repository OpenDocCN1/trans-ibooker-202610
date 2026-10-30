
- `boolean isHttpOnly`：此 cookie 是否为仅 HTTP 的 cookie，即该 cookie 无法通过客户端脚本访问（可选）。
- `String sameSite`：此 cookie 是否为同站 cookie，即该 cookie 仅限于第一方或同站上下文（可选）。

## 表 4-6. Cookie 方法

| 方法 | 返回 | 描述 |
|---|---|---|
| `getName()` | `String` | 读取 cookie 名称 |
| `getValue()` | `String` | 读取 cookie 值 |
| `getDomain()` | `String` | 读取 cookie 域 |
| `getPath()` | `String` | 读取 cookie 路径 |
| `isSecure()` | `boolean` | 读取 cookie 是否需要安全连接 |
| `isHttpOnly()` | `boolean` | 读取 cookie 是否仅 HTTP |
| `getExpiry()` | `Date` | 读取 cookie 过期日期 |
| `getSameSite()` | `String` | 读取 cookie 同站上下文 |
| `validate()` | `void` | 检查 cookie 的不同字段，如果遇到任何问题则抛出 `IllegalArgumentException` |
| `toJson()` | `Map<String, Object>` | 将 cookie 值映射为键值映射 |

以下示例展示了使用 WebDriver API 管理 Web cookie 的不同测试。这些示例使用一个练习网页，该网页在 GUI 上显示站点 cookie（参见图 4-5）：

- 示例 4-14 演示了如何读取网站的现有 cookie。
- 示例 4-15 展示了如何添加新 cookie。
- 示例 4-16 说明了如何编辑现有 cookie。
- 示例 4-17 演示了如何删除 cookie。

```text
Practice site

Cookies

Display cookies

username=John Doe

date=10/07/2018

Figure 4-5. Practice web page for web cookies
```

### 示例 4-14. 测试读取现有 cookie

```java
@Test
void testReadCookies() {
    driver.get("https://bonigarcia.dev/selenium-webdriver-java/cookies.html");

    Set<Cookie> cookies = driver.manage().getCookies();
    assertThat(cookies).hasSize(2);

    Cookie username = driver.manage().getCookieNamed("username");
    assertThat(username.getValue()).isEqualTo("John Doe");
    assertThat(username.getPath()).isEqualTo("/");

    driver.findElement(By.id("refresh-cookies")).click();
}
```

1. 我们读取此页面上所有可用的 cookie。其中应包含两个 cookie。
2. 我们读取名称为 `username` 的 cookie。
3. 上一个 cookie 的值应为 `John Doe`。
4. 最后一个语句对测试没有任何影响。我们调用此命令是为了让你在浏览器 GUI 中检查 cookie。

### 示例 4-15. 测试添加新 cookie

```java
@Test
void testAddCookies() {
    driver.get("https://bonigarcia.dev/selenium-webdriver-java/cookies.html");

    Cookie newCookie = new Cookie("new-cookie-key", "new-cookie-value");
    driver.manage().addCookie(newCookie);
    String readValue =
        driver.manage().getCookieNamed(newCookie.getName())
            .getValue();
    assertThat(newCookie.getValue()).isEqualTo(readValue);

    driver.findElement(By.id("refresh-cookies")).click();
}
```

- ① 我们创建一个新 cookie。
- ② 我们将该 cookie 添加到当前页面。
- ③ 我们读取刚添加的 cookie 的值。
- ④ 我们验证该值是否符合预期。

### 示例 4-16. 测试编辑现有 cookie

```java
@Test
void testEditCookie() {
    driver.get("https://bonigarcia.dev/selenium-webdriver-java/cookies.html");

    Cookie username = driver.manage().getCookieNamed("username");
    Cookie editedCookie = new Cookie(username.getName(), "new-value");
    driver.manage().addCookie(editedCookie);

    Cookie readCookie = driver.manage().getCookieNamed(username.getName());
    assertThat(editedCookie).isEqualTo(readCookie);

    driver.findElement(By.id("refresh-cookies")).click();
}
```

- ① 我们读取一个现有 cookie。
- ② 我们创建一个新 cookie，并复用之前的 cookie 名称。
- ③ 我们将新 cookie 添加到网页中。
- ④ 我们读取刚添加的 cookie。
- ⑤ 我们验证 cookie 是否已正确编辑。

### 示例 4-17. 测试删除现有 cookie

```java
@Test
void testDeleteCookies() {
    driver.get("https://bonigarcia.dev/selenium-webdriver-java/cookies.html");

    Set<Cookie> cookies = driver.manage().getCookies(); // ①
    Cookie username = driver.manage().getCookieNamed("username"); // ②
    driver.manage().deleteCookie(username); // ③

    assertThat(driver.manage().getCookies()).hasSize(cookies.size() - 1); // ④

    driver.findElement(By.id("refresh-cookies")).click();
}
```

- ① 我们读取所有 cookie。
- ② 我们读取名称为 username 的 cookie。
- ③ 我们删除之前的 cookie。
- ④ 我们验证 cookie 的数量是否符合预期。

## 下拉列表

Web 表单中的一个典型元素是下拉列表。这些字段允许用户在选项列表中选择一个或多个元素。用于渲染这些字段的经典 HTML 标签是 `<select>` 和 `<option>`。与往常一样，练习 Web 表单包含其中一个元素（参见图 4-6），其 HTML 定义如下：

```html
<select class="form-select" name="my-select">
    <option selected>Open this select menu</option>
    <option value="1">One</option>
    <option value="2">Two</option>
    <option value="3">Three</option>
</select>
```

```text
Practice site

Web form

Text input

Password

Textarea

Disabled input

Disabled input

Readonly input

Readonly input

Dropdown (select)

- Open this select menu
- One
- Two
- Three

- Checked checkbox
- Default checkbox
- Checked radio
- Default radio

Submit

Color picker

Date picker

Example range

Return to index
```

![](img/b67ec8e49249e340add6ed66ab413d78_216_0.png)

*图 4-6. 练习 Web 表单中的选择字段*

这些元素在 Web 表单中非常普遍。因此，Selenium WebDriver 提供了一个名为 `Select` 的辅助类来简化对它们的操作。该类包装了一个 select `WebElement`，并提供了多种功能。表 4-7 总结了 `Select` 类中可用的公开方法。之后，示例 4-18 展示了一个使用该类的基本测试。

## 表 4-7. Select 方法

| 方法 | 返回 | 描述 |
|---|---|---|
| `Select(WebElement element)` | `Select` | 构造函数，使用 WebElement 作为参数（它必须是一个 `<select>` 元素），否则抛出 `UnexpectedTagNameException` |
| `getWrappedElement()` | `WebElement` | 获取包装的 WebElement（即构造函数中使用的那个） |
| `isMultiple()` | `boolean` | select 元素是否支持选择多个选项 |
| `getOptions()` | `List<WebElement>` | 读取属于 select 的所有选项 |
| `getAllSelectedOptions()` | `List<WebElement>` | 读取所有选中的选项 |
| `getFirstSelectedOption()` | `WebElement` | 读取第一个选中的选项 |
| `selectByVisibleText(String text)` | `void` | 选择匹配给定显示文本的所有选项 |
| `selectByIndex(int index)` | `void` | 按索引号选择一个选项 |
| `selectByValue(String value)` | `void` | 按 value 属性选择选项 |
| `deselectAll()` | `void` | 取消选择所有选项 |
| `deselectByValue(String value)` | `void` | 按 value 属性取消选择选项 |
| `deselectByIndex(int index)` | `void` | 按索引号取消选择 |
| `deselectByVisibleText(String text)` | `void` | 取消选择匹配给定显示文本的选项 |

### 示例 4-18. 测试与 select 字段交互

```java
@Test
void test() {
    driver.get("https://bonigarcia.dev/selenium-webdriver-java/web-form.html");

    Select select = new Select(driver.findElement(By.name("my-select"))); // ①

    String optionLabel = "Three";
    select.selectByVisibleText(optionLabel); // ②

    assertThat(select.getFirstSelectedOption().getText()).isEqualTo(optionLabel); // ③
}
```

- ① 我们按名称找到 select 元素，并使用得到的 WebElement 实例化一个 Select 对象。
- ② 我们使用按文本匹配的策略，选择此 select 中可用的一个选项。
- ③ 我们验证所选选项的文本是否符合预期。

## 数据列表元素

在 HTML 中实现下拉列表的另一种方式是使用数据列表。虽然从图形角度来看，数据列表与 select 元素非常相似，但它们之间存在明显区别。一方面，select 字段显示一个选项列表，用户从可用选项中选择一个（或几个）。另一方面，数据列表显示与输入表单（文本）字段相关联的建议选项列表，用户可以自由选择其中一个建议值或输入自定义值。练习 Web 表单包含其中一个数据列表。你可以在下面的代码片段中找到其标记，截图见图 4-7。

```html
<input class="form-control" list="my-options" name="my-datalist" placeholder="Type to search...">
<datalist id="my-options">
    <option value="San Francisco">
    <option value="New York">
    <option value="Seattle">
    <option value="Los Angeles">
    <option value="Chicago">
</datalist>
```
