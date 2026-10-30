
通过 id，使用 CSS 选择器和 XPath 查询定位练习 Web 表单中的第一个输入文本元素。

![](img/b67ec8e49249e340add6ed66ab413d78_130_0.png)

图 3-5. 在 Firefox 开发者工具中搜索 CSS 选择器和 XPath

## 复合定位器

Selenium WebDriver API 提供了几个支持类，使我们能够组合前面看到的各种定位器类型。这些类如下：

- ByIdOrName (String idOrName)：按 id 查找，如果不可用，则按 name 查找。
- ByChained (By... bys)：按顺序查找元素（即第二个元素应出现在第一个元素内部，依此类推）。
- ByAll (By... bys)：查找匹配多个定位策略的元素（对这些定位器采用 and 逻辑条件）。

示例 3-13 展示了一个使用 ByIdOrName 的测试。该测试查找练习 Web 表单中可用的以下文件选择字段。请注意，该字段指定了 name 属性（但没有指定 id）。

```
<input class="form-control" type="file" name="my-file">
```

### 示例 3-13：使用 by id 或 name 复合定位器的测试

```
@Test
void testByIdOrName() {
    driver.get(
        "https://bonigarcia.dev/selenium-webdriver-java/web-form.html");

    WebElement fileElement = driver.findElement(new ByIdOrName("my-file")); ①
    assertThat(fileElement.getAttribute("id")).isBlank(); ②
    assertThat(fileElement.getAttribute("name")).isNotBlank(); ③
}
```

- ① 我们使用按 id 或 name 定位的定位器。
- ② 我们检查该元素具有 name 属性。
- ③ 我们验证同一元素中不存在 name 属性。

示例 3-14 展示了两个测试，说明 ByChained 和 ByAll 之间的区别。这两个定位器再次使用了练习 Web 表单。如果检查其源代码，您会注意到 `<form>` 内部有四个单独的 `<div class="row">`。

### 示例 3-14：使用 by chained 和 by all 复合定位器的测试

```
@Test
void testByChained() {
    driver.get(
        "https://bonigarcia.dev/selenium-webdriver-java/web-form.html");

    List<WebElement> rowsInForm = driver.findElements(
        new ByChained(By.tagName("form"), By.className("row"))); ①
    assertThat(rowsInForm.size()).isEqualTo(1); ②
}

@Test
void testByAll() {
    driver.get(
        "https://bonigarcia.dev/selenium-webdriver-java/web-form.html");

    List<WebElement> rowsInForm = driver.findElements(
        new ByAll(By.tagName("form"), By.className("row"))); ③

    assertThat(rowsInForm.size()).isEqualTo(5); ④
}
```

- 1. 我们使用 ByChained 定位器。
- 2. 因为表单内只有一个 row 元素，所以我们找到一个元素。
- 3. 我们使用 ByAll 定位器。
- 4. 我们找到五个元素，因为该定位器匹配页面上的一个 `<form>` 元素加上四个 `<div class="row">`。

## 相对定位器

Selenium WebDriver 4 版引入了一种在网页中查找元素的新方法：相对定位器。这些新定位器旨在利用相对于另一个已知元素的位置来查找 Web 元素。该功能基于 CSS 盒模型。该模型规定 Web 文档中的每个元素都使用矩形框进行渲染。图 3-6 展示了练习表单中某个 Web 元素的盒模型示例。

利用此盒模型，Selenium WebDriver API 中可用的相对定位器允许根据另一个 Web 元素的位置来查找元素。为此，首先我们需要使用标准定位策略（例如按 id、name、attribute 等）定位该 Web 元素。然后，我们需要使用 RelativeLocator 类的静态方法 with() 指定通过邻近原始 Web 元素获得的定位器类型。这样，我们得到一个 RelativeBy 对象，它扩展了标准定位策略中使用的抽象类 By。RelativeBy 对象提供以下方法来实现相对定位：

- above()：查找位于原始元素上方的元素。
- below()：查找位于原始元素下方的元素。
- near()：查找靠近原始元素的元素。默认认为一个元素靠近另一个元素的距离为 100 像素。该定位器被重载，可指定其他距离。
- toLeftOf()：查找位于原始元素左侧的元素。
- toRightOf()：查找位于原始元素右侧的元素。

示例 3-15 展示了一个使用相对定位器的基本测试。我们再次使用练习 Web 表单来演示此功能。

### 示例 3-15：使用相对定位器的测试

```
@Test
void testRelativeLocators() {
    driver.get(
        "https://bonigarcia.dev/selenium-webdriver-java/web-form.html");

    WebElement link = driver.findElement(By.linkText("Return to index")); ①
    RelativeBy relativeBy = RelativeLocator.with(By.tagName("input")); ②
    WebElement readOnly = driver.findElement(relativeBy.above(link)); ③
    assertThat(readOnly.getAttribute("name")).isEqualTo("my-readonly"); ④
}
```

- ① 我们定位文本为 `Return to index` 的链接。
- ② 我们指定相对定位器类型，即按标签名 `input` 定位。
- ③ 我们使用相对定位器在原始 Web 元素（即链接）上方查找一个 Web 元素（应为 `input` 字段）。
- ④ 我们验证参考链接上方的元素是只读字段（参见图 3-2 进行核对）。

**警告**

相对定位器有助于根据其他元素的相对位置查找元素。另一方面，这种策略对页面布局非常敏感。例如，在响应式页面中使用相对定位器时需要小心，因为布局会随视口而变化。

## 一个具有挑战性的示例

到目前为止，我们看到的示例都相当简单。现在让我们看一个更复杂的用例。在练习网站中，有一个非默认元素：*日期选择器*。顾名思义，该元素提供了一种使用 Web 图形界面选择日期的便捷方式。由于练习网站使用的 CSS 框架是 Bootstrap，我使用 bootstrap-datepicker 来实现日期选择器。这个日期选择器附加到一个输入字段上。当用户单击此字段时，网页上会出现一个日历（参见图 3-7）。用户可以通过导航到不同的天、月和年，点击首选日期来选择给定日期。

我们希望使用 Selenium WebDriver 实现一个自动化测试，通过与日期选择器图形界面交互来选择当前日期和月份，但年份为前一年。**示例 3-16** 展示了最终的实现。

> **提示**
> 
> 要跟随此示例，我建议您在浏览器中打开练习 Web 表单（参见代码示例中的 URL），并使用开发者工具检查日期选择器的内部元素，注意所使用的不同选择器策略。

### 示例 3-16：与日期选择器交互的测试

```
@Test
void testDatePicker() {
    driver.get(
        "https://bonigarcia.dev/selenium-webdriver-java/web-form.html");

    // Get the current date from the system clock
    Calendar today = Calendar.getInstance();
    int currentYear = today.get(Calendar.YEAR);

    int currentDay = today.get(Calendar.DAY_OF_MONTH);

    // Click on the date picker to open the calendar
    WebElement datePicker = driver.findElement(By.name("my-date"));
    datePicker.click();

    // Click on the current month by searching by text
    WebElement monthElement = driver.findElement(By.xpath(
        String.format("//th[contains(text(),'%d')]", currentYear)));
    monthElement.click();

    // Click on the left arrow using relative locators
    WebElement arrowLeft = driver.findElement(
        RelativeLocator.with(By.tagName("th")).toRightOf(monthElement));
    arrowLeft.click();

    // Click on the current month of that year
    WebElement monthPastYear = driver.findElement(RelativeLocator
        .with(By.cssSelector("span[class$='focused']")).below(arrowLeft));
    monthPastYear.click();
}
```
