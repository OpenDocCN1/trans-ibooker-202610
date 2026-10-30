```
assertThat(driver.getWindowHandles().size()).isEqualTo(2);

driver.switchTo().window(initHandle);
driver.close();
assertThat(driver.getWindowHandles().size()).isEqualTo(1);
}
```

① This line is different in both examples. In this case, we open a new window (instead of a tab) and focus on it.

# Frames and Iframes

Example 4-22 shows a test in which the web page under test contains an iframe. Example 4-23 shows the equivalent case but using a frameset.

# Example 4-22. Test handling iframes

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

① We open a web page that contains an iframe (see Figure 4-8).

② We use an explicit wait for waiting for the frame and switching to it.

③ We use another explicit wait to pause until the paragraphs contained in the iframe are available.