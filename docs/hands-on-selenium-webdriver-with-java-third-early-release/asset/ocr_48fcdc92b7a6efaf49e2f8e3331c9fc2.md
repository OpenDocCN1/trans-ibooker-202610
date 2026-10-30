Selenium WebDriver does not provide a custom helper class to manipulate data lists. Instead, we need to interact with them as standard input texts, with the distinction that their options are displayed when clicking on the input field. Example 4-19 shows a test illustrating this.

# Example 4-19. Test interacting with a data list field

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

+   ① We locate the input field used for the data list.
+   ② We click on it to display its options.
+   ③ We find the second option.
+   ④ We read the value of the located option.
+   ⑤ We type that value in the input field.
+   ⑥ We assert the option value is as expected.

# Navigation Targets

When navigating web pages using a browser, by default, we use a single page corresponding to the URL in the navigation bar. Then, we can open