# Example 5-44. Test using WebAuthn

```
```java
@Test
void testWebAuthn() {
    driver.get("https://webauthn.io/");
    HasVirtualAuthenticator virtualAuthenticator =
        (HasVirtualAuthenticator) driver;
    VirtualAuthenticator authenticator = virtualAuthenticator
        .addVirtualAuthenticator(new
        VirtualAuthenticatorOptions());

    String randomId = UUID.randomUUID().toString();
    driver.findElement(By.id("input-email")).sendKeys(randomId);
    WebDriverWait wait = new WebDriverWait(driver,
        Duration.ofSeconds(20));
    driver.findElement(By.id("register-button")).click();
    wait.until(ExpectedConditions.textToBePresentInElementLocated(
        By.className("popover-body"), "Success! Now try logging
in"));

    driver.findElement(By.id("login-button")).click();
    wait.until(ExpectedConditions.textToBePresentInElementLocated(
        By.className("main-content"), "You're logged in!"));

    virtualAuthenticator.removeVirtualAuthenticator(authenticator);
}
```
```

+   1. We open a website protected with the Web Authentication API.
+   2. We cast the driver object to `HasVirtualAuthenticator`.
+   3. We create and register a new virtual authenticator.
+   4. We send a random identifier in the web form.
+   5. We submit that identifier and wait until it is received.
+   6. We click the button to log in.
+   7. We verify the authentication has been correctly performed.
+   8. We remove the virtual authenticator.