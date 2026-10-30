WebDriver API. For example, you can use a similar approach to crawl web pages using Selenium WebDriver. Example 4-3 shows a test using an infinite scroll page.

# Example 4-3. Test executing JavaScript in an infinite scroll page

```
```java
@Test
void testInfiniteScroll() {
    driver.get(
            "https://bonigarcia.dev/selenium-webdriver-
java/infinite-scroll.html");
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
```

+   ❶ We define an explicit wait since we need to pause the test until the new content is loaded.
+   
+   ❷ We find the initial number of paragraphs on the page.
+   
+   ❸ We locate the last paragraph of the page.
+   
+   ❹ We scroll down into this element.
+   
+   ❺ We wait until more paragraphs are available on the page.