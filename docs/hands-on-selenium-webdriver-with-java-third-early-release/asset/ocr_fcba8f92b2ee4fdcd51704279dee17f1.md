```
@Test
void testFrames() {
    driver.get(
        "https://bonigarcia.dev/selenium-webdriver-java/frames.html"); ①

    WebDriverWait wait = new WebDriverWait(driver,
    Duration.ofSeconds(10));
    String frameName = "frame-body";
    wait.until(ExpectedConditions
        .presenceOfElementLocated(By.name(frameName))); ②
    driver.switchTo().frame(frameName); ③

    By pName = By.tagName("p");

wait.until(ExpectedConditions.numberOfElementsToBeMoreThan(pName,
0));
    List<WebElement> paragraphs = driver.findElements(pName);
    assertThat(paragraphs).hasSize(20);
}
```

① We open a web page that contains a frameset (see Figure 4-9).

② We wait for the frame to be available. Note that steps 2 and 3 in Example 4-22 are equivalent this step.

③ We change the focus to this frame.