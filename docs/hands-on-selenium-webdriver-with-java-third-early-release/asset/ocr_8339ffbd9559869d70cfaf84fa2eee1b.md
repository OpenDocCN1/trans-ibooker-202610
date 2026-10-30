setup, and maintenance) in an automated manner. Concerning Java, the reference implementation is **WebDriverManager**, a Selenium WebDriver helper library that allows driver management in an automated fashion. This section explains how to use WebDriverManager as a Java dependency.

Once the WebDriverManager dependency is resolved in our project (see *Appendix 3* for the configuration details), we can use the WebDriverManager API to manage drivers. This API provides a set of singletons (called *managers*) to download, set up, and maintain drivers. These singletons are accessible through the WebDriverManager class. For instance, we need to invoke the method *chromedriver()* to manage the driver required by Chrome, i.e., chromedriver, as follows:

```
```java
WebDriverManager.chromedriver().setup();
WebDriver driver = new ChromeDriver();
```
```

**Table 2-2** summarizes the basic WebDriverManager calls for all the supported browsers. In addition to these basic calls (i.e., the method `setup()`), WebDriverManager exposes a fluent API for advanced configuration. See *Appendix 2* for more details about the WebDriverManager methodology, configuration capabilities, and other usages, such as a Command Line Interface (CLI) tool (from the shell), as a server (using a REST-like (Representational State Transfer) API), as an agent (using Java instrumentation), or as a Docker container.