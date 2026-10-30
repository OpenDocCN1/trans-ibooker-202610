

# Java 版 Selenium WebDriver 实战

练习站点

# Web 表单

文本输入框

下拉框（select）

颜色选择器

密码

下拉框（datalist）

日期选择器

多行文本框

示例范围

禁用的输入框

只读的输入框

- 旧金山
- 纽约
- 西雅图
- 洛杉矶
- 芝加哥

打开此选择菜单

输入以搜索...

提交

返回索引

![](img/b67ec8e49249e340add6ed66ab413d78_221_0.png)

*图 4-7. 练习 Web 表单中的数据列表字段*

Selenium WebDriver 没有提供用于操作数据列表的自定义辅助类。相反，我们需要像处理标准输入文本一样与它们交互，区别在于，当点击输入字段时，会显示其选项。示例 4-19 展示了一个说明这一点的测试。

## 示例 4-19. 与数据列表字段交互的测试

```
@Test
void testDatalist() {
    driver.get(
        "https://bonigarcia.dev/selenium-webdriver-java/web-form.html");

    WebElement datalist = driver.findElement(By.name("my-datalist")); ①
    datalist.click(); ②

    WebElement option = driver
        .findElement(By.xpath("//datalist/option[2]")); ③
    String optionValue = option.getAttribute("value"); ④
    datalist.sendKeys(optionValue); ⑤

    assertThat(optionValue).isEqualTo("New York"); ⑥
}
```

- ① 我们定位用于数据列表的输入字段。
- ② 我们点击它以显示其选项。
- ③ 我们找到第二个选项。
- ④ 我们读取所定位选项的值。
- ⑤ 我们在输入字段中输入该值。
- ⑥ 我们断言选项值符合预期。

## 导航目标

使用浏览器导航网页时，默认情况下，我们使用与导航栏中 URL 对应的单个页面。然后，我们可以打开

## 通过语义分块提升大语言模型的长上下文推理能力

Zihan Wang, Yunxuan Li, Mintong Kang, Jun Li, Zhangyang Wang, Rahul G. Krishnan, Terrence J. Sejnowski, Yixuan Li

加州大学圣迭戈分校 | 德克萨斯大学奥斯汀分校 | 索尔克研究所

### 摘要

大语言模型（LLMs）在一系列广泛的自然语言处理任务中展现了卓越的性能。然而，它们在长上下文推理方面仍然存在困难。在本文中，我们提出了语义分块（semantic chunking），一种将文档划分为语义连贯的块以改进长上下文推理的方法。实验表明，我们的方法在多个基准测试上优于固定长度分块。

### 1 引言

大语言模型（LLMs）的最新进展使其能够处理长文档。然而，由于信息往往分布在许多段落中，长上下文推理仍然具有挑战性。我们研究了分块策略如何影响 LLM 的性能，并表明语义分块可以显著改善检索与推理。

![](img/b67ec8e49249e340add6ed66ab413d78_223_0.png)

图 1：语义分块与固定长度分块对比的总体示意图。

表 4-8. Target Locator 方法

| 方法 | 返回类型 | 描述 |
|---|---|---|

| frame(int index) | WebDriver | 按索引号将焦点切换到某个框架（或 iframe） |
| frame(String nameOrId) | WebDriver | 按名称或 id 将焦点切换到某个框架（或 iframe） |
| frame(WebElement frameElement) | WebDriver | 将焦点切换到先前定位为 WebElement 的框架（或 iframe） |
| parentFrame() | WebDriver | 将焦点切换到父上下文 |
| window(String nameOrHandle) | WebDriver | 按名称或句柄将焦点切换到另一个窗口。窗口句柄是一个十六进制字符串，用于唯一标识某个窗口或标签页 |
| newWindow(WindowType typeHint) | WebDriver | 创建一个新的浏览器窗口（使用 WindowType.WINDOW）或标签页（使用 WindowType.TAB），并将焦点切换到它 |
| defaultContent() | WebDriver | 选择主文档（使用 iframe 时）或页面上的第一个框架（使用 frameset 时） |
| activeElement() | WebElement | 获取当前选中的元素 |
| alert() | Alert | 将焦点切换到窗口警告框（详见“对话框”） |

## 标签页与窗口

示例 4-20 展示了一个测试，其中我们打开一个新的标签页来导航第二个网页。示例 4-21 是等价场景，但为第二个网页打开了一个新窗口。请注意，两个示例之间的区别仅在于参数 WindowType.TAB 和 WindowType.WINDOW。

## 示例 4-20. 打开新标签页的测试

```
@Test
void testNewTab() {
```

```java
driver.get("https://bonigarcia.dev/selenium-webdriver-java/");

String initHandle = driver.getWindowHandle();

driver.switchTo().newWindow(WindowType.TAB);
driver.get(
    "https://bonigarcia.dev/selenium-webdriver-java/web-form.html");
assertThat(driver.getWindowHandles().size()).isEqualTo(2);

driver.switchTo().window(initHandle);
driver.close();
assertThat(driver.getWindowHandles().size()).isEqualTo(1);
```
```

- 1. 我们导航到一个网页。
- 2. 我们获取当前窗口句柄。
- 3. 我们打开一个新标签页，并将焦点切换到它。
- 4. 我们打开另一个网页（由于焦点在第二个标签页中，因此该网页会在第二个标签页中打开）。
- 5. 我们验证此时的窗口句柄数量为 2。
- 6. 我们使用初始窗口的句柄将焦点切换到初始窗口。
- 7. 我们（仅）关闭当前窗口。第二个标签页保持打开。
- 8. 我们验证现在的窗口句柄数量为 1。

## 示例 4-21. 打开新窗口的测试

```
```java
@Test
void testNewWindow() {
    driver.get("https://bonigarcia.dev/selenium-webdriver-java/");
    String initHandle = driver.getWindowHandle();

    driver.switchTo().newWindow(WindowType.WINDOW);
    driver.get(
        "https://bonigarcia.dev/selenium-webdriver-java/web-form.html");
```

```
assertThat(driver.getWindowHandles().size()).isEqualTo(2);

driver.switchTo().window(initHandle);
driver.close();
assertThat(driver.getWindowHandles().size()).isEqualTo(1);
}
```

① 这一行在两个示例中有所不同。在本例中，我们打开一个新窗口（而不是标签页）并将焦点切换到它。

## 框架与内联框架

示例 4-22 展示了一个测试，其中被测网页包含一个 iframe。示例 4-23 展示的是等价场景，但使用了 frameset。

## 示例 4-22. 处理 iframe 的测试

```
@Test
void testIFrames() {
    driver.get(
        "https://bonigarcia.dev/selenium-webdriver-java/iframes.html"); ①

    WebDriverWait wait = new WebDriverWait(driver,
    Duration.ofSeconds(10));
    wait.until(ExpectedConditions
        .frameToBeAvailableAndSwitchToIt("my-iframe")); ②

    By pName = By.tagName("p");

wait.until(ExpectedConditions.numberOfElementsToBeMoreThan(pName,
0)); ③
    List<WebElement> paragraphs = driver.findElements(pName);
    assertThat(paragraphs).hasSize(20); ④
}
```

① 我们打开一个包含 iframe 的网页（见图 4-8）。
② 我们使用显式等待来等待框架可用并切换到它。
③ 我们使用另一个显式等待，暂停直到 iframe 中包含的段落可用。
④ 我们断言段落数量符合预期。

# Java 版 Selenium WebDriver 实战

练习站点

# 内联框架

Lorem ipsum dolor sit amet consectetur adipiscing elit habitant metus, tincidunt maecenas posuere sollicitudin augue duis bibendum mauris eu, et dignissim magna ad nascetur suspendisse quis nunc. Fames est ligula molestie aliquam pretium bibendum nullam, sociosqu maecenas mus etiam consequat ornare leo, sem mattis varius luctus itora senectus. Parturient quis tristique erat natoque tortor nascetur, primis augue vivamus habitasse senectus porta leo, aenean potenti ante a nam.

Nioh netus aliquet nam mattis vestibulum interdum euismod suspendisse rutrum ullamcorper venenatis, vehicula curabitur viverra habitasse leo vulputate eget malesuada ad tincidunt, donec posuere bibendum varius sollicitudin aenean accumsan lacinia consequat mi. Litora vel turpis luctus nec quisque hendrerit morbi maecenas, netus conubia feugiat orci fringilla suspendisse iaculis erat, semper posuere integer et senectus justo curabitur. Fringilla ut taciti mollis morbi blandit dictumst odio ultrices, parturient feugiat vel porttitor convallis eros pellentesque pretium risus, varius accumsan litora habitasse cursus pharetra commodo.

Varius bibendum volutpat porttitor habitant quis quam vehicula cras, facilisi natoque ornare viverra vestibulum aliquet aliquam. Libero class porttitor hac iaculis mauris ligula mattis turpis, tincidunt leo vivamus velit massa praesent ante, torquent eleifend ullamcorper scelerisque nec dictum imperdiet. Tincidunt purus nostra felis fusce varius at pellentesque, sociosqu accumsan phasellus interdum

版权所有 © 2021 Boni Garcia

图 4-8. 使用 iframe 的练习网页

## 示例 4-23. 处理框架的测试

