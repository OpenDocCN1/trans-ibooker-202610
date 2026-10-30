```
// Exercise
String sutUrl = "https://bonigarcia.dev/selenium-webdriver-java/";
    driver.get(sutUrl); ④
    String title = driver.getTitle(); ⑤
    log.debug("The title of {} is {}", sutUrl, title); ⑥

    // Verify
    assertThat(title).isEqualTo("Hands-On Selenium WebDriver with Java"); ⑦
}

@AfterEach
void teardown() {
    driver.quit(); ⑧
}
}
```

① We declare a Java attribute using the interface WebDriver. We use this variable in tests to control web browsers with Selenium WebDriver.

② In the setup for all tests within this class (i.e., executed once), we call WebDriverManager to manage the required driver. In this example, since we use Chrome as a browser, we need to resolve chromedriver.

③ In the test setup (executed once per test method), we instantiate the WebDriver object to control Chrome. In other words, we create an object of the type ChromeDriver.

④ The test logic uses the Selenium WebDriver API through the driver variable. First, the test exercises the SUT. To that aim, first, we open the practice site using the get() method of our webdriver variable (which represents a Chrome browser, in this case).

⑤ We get the web page title using the method getTitle().

⑥ For debugging purposes, we log that title using the DEBUG level.

⑦ The last part of the test contains an AssertJ assertion. In this case, we verify the web page title is as expected.