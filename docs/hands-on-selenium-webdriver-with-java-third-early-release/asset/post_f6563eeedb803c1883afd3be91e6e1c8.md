
## Example 5-39. Test setting custom geolocation coordinates through LocationContext

```java
@Test
void testLocationContext() {
    LocationContext location = (LocationContext) driver; ①
    location.setLocation(new Location(27.5916, 86.5640, 8850)); ②

    driver.get(
        "https://bonigarcia.dev/selenium-webdriver-java/geolocation.html"); ③
    driver.findElement(By.id("get-coordinates")).click();

    WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(5));
    WebElement coordinates =
        driver.findElement(By.id("coordinates"));
    wait.until(ExpectedConditions.visibilityOf(coordinates)); ④
}
```

- ① We cast the driver object to LocationContext (only possible for Chrome, Edge, or Opera).
- ② We open a practice page in which the geolocation coordinates are displayed to the end-user.
- ③ We set a custom location, in this case, the coordinates of Mount Everest (on the Nepal-China border).
- ④ We assert the coordinates are visible on the page.

## WebDriver BiDi

The WebDriver BiDi is a W3C recommendation that defines the bidirectional WebDriver protocol. Instead of the strict command/response format of the WebDriver protocol, BiDi introduces a WebSocket connection between driver and browser to enable bidirectional communication.

Thanks to BiDi, the WebDriver API allows implementing listener events without polling the browser to get responses. BiDi is available only in Chromium-based browsers through the interface `HasLogEvents` at the time of this writing. This interface allows implementing listeners for the following events:

- `domMutation`: To capture events about changes in the DOM. **Example 5-40** shows a test implementing a listener for these events.
- `consoleEvent`: To capture events about changes in the browser console, such as JavaScript traces. **Example 5-41** shows a second test implementing this type of listener.

## Example 5-40. Test implementing a listener for DOM mutation events

```java
@Test
void testDomMutation() throws InterruptedException {
    driver.get("https://bonigarcia.dev/selenium-webdriver-java/");

    HasLogEvents logger = (HasLogEvents) driver;
    JavascriptExecutor js = (JavascriptExecutor) driver;

    AtomicReference<DomMutationEvent> seen = new AtomicReference<>();
    CountDownLatch latch = new CountDownLatch(1);
    logger.onLogEvent(CdpEventTypes.domMutation(mutation -> {
        seen.set(mutation);
        latch.countDown();
    }));

    WebElement img = driver.findElement(By.tagName("img"));
    String newSrc = "img/award.png";
    String script = String.format("arguments[0].src = '%s';", newSrc);
    js.executeScript(script, img);

    assertThat(latch.await(10, TimeUnit.SECONDS)).isTrue();
    assertThat(seen.get().getElement().getAttribute("src"))
        .endsWith(newSrc);
}
```

❶ We cast the driver object to `HasLogEvents`. This cast is only possible for Chrome and Edge.

HTTP provides two built-in mechanisms for recognizing users’ identity, called *basic* and *digest* authentication. Both methods allow specifying the user’s credentials using a pair of values: username and password. The difference between them is how they communicate the credentials. On the one hand, the digest authentication method sends encrypted credentials by applying a hash function to the username and password. On the other hand, basic authentication uses Base64 to encode (not encrypt) the credentials.

Selenium WebDriver provides the interface `HasAuthentication` to implement basic and digest authentication in a seamless manner. This interface is inherited by `ChromiumDriver`. Therefore, we can use it only with Chrome and Edge. **Example 5-42** shows a test using Chrome and basic authentication. You can use the exact mechanism with Edge and digest authentication (see the complete test in the example repository).

## Example 5-42. Test using basic authentication with Chrome

```java
@Test
void testBasicAuth() {
    ((HasAuthentication) driver)
        .register(() -> new UsernameAndPassword("guest", "guest"));

    driver.get("https://jigsaw.w3.org/HTTP/Basic/");

    WebElement body = driver.findElement(By.tagName("body"));
    assertThat(body.getText()).contains("Your browser made it!");
}
```

- 1. We cast the driver object to `HasAuthentication` and register the credentials (username and password).
- 2. We open a website protected with basic authentication.
- 3. We verify the page content is available.

When using other browsers (such as Firefox), we cannot cast the driver object to `HasAuthentication`. Nevertheless, there is a general way to send the credentials in the URL using the syntax

`protocol://username:password@domain`.

**Example 5-43** demonstrates this use.

## Example 5-43. Test using basic authentication and Firefox

```java
@Test
void testGenericAuth() {
    driver.get("https://guest:guest@jigsaw.w3.org/HTTP/Basic/");

    WebElement body = driver.findElement(By.tagName("body"));
    assertThat(body.getText()).contains("Your browser made it!");
}
```

## Web Authentication

The Web Authentication API (also known as *WebAuthn*) is a **W3C specification** that allows servers to register and authenticate users using public-key cryptography instead of passwords. Major browsers (Chrome, Firefox, Edge, and Safari) support WebAuthn as of January 2019. These browsers allow credential creation and assertion using U2F (Universal 2nd Factor) tokens, which are Universal Serial Bus (USB) or Near-Field Communication (NFC) secure devices.

In the classical web authentication approach, users send their username and password to the server using a web form. In WebAuthn, the web server uses the Web Authentication API to prompt the user to create a private-public key pair (known as a *credential*). The private key is stored securely on the user’s device, and the public key is sent to the server. Then, the server can use that public key to validate the user identity.

As of version 4, Selenium WebDriver supports *WebAuthn* out of the box. To that aim, the WebDriver API provides the interface `HasVirtualAuthenticator`. Instead of using secure physical devices, this interface allows us to use virtual authenticators. Although the `RemoteWebDriver` class implements this interface, at the time of this writing, this mechanism is only supported in Chromium-based browsers, i.e., Chrome and Edge. **Example 5-44** shows a test using the Web Authentication API.

## Example 5-44. Test using WebAuthn

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
        By.className("popover-body"), "Success! Now try logging in"));

    driver.findElement(By.id("login-button")).click();
    wait.until(ExpectedConditions.textToBePresentInElementLocated(
        By.className("main-content"), "You're logged in!"));

    virtualAuthenticator.removeVirtualAuthenticator(authenticator);
}
```

- 1. We open a website protected with the Web Authentication API.
- 2. We cast the driver object to `HasVirtualAuthenticator`.
- 3. We create and register a new virtual authenticator.
- 4. We send a random identifier in the web form.
- 5. We submit that identifier and wait until it is received.
- 6. We click the button to log in.
- 7. We verify the authentication has been correctly performed.
- 8. We remove the virtual authenticator.
