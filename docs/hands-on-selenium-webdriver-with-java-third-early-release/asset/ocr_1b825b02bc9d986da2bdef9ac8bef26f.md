```
public void teardown() {
    driver.quit();
}

@Parameters(name = "{index}: username={0} password={1} expectedText={2}")
public static Collection<Object[]> data() {
    return Arrays
        .asList(new Object[][] { { "user", "user", "Login successful" },
            { "bad-user", "bad-passwd", "Invalid credentials" } });
}

@Test
public void testParameterized() {
    driver.get(
        "https://bonigarcia.dev/selenium-webdriver-java/login-form.html");

    driver.findElement(By.id("username")).sendKeys(username);
    driver.findElement(By.id("password")).sendKeys(password);
    driver.findElement(By.cssSelector("button")).click();

    String bodyText =
    driver.findElement(By.tagName("body")).getText();
    assertThat(bodyText).contains(expectedText);
}
```

① We specify the Parameterized test runner for this Java class.

② We inject three test parameters as class attributes: username (index 0), password (index 1), and expected text (index 2).

③ We specify the test parameter in a method that returns a collection of generic parameters (Collection<Object[]>).

④ We return a collection of the three String sets to be used as a test parameter. The values of each entry will be injected using the previously declared three parameters (username, password, and expected text).