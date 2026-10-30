

```java
assertThat(checkbox2.isSelected()).isTrue();

WebElement radio2 = driver.findElement(By.id("my-radio-2"));
radio2.click();
assertThat(radio2.isSelected()).isTrue();
}
```

## 用户手势

Selenium WebDriver 提供了 `Actions` 类，这是一个强大的工具，可用于自动化不同的用户操作，包括键盘和鼠标操作。该类遵循*构建者*模式。这样，你可以链式调用多个方法（即不同的操作），并最终通过调用 `build()` 一次性执行所有操作。表 3-11 总结了该类中可用的公共方法。我们将在接下来的小节中通过示例来回顾这些方法。

### 表 3-11. Actions 方法

| 方法 | 描述 |
|---|---|
| keyDown(CharSequence key) | 在当前位置（或指定元素）发送单个按键（可以是使用 Keys 类的特殊字符）。该键保持按下状态，直到调用 keyUp() |
| keyDown(WebElement target, CharSequence key) |  |
| `keyUp(CharSequence key)` | 释放之前用 `keyDown()` 按下的键 |
| `keyUp(WebElement target, CharSequence key)` |  |
| `sendKeys(CharSequence... keys)` | 在当前位置（或指定元素）发送按键序列。此方法与 `WebElement#sendKeys(CharSequence...)` 有两个不同点：1) 修饰键（例如 `Keys.CONTROL`、`Keys.SHIFT`）不会被显式释放。2) 不会重新聚焦元素，因此 `Keys.TAB` 应该可以正常工作 |
| `sendKeys(WebElement target, CharSequence... keys)` |  |
| `clickAndHold()` | 在当前位置（或指定元素中部）单击但不释放 |
| `clickAndHold(WebElement target)` |  |
| `release()` | 释放之前用 `clickAndHold()` 按下的鼠标左键 |
| `release(WebElement target)` |  |
| `click()` | 在当前位置（或指定元素）点击 |
| `click(WebElement target)` |  |
| `doubleClick()` | 在当前位置（或元素）双击 |
| `doubleClick(WebElement target)` |  |
| `contextClick()` | 在当前位置（或元素）右键单击 |
| `contextClick(WebElement target)` |  |
| `moveToElement(WebElement target)` | 将鼠标光标移动到指定元素的中部（或按给定的偏移量移动） |
| `moveToElement(WebElement target)` |  |
| `moveByOffset(int xOffset, int yOffset)` | 从当前位置（默认为 (0, 0)）按给定偏移量移动鼠标。 |
| `dragAndDrop(WebElement source, WebElement target)` | 该操作包含三个步骤：1) 在源元素位置的中部（或按给定偏移量移动后）按住鼠标并保持点击。2) 将鼠标移动到目标元素位置。3) 释放鼠标点击。 |
| `dragAndDropBy(WebElement source, int xOffset, int yOffset)` |  |
| `pause(long pause)` | 在操作链中执行暂停（单位毫秒，或使用 Java 的 `Duration`）。 |
| `pause(Duration duration)` |  |
| `build()` | 生成一个包含之前所有操作的复合操作。 |
| `perform()` | 执行复合操作。 |

## 右键单击和双击

你可以在练习网站上找到一个使用三个下拉菜单的演示页面（见图 3-8）。在此页面上，第一个下拉菜单在点击时出现

```
@Test
void testContextAndDoubleClick() {
    driver.get(
        "https://bonigarcia.dev/selenium-webdriver-java/dropdown-menu.html");
    Actions actions = new Actions(driver);

    WebElement dropdown2 = driver.findElement(By.id("my-dropdown-2"));
    actions.contextClick(dropdown2).build().perform();
    WebElement contextMenu2 = driver.findElement(By.id("context-menu-2"));
    assertThat(contextMenu2.isDisplayed()).isTrue();

    WebElement dropdown3 = driver.findElement(By.id("my-dropdown-3"));
    actions.doubleClick(dropdown3).build().perform();
    WebElement contextMenu3 = driver.findElement(By.id("context-menu-3"));
    assertThat(contextMenu3.isDisplayed()).isTrue();
}
```

- 1 我们在中间的下拉菜单上使用 contextClick()。
- 2 我们验证中间菜单正确显示。
- 3 我们在右侧下拉菜单上使用 doubleClick()。
- 4 我们验证右侧菜单正确显示。

## 鼠标悬停

第二个处理 Actions 的示例使用了一个实现鼠标悬停的示例网页。该页面显示四张图片。当鼠标指针悬停时，每张图片下方都会显示一个文本标签（图 3-9 显示的是鼠标位于第一张图片上时的页面）。示例 3-23 包含一个使用此页面的测试。

![](img/b67ec8e49249e340add6ed66ab413d78_158_0.png)

### 示例 3-23. 使用鼠标悬停的测试

```java
@Test
void testMouseOver() {
    driver.get(
        "https://bonigarcia.dev/selenium-webdriver-java/mouse-over.html");
    Actions actions = new Actions(driver);

    List<String> imageList = Arrays.asList("compass", "calendar",
    "award",
        "landscape");
    for (String imageName : imageList) { ①
        String xpath = String.format("//img[@src='img/%s.png']",
        imageName);
        WebElement image = driver.findElement(By.xpath(xpath)); ②
        actions.moveToElement(image).build().perform(); ③

        WebElement caption = driver.findElement(
            RelativeLocator.with(By.tagName("div")).near(image));
        ④
        assertThat(caption.getText()).containsIgnoringCase(imageName);
        ⑤
    }
}
```

① 我们遍历一个字符串列表来定位页面上的四张图片。

② 我们使用 XPath 查找每个 <img> Web 元素。

③ 我们使用 moveToElement() 将鼠标指针移动到每张图片的中部。

④ 我们使用相对定位器查找显示的标签。

⑤ 我们使用断言来验证文本符合预期。

## 拖放

示例 3-24 演示了拖放的使用。该测试使用了图 3-10 中所示的练习网页。

### 示例 3-24. 使用拖放的测试

```java
@Test
void testDragAndDrop() {
    driver.get(
        "https://bonigarcia.dev/selenium-webdriver-java/drag-and-drop.html");
    Actions actions = new Actions(driver);

WebElement draggable = driver.findElement(By.id("draggable")); ①
int offset = 100;
Point initLocation = draggable.getLocation();
actions.dragAndDropBy(draggable, offset, 0)
    .dragAndDropBy(draggable, 0, offset)
    .dragAndDropBy(draggable, -offset, 0)
    .dragAndDropBy(draggable, 0, -offset).build().perform(); ②
assertThat(initLocation).isEqualTo(draggable.getLocation()); ③

WebElement target = driver.findElement(By.id("target")); ④
actions.dragAndDrop(draggable, target).build().perform(); ⑤

assertThat(target.getLocation()).isEqualTo(draggable.getLocation()); ⑥
}
```

① 我们定位*可拖拽*元素。

② 我们使用 dragAndDropBy() 将此元素按固定像素数（100）移动四次（向右、向下、向左、向上）。

③ 我们断言元素位置与开始时相同。

④ 我们找到第二个元素（这次不可拖拽）。

⑤ 我们使用 dragAndDrop() 将可拖拽元素移动到第二个元素上。

⑥ 我们断言两个元素的位置相同。

## Hands-on Selenium WebDriver with Java（Java 实战 Selenium WebDriver）

练习网站

### 拖放

可拖拽面板

拖我

图 3-10. 包含可拖拽元素的练习网页

## 单击并按住

以下示例展示了如何使用复杂的用户手势，包括单击并按住。为此，我们在图 **3-11** 所示的网页上进行练习。该页面使用了一个名为 **Signature Pad** 的开源 JavaScript 库，通过鼠标在 HTML 画布上绘制签名。**示例 3-25** 展示了使用它的测试。

![](img/b67ec8e49249e340add6ed66ab413d78_162_0.png)

*图 3-11. 包含可绘制画布的练习网页*

### 示例 3-25. 在画布上绘制圆周的测试

```java
@Test
void testClickAndHold() {
    driver.get(
        "https://bonigarcia.dev/selenium-webdriver-java/draw-in-canvas.html");
    Actions actions = new Actions(driver);

    WebElement canvas = driver.findElement(By.tagName("canvas"));
    actions.moveToElement(canvas).clickAndHold();

    int numPoints = 10;
    int radius = 30;
    for (int i = 0; i <= numPoints; i++) {
        double angle = Math.toRadians(360 - i / numPoints);
        double x = Math.sin(angle) * radius;
        double y = Math.cos(angle) * radius;
        actions.moveByOffset((int) x, (int) y);
    }

    actions.release(canvas).build().perform();
}
```

- 1. 我们按标签名定位画布。
- 2. 我们使用 `moveToElement()` 将鼠标移动到该元素，然后将 `clickAndHold()` 操作（用于在画布上绘制）添加到操作管道中。
- 3. 我们使用固定数量的点进行迭代，利用方程式计算圆周上的点。
- 4. 我们使用圆周上的点（x 和 y）通过 `moveByOffset()` 移动鼠标。由于上一步中的点击仍处于按住状态，最终的复合操作将在鼠标按钮按下的同时移动鼠标。
- 5. 我们释放点击，构建操作，并执行整个操作链。结果，画布上应出现一个圆周。

## 复制和粘贴

