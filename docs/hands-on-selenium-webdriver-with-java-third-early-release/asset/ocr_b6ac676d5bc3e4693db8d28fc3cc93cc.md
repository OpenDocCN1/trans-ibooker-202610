```
INFO [NodeOptions.discoveryDrivers] - Discovered 2 driver(s)
INFO [NodeOptions.report] - Adding Chrome for {"browserName": "chrome"} 8 times
INFO [NodeOptions.report] - Adding Firefox for {"browserName": "firefox"} 8 times
INFO [Node.<init>] - Binding additional locator mechanisms: name, id, relative
INFO [LocalDistributor.add] - Added node 41045bd8-ec7e-43c9-84bd-f63f7aca59ed
    at http://192.168.56.1:4444. Health check every 120s
INFO [GridModel.setAvailability] - Switching node 41045bd8-ec7e-43c9-84bd-f63f7aca59ed (uri: http://192.168.56.1:4444) from DOWN to UP
INFO [Standalone.execute] - Started Selenium Standalone 4.0.0 (revision 3a21814679): http://192.168.56.1:4444
```

+   1. We use WebDriverManager CLI to resolve chromedriver.
+   2. We use WebDriverManager CLI to resolve geckodriver (the driver required for Firefox).
+   3. We start Selenium Grid in standalone mode in the same folder (which contains chromedriver and geckodriver).

After these commands, the standalone Selenium Server listens to incoming HTTP requests in port 4444 of the localhost. Therefore, we can create an instance of RemoteWebDriver using that URL (e.g., `http://localhost:4444/` if the test executes in the same host) and the required capabilities (for Chrome or Firefox, in this case). For instance, as follows:

```
WebDriver driver = new RemoteWebDriver("http://localhost:4444/",
        new ChromeOptions());
```