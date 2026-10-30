repository. This test is an example of asynchronous script execution. This script overrides the original `Notification` JavaScript object. The new implementation of this object gets the title of the notification message, which is returned in the script callback to the WebDriver test.

# Example 5-15. Test handling notifications

```
```java
@Test
void testNotifications() {
    driver.get(
        "https://bonigarcia.dev/selenium-webdriver-
        java/notifications.html");
    JavascriptExecutor js = (JavascriptExecutor) driver;

    String script = String.join("\n",
        "const callback = arguments[arguments.length - 1);",
        "const OldNotify = window.Notification;",
        "function newNotification(title, options) {",
        "    callback(title);",
        "    return new OldNotify(title, options);",
        "}",
        "newNotification.requestPermission = " +
        "    OldNotify.requestPermission.bind(OldNotify);",
        "Object.defineProperty(newNotification, 'permission',",
        "{",
        "    get: function() {",
        "        return OldNotify.permission;",
        "    }",
        "});",
        "window.Notification = newNotification;",
        "document.getElementById('notify-me').click();");
    log.debug("Executing the following script asynchronously:\n{}",
        script);

    Object notificationTitle = js.executeAsyncScript(script);
    assertThat(notificationTitle).isEqualTo("This is a
    notification");
}
```
```

+   1. As usual in asynchronous script execution, the last argument is the callback function used to signal the script termination.
+   2. We store a copy of the original `Notification` constructor.