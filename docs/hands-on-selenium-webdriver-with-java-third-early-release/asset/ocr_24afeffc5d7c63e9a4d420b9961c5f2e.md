```
```java
@Test
void testSlider() {
    driver.get(
        "https://bonigarcia.dev/selenium-webdriver-java/web-form.html");

    WebElement slider = driver.findElement(By.name("my-range"));
    String initValue = slider.getAttribute("value");
    log.debug("The initial value of the slider is {}", initValue);

    for (int i = 0; i < 5; i++) {
        slider.sendKeys(Keys.ARROW_RIGHT);
    }

    String endValue = slider.getAttribute("value");
    log.debug("The final value of the slider is {}", endValue);
    assertThat(initValue).isNotEqualTo(endValue);
}
```
```

> ❶ We send a keyboard key to the range field available in the practice web form. We use the class `Keys` available in the WebDriver API to handle special keyboard characters. In particular, we send the right arrow key to the slider, and as a result, it moves to the right (i.e., it increases the selected number within the range).

> ❷ We assert the resulting selected value is different from the one in the original position.

# Mouse Actions

In addition to the keyboard, the other primary input device for interacting with web applications is the computer mouse. First of all, the single-click (also known as left-click or simply *click*) is impersonated by the Selenium WebDriver API using the method `click()`, which is one of the methods available per `WebElement` in Selenium WebDriver. This section shows examples of two typical use cases using this feature: web navigation and interaction with checkboxes and radio buttons in web forms.

Other common mouse actions are right-clicking (also known as *context-click*), double-clicking, cursor movement, drag and drop, or mouse-over. Selenium WebDriver allows impersonating these actions using a helper class called