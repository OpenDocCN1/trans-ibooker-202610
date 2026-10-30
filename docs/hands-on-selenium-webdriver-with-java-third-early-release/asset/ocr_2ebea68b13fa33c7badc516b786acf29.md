Once we have implemented our listener, we need to create the decorator class. There are two ways to do that. If we want to decorate a WebDriver object, we can create an instance of EventFiringDecorator (passing the listener as the argument to the constructor) and then invoke the method decorate() to pass the WebDriver object. For instance:

```
WebDriver decoratedDriver = new EventFiringDecorator(myListener)
    .decorate(originalDriver);
```

The second way is to decorate other objects of the WebDriver API, namely: WebElement, TargetLocator, Navigation, Options, Timeouts, Window, Alert, or VirtualAuthenticator. In this case, we need to invoke the method createDecorated() in an EventFiringDecorator object to get a Decorated<T> generic class. The following snippet shows an example using a WebElement as a parameter:

```
Decorated<WebElement> decoratedWebElement = new
EventFiringDecorator(
    listener).createDecorated(myWebElement);
```

Let’s see a complete example. First, Example 4-29 shows the class that implements the WebDriverListener interface. Notice this class implements two methods: afterGet() and beforeQuit(). Both methods call to takeScreenshot() to take a browser screenshot. All in all, we are collecting browser screenshots just after loading a webpage (typically at the beginning of the test) and before quitting (typically at the end of the test). Then, Example 4-30 shows the test which uses this listener.

# Example 4-29. Event listener implementing methods afterGet() and beforeQuit()

```
public class MyEventListener implements WebDriverListener {
    static final Logger log = getLogger(lookup().lookupClass());
    @Override
    public void afterGet(WebDriver driver, String url) { ①
```