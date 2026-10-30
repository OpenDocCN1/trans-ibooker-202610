```
try (NetworkInterceptor interceptor = new
NetworkInterceptor(driver,
    Route.matching(req -> req.getUri().endsWith(".png"))
        .to(() -> req -> new HttpResponse()
            .setContent(Contents.bytes(bytes))))) {
    driver.get("https://bonigarcia.dev/selenium-webdriver-java/");
    int width =
Integer.parseInt(driver.findElement(By.tagName("img"))
        .getAttribute("width"));
    assertThat(width).isGreaterThan(80);
}
```

+   1 We load a local image stored as a test resource in the Java project.
+   
+   2 We create a network interceptor instance, creating a route for all the requests ending in .png, and stubbing this request with a new response, in this case sending the content of the previous picture.
+   
+   3 We open the practice site.
+   
+   4 If the interception works as expected, the image on the page should have a width higher than the original logo.

> NOTE
> A DevToolsException will be thrown if a browser different than Chrome or Edge (such as Firefox) is used with previous code.

# Full Page Screenshot

Another possible usage of the CDP is making screenshots of a full page (i.e., capture the content page beyond the viewport). Example 5-37 demonstrate this feature in Chrome.

Example 5-37. Test making a full page screenshot using CDP in Chrome