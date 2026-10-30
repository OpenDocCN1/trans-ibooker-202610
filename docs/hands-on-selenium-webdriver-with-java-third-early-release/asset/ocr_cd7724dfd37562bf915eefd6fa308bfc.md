+   ⑥ We locate a list of elements by class.
+   ⑦ We verify the list has more than one element.
+   ⑧ We check the first element found by class is the same as the input text located before.

# Locating by Link Text

The last basic locator is by link text. This strategy is twofold: locate by exact and by partial text occurrence. We use a link in the practice web form to illustrate this locator in the following HTML markup. Then, **Example 3-8** shows a test using these locators.

```
```html
<a href="./index.html">Return to index</a>
```
```

# *Example 3-8. Test using locators by text link*

```
```java
@Test
void testByLinkText() {
    driver.get(
        "https://bonigarcia.dev/selenium-webdriver-java/web-form.html");

    WebElement linkByText = driver
        .findElement(By.linkText("Return to index"));
    assertThat(linkByText.getTagName()).isEqualTo("a");
    assertThat(linkByText.getCssValue("cursor")).isEqualTo("pointer");

    WebElement linkByPartialText = driver
        .findElement(By.partialLinkText("index"));
    assertThat(linkByPartialText.getLocation())
        .isEqualTo(linkByText.getLocation());

    assertThat(linkByPartialText.getRect()).isEqualTo(linkByText.getRect());
}
```
```

+   ❶ We locate an element by its full link text.
+   ❷ We check its tag name is `a`.