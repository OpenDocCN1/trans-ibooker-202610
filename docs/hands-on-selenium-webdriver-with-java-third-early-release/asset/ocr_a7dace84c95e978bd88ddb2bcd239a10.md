```
options.setProxy(proxy); // ⑥

driver =
WebDriverManager.chromedriver().capabilities(options).create();
}
```

+   ❶ We create an instance of the class `Proxy`.
+   
+   ❷ The syntax required to specify a proxy is `host:port`.
+   
+   ❸ We specify the proxy is used for HTTP connections.
+   
+   ❹ We also specify the proxy is used for HTTPS connections.
+   
+   ❺ Although not mandatory, it is typically required to accept insecure certificates.
+   
+   ❻ We set the proxy as a capability. This line is equivalent to `options.setCapability(CapabilityType.PROXY, proxy);`.

> **TIP**
> 
> The section “Capture Network Traffic” in *Chapter 9* shows how to use a third-party library to capture network traffic by using a web proxy in a Selenium WebDriver test.

# ## Log Gathering

The WebDriver API allows gathering different log sources. This feature is enabled using capabilities, although it is only supported in Chromium-based browsers at the time of this writing. **Example 5-47** presents a test setup that enables gathering browser logs (i.e., console messages). This snippet also contains the test logic, in which we need to invoke `driver.manage().logs()` to gather the log list.

# *Example 5-47. Test gathering browser log using Chrome*