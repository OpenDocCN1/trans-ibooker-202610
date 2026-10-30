JavascriptExecutor as shown in the following snippet. Then, we can use the executor (using variable js in the example) to invoke the methods presented in the table before.

```
WebDriver driver = new ChromeDriver();
JavascriptExecutor js = (JavascriptExecutor) driver;
```

# Synchronous Scripts

The method executeScript() of a JavascriptExecutor object allows executing a piece of JavaScript in the context of the current web page in a WebDriver session. The invocation of this method (in Java) blocks the control flow until the script terminates. Therefore, we typically use this method for executing synchronous scripts in a webpage under test. The method executeScript() allows two arguments:

+   - String script: Mandatory JavaScript fragment to be executed. This code is executed in the body of the current page as an anonymous function (i.e., a JavaScript function without a name).
+   - Object... args: Optional arguments script. These arguments must be one of the following types: number, boolean, string, WebElement, or a List of these types (otherwise, WebDriver throws an exception). These arguments are available in the injected script using the arguments built-in JavaScript variable.

When the script returns some value (i.e., the code contains a return statement), the Selenium WebDriver method executeScript() method also returns a value in Java (otherwise, executeScript() returns null). The possible returned types are the following:

+   - WebElement: When returning an HTML element.
+   - Double: For decimals.
+   - Long: For non-decimal numbers.