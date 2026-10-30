
JavascriptExecutor as shown in the following snippet. Then, we can use the executor (using variable `js` in the example) to invoke the methods presented in the table before.

```
WebDriver driver = new ChromeDriver();
JavascriptExecutor js = (JavascriptExecutor) driver;
```

## Synchronous Scripts

The method `executeScript()` of a JavascriptExecutor object allows executing a piece of JavaScript in the context of the current web page in a WebDriver session. The invocation of this method (in Java) blocks the control flow until the script terminates. Therefore, we typically use this method for executing synchronous scripts in a webpage under test. The method `executeScript()` allows two arguments:

- String script: Mandatory JavaScript fragment to be executed. This code is executed in the body of the current page as an anonymous function (i.e., a JavaScript function without a name).
- Object... args: Optional arguments script. These arguments must be one of the following types: number, boolean, string, WebElement, or a List of these types (otherwise, WebDriver throws an exception). These arguments are available in the injected script using the `arguments` built-in JavaScript variable.

When the script returns some value (i.e., the code contains a `return` statement), the Selenium WebDriver method `executeScript()` also returns a value in Java (otherwise, `executeScript()` returns null). The possible returned types are the following:

- WebElement: When returning an HTML element.
- Double: For decimals.
- Long: For non-decimal numbers.

3. Execute a piece of JavaScript code. In this case, we call the JavaScript function `scrollBy()` to scroll the document by a given amount (in this case, 1000px down). Notice that this fragment does not use `return`, and therefore, we do not receive any returned object in the Java logic. In addition, we are not passing any argument to the script.

Example 4-2 shows another test using scrolling and the same example web page as before. This time, instead of moving a fixed amount of pixels, we move the document scroll until a given element.

### Example 4-2. Test executing JavaScript to scroll down to a given element

```java
@Test
void testScrollIntoView() {
    driver.get(
        "https://bonigarcia.dev/selenium-webdriver-java/long-page.html");
    JavascriptExecutor js = (JavascriptExecutor) driver;

    driver.manage().timeouts().implicitlyWait(Duration.ofSeconds(10));

    WebElement lastElement = driver
        .findElement(By.cssSelector("p:last-child"));
    String script = "arguments[0].scrollIntoView();";
    js.executeScript(script, lastElement);
}
```

1. To make this test robust, we specify an implicit timeout. Otherwise, the test might fail if the page is not entirely loaded when executing the subsequent commands.
2. We locate the last paragraph in the webpage using a CSS selector.
3. We define the script to be injected into the page. Notice the script does not return any value, but as a novelty, it uses the first function argument to invoke the JavaScript function `scrollIntoView()`.
4. We execute the previous script, passing as an argument the located `WebElement`. This element will be the first argument for the script (i.e., `arguments[0]`).

The last example I will show you about scrolling has to do with *infinite scroll*. This technique enables the dynamic loading of more content when the user reaches the end of the web page. Automating this kind of webpages is an instructive use case since it involves different aspects of the WebDriver API. For example, you can use a similar approach to crawl web pages using Selenium WebDriver. Example 4-3 shows a test using an infinite scroll page.

### Example 4-3. Test executing JavaScript in an infinite scroll page

```java
@Test
void testInfiniteScroll() {
    driver.get(
            "https://bonigarcia.dev/selenium-webdriver-java/infinite-scroll.html");
    JavascriptExecutor js = (JavascriptExecutor) driver;
    WebDriverWait wait = new WebDriverWait(driver,
            Duration.ofSeconds(10));

    By pLocator = By.tagName("p");
    List<WebElement> paragraphs = wait.until(
            ExpectedConditions.numberOfElementsToBeMoreThan(pLocator, 0));
    int initParagraphsNumber = paragraphs.size();

    WebElement lastParagraph = driver.findElement(
            By.xpath(String.format("//p[%d]",
            initParagraphsNumber)));
    String script = "arguments[0].scrollIntoView();";
    js.executeScript(script, lastParagraph);

    wait.until(ExpectedConditions.numberOfElementsToBeMoreThan(pLocator,
            initParagraphsNumber));
}
```

1. We define an explicit wait since we need to pause the test until the new content is loaded.
2. We find the initial number of paragraphs on the page.
3. We locate the last paragraph of the page.
4. We scroll down into this element.
5. We wait until more paragraphs are available on the page.

## Color Picker

A *color picker* in HTML is an input type that allows users to select a color by clicking and dragging the cursor using a graphical area. The practice web form contains one of these elements (see Figure 4-2). The following code shows the HTML markup for the color picker. Notice that it sets an initial color value (otherwise, the default color is black).

```
<input type="color" class="form-control form-control-color"
name="my-colors"
value="#563d7c">
```

- Hands-on Selenium WebDriver with Java
- Practice site
- Web form
- Text input
- Password
- Textarea
- Disabled input
- Readonly input
- Dropdown (select)
- Dropdown (datalist)
- File input
- Color picker
- Checked checkbox
- Default checkbox
- Checked radio
- Default radio
- Submit
- Return to index

Figure 4-2. Color picker in the practice web form

Example 4-4 illustrates how to interact with this color picker. Because the WebDriver API does not provide any asset to control color pickers, we use JavaScript. In addition, this test also illustrates the use of Color, a support class available in the WebDriver API for working with colors.

### Example 4-4. Test executing JavaScript to interact with a color picker

```java
@Test
void testColorPicker() {
    driver.get(
        "https://bonigarcia.dev/selenium-webdriver-java/web-form.html");
    JavascriptExecutor js = (JavascriptExecutor) driver;

    WebElement colorPicker = driver.findElement(By.name("my-colors")); ①
    String initColor = colorPicker.getAttribute("value"); ②
    log.debug("The initial color is {}", initColor);

    Color red = new Color(255, 0, 0, 1); ③
    String script = String.format(
        "arguments[0].setAttribute('value', '%s');",
        red.asHex());
    js.executeScript(script, colorPicker); ④

    String finalColor = colorPicker.getAttribute("value"); ⑤
    log.debug("The final color is {}", finalColor);
    assertThat(finalColor).isNotEqualTo(initColor); ⑥
    assertThat(Color.fromString(finalColor)).isEqualTo(red);
}
```

1. We locate the color picker by name.
2. We read the initial value of the color picker (it should be #563d7c).
3. We define a color to work with using the following RGBA components: red=255 (maximum value), green=0 (minimum value), blue=0 (minimum value), and alpha=1 (maximum value, i.e., fully opaque).
4. We use JavaScript to change the value selected in the color picker.
5. We read the resulting value of the color picker (it should be #ff0000).
6. We assert that the color is different from the initial value, but as expected.

## Pinned Scripts

The WebDriver API allows you to pin scripts as of Selenium WebDriver 4. This feature enables attaching JavaScript fragments to a WebDriver session, assigning a unique key to each snippet, and executing these snippets on demand (even on different web pages). Example 4-5 shows a test using these pinned scripts.

### Example 4-5. Test executing JavaScript as pinned scripts

```java
@Test
void testPinnedScripts() {
    String initPage = "https://bonigarcia.dev/selenium-webdriver-java/";
    driver.get(initPage);
    JavascriptExecutor js = (JavascriptExecutor) driver;

    ScriptKey linkKey = js
        .pin("return document.getElementsByTagName('a')[2];");

    ScriptKey firstArgKey = js.pin("return arguments[0];");

    Set<ScriptKey> pinnedScripts = js.getPinnedScripts();
    assertThat(pinnedScripts).hasSize(2);

    WebElement formLink = (WebElement) js.executeScript(linkKey);
    formLink.click();
    assertThat(driver.getCurrentUrl()).isNotEqualTo(initPage);

    String message = "Hello world!";
    String executeScript = (String) js.executeScript(firstArgKey, message);
    assertThat(executeScript).isEqualTo(message);

    js.unpin(linkKey);
    assertThat(js.getPinnedScripts()).hasSize(1);
}
```
