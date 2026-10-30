# Example 4-24. Test handling an alert dialog

```
@Test
void testAlert() {
    driver.get(
        "https://bonigarcia.dev/selenium-webdriver-java/dialog-boxes.html"); ①
    WebDriverWait wait = new WebDriverWait(driver,
    Duration.ofSeconds(5));

    driver.findElement(By.id("my-alert")).click(); ②
    wait.until(ExpectedConditions.alertIsPresent()); ③
    Alert alert = driver.switchTo().alert(); ④
    assertThat(alert.getText()).isEqualTo("Hello world!"); ⑤
    alert.accept(); ⑥
}
```

+   ① We open the practice web page that launches dialog boxes.
+   ② We click on the left button to launch a JavaScript alert.
+   ③ We wait until the alert dialog is displayed on the screen.
+   ④ We change the focus to the alert popup.
+   ⑤ We verify that the alert text is as expected.
+   ⑥ We click on the button ok of the alert dialog.

We can replace steps 3 and 4 with a single explicit wait statement, as follows (you can find it in a second test in the same class in the examples repository):

```
Alert alert = wait.until(ExpectedConditions.alertIsPresent());
```

The next test (Example 4-25) illustrates how to deal with a confirm dialog. Notice this example is quite similar to the previous one, but in this case, we can invoke the method dismiss() to click on the button cancel available on the confirm dialog. Finally, Example 4-26 shows how to manage a prompt dialog. In this case, we can type some string into the input text.