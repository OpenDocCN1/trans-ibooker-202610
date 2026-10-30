# Device Emulation

Another feature provided by CDP is the ability to emulate mobile devices (e.g., smartphones, tablets). Example 5-31 illustrate this usage. This test first overrides the user agent by sending the command Network.setUserAgentOverride(). Then, it emulates the device metrics sending the command Emulation.setDeviceMetricsOverride.

# Example 5-31. Test emulating a mobile device

```
@Test
void testDeviceEmulation() {
    // 1. Override user agent (Apple iPhone 6)
    String userAgent = "Mozilla/5.0 (iPhone; CPU iPhone OS 8_0 like Mac OS X) "
        + "AppleWebKit/600.1.3 (KHTML, like Gecko) "
        + "Version/8.0 Mobile/12A4345d Safari/600.1.4";
    devTools.send(Network.setUserAgentOverride(userAgent,
        Optional.empty(),
        Optional.empty(), Optional.empty())); ①

    // 2. Emulate device dimension
    Map<String, Object> deviceMetrics = new HashMap<>();
    deviceMetrics.put("width", 375);
    deviceMetrics.put("height", 667);
    deviceMetrics.put("mobile", true);
    deviceMetrics.put("deviceScaleFactor", 2);
    ((ChromeDriver) driver).executeCdpCommand(
        "Emulation.setDeviceMetricsOverride", deviceMetrics); ②

    driver.get("https://bonigarcia.dev/selenium-webdriver-java/");
    assertThat(driver.getTitle()).contains("Selenium WebDriver");
}
```

① We override the user agent for emulating an Apple iPhone 6.

② We override the device screen parameters.

# Console Listeners

CDP allows you to implement listeners to monitor the console events, i.e., a web page JavaScript log and error traces. Example 5-32 shows a test about