
```java
@Test
void testFrames() {
    driver.get(
        "https://bonigarcia.dev/selenium-webdriver-java/frames.html"); ①

    WebDriverWait wait = new WebDriverWait(driver,
    Duration.ofSeconds(10));
    String frameName = "frame-body";
    wait.until(ExpectedConditions
        .presenceOfElementLocated(By.name(frameName))); ②
    driver.switchTo().frame(frameName); ③

    By pName = By.tagName("p");

    wait.until(ExpectedConditions.numberOfElementsToBeMoreThan(pName,
        0));
    List<WebElement> paragraphs = driver.findElements(pName);
    assertThat(paragraphs).hasSize(20);
}
```

① We open a web page that contains a frameset (see Figure 4-9).

② We wait for the frame to be available. Note that steps 2 and 3 in Example 4-22 are equivalent to this step.

③ We change the focus to this frame.

## Hands-on Selenium WebDriver with Java

Practice site

![](img/b67ec8e49249e340add6ed66ab413d78_231_0.png)

## Frames

Lorem ipsum dolor sit amet consectetur adipiscing elit habitant metus, tincidunt maecenas posuere sollicitudin augue duis bibendum mauris eu, et dignissim magna ad nascetur suspendisse quis nunc. Fames est ligula molestie aliquam pretium bibendum nullam, sociosqu maecenas mus etiam consequat ornare leo, sem mattis varius luctus litora senectus. Parturient quis tristique erat natoque tortor nascetur, primis augue vivamus habitasse senectus porta leo, aenean potenti ante a nam.

Nibh netus aliquet nam mattis vestibulum interdum euismod suspendisse rutrum ullamcorper venenatis, vehicula curabitur viverra habitasse leo vulputate eget malesuada ad tincidunt, donec posuere bibendum varius sollicitudin aenean accumsan lacinia consequat mi. Litora vel turpis luctus nec quisque hendrerit morbi maecenas, netus conubia feugiat orci fringilla suspendisse iaculis erat, semper posuere integer et senectus justo curabitur. Fringilla ut taciti mollis morbi blandit dictumst odio ultrices, parturient feugiat vel porttitor convallis eros pellentesque pretium risus, varius accumsan litora habitasse cursus pharetra commodo.

Varius bibendum volutpat porttitor habitant quis quam vehicula cras, facilisi natoque ornare viverra vestibulum aliquet aliquam. Libero class porttitor hac iaculis mauris ligula mattis turpis, tincidunt leo vivamus velit massa praesent ante, torquent eleifend ullamcorper scelerisque nec dictum imperdiet. Tincidunt purus nostra felis fusce varius at pellentesque, sociosqu accumsan phasellus interdum posuere eros, pharetra velit diam quisque porttitor scelerisque.

## Dialog Boxes

JavaScript provides different dialog boxes (sometimes called *popups*) to interact with the user, namely:

- **Alert.** To show a message and wait for the user to press the button *ok* (only choice in the dialog). For instance, the following code will open a dialog that displays “Hello world!” and wait for the user to press the ok button.

```javascript
alert("Hello world!");
```

- **Confirm.** To show a dialog box with a question and two buttons: *ok* and *cancel*. For instance, the following code will open a dialog showing the message “Is this correct?” and prompting the user to click on *ok* or *cancel*.

```javascript
let correct = confirm("Is this correct?");
```

- **Prompt.** To show a dialog box with a text message, an input text field, and the buttons *ok* and *cancel*. For example, the following code shows a popup displaying “Please enter your name”, a dialog box in which the user can type, and two buttons (*ok* and *cancel*).

```javascript
let username = prompt("Please enter your name");
```

In addition, CSS allows implementing another type of dialog box called *modal window*. This dialog disables the main window (but keeps it visible) while overlaying a child popup, typically showing a message and some buttons. You can find a sample page on the practice web containing all these dialog boxes (alert, confirm, prompt, and modal). Figure 4-10 shows a screenshot of this page when the modal dialog is active.

## Hands-on Selenium WebDriver with Java

Practice site

## Dialog boxes

Launch alert

Launch confirm

Launch prompt

Launch modal

### Modal title

This is the modal body

Close

Save changes

Figure 4-10. Practice web page with dialog boxes (alert, confirm, prompt, and modal)

## Alerts, Confirms, and Prompts

The WebDriver API provides the interface `Alert` to manipulate JavaScript dialogs (i.e., alerts, confirms, and prompts). **Table 4-9** describes the methods provided by this interface. Then, **Example 4-24** shows a basic test interacting with an alert.

## Example 4-24. Test handling an alert dialog

```java
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

- ① We open the practice web page that launches dialog boxes.
- ② We click on the left button to launch a JavaScript alert.
- ③ We wait until the alert dialog is displayed on the screen.
- ④ We change the focus to the alert popup.
- ⑤ We verify that the alert text is as expected.
- ⑥ We click on the *ok* button of the alert dialog.

We can replace steps 3 and 4 with a single explicit wait statement, as follows (you can find it in a second test in the same class in the examples repository):

```java
Alert alert = wait.until(ExpectedConditions.alertIsPresent());
```

The next test (Example 4-25) illustrates how to deal with a confirm dialog. Notice this example is quite similar to the previous one, but in this case, we can invoke the method `dismiss()` to click on the *cancel* button available on the confirm dialog. Finally, Example 4-26 shows how to manage a prompt dialog. In this case, we can type some string into the input text.

```java
LocalStorage localStorage = webStorage.getLocalStorage();
log.debug("Local storage elements: {}", localStorage.size());

SessionStorage sessionStorage = webStorage.getSessionStorage();
sessionStorage.keySet()
    .forEach(key -> log.debug("Session storage: {}={}",
        key,
        sessionStorage.getItem(key)));
assertThat(sessionStorage.size()).isEqualTo(2);

sessionStorage.setItem("new element", "new value");
assertThat(sessionStorage.size()).isEqualTo(3);

driver.findElement(By.id("display-session")).click();
}
```

① We cast the driver object to `WebStorage`.

② We log the number of elements of the local storage.

③ We log the session storage (it should contain two elements).

④ After adding a new element, it should be three elements in the session storage.

## Event Listeners

The WebDriver API allows creating listeners that notify events happening in WebDriver and derived objects. In former versions of Selenium WebDriver, this feature was accessible through the class `EventFiringWebDriver`. This class is deprecated as of Selenium WebDriver 4, and instead, we should use the following:

- **EventFiringDecorator:** Wrapper class for WebDriver and derived objects (e.g., WebElement, TargetLocator, etc.). It allows registering one or more listeners (i.e., `WebDriverListener` instances).
- **`WebDriverListener`:** Interface that should be implemented by the listeners registered in the decorator. It supports three types of events:
  - *Before* events: logic inserted just before some event starts.
  - *After* events: logic inserted just after some event terminates.
  - *Error* events: logic inserted before an exception is thrown.

To implement an event listener, first, we should create a listener class. In other words, we need to create a class that implements `WebDriverListener`. This interface defines all its methods using the `default` keyword, and therefore, it is optional to override them. Thanks to that feature (available as of Java 8), our class should only implement the methods we need. There are plenty of listener methods available, for instance, `afterGet()` (executed *after* calling the method `get()` in a `WebDriver` instance), or `beforeQuit()` (executed *before* calling the `quit()` method in a `WebDriver` instance), to name a few. My recommendation for checking all these listeners is to use your favorite IDE to discover the possible methods to be overridden or implemented. Figure 4-11 shows the wizard to do this in Eclipse.

![](img/b67ec8e49249e340add6ed66ab413d78_240_0.png)

*Figure 4-11. WebDriverListener methods in Eclipse*
