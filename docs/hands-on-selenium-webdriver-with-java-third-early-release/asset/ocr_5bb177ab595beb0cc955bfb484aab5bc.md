5 We read the resulting value of the color picker (it should be #ff0000).

6 We assert that the color is different from the initial value, but as expected.

# Pinned Scripts

The WebDriver API allows you to pin scripts as of Selenium WebDriver 4. This feature enables attaching JavaScript fragments to a WebDriver session, assigning a unique key to each snippet, and executing these snippets on demand (even on different web pages). Example 4-5 shows a test using this pinned scripts.

# Example 4-5. Test executing JavaScript as pinned scripts

```
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