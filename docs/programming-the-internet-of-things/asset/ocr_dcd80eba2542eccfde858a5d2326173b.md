+   - Add the public startApp() method, and log an info message indicating the app was started. Include a try / catch block to handle the start code. On exception, log an error message along with the stack trace, then call stopApp(-1).
+   - Add the private initConfig(String fileName) method, and log an info message indicating the method was called. It will mostly remain empty for now.
+   - Add the private parseArgs(String[] args) method, and log an info message indicating the method was called. For now, the args can be ignored. Before the method exits, call initConfig(null).
+   - Update the constructor to include a call to parseArgs(args).
+   - Add the public static void main(String[] args) method to enable running as an application. It will create an instance of GatewayDeviceApp, call startApp(), wait 60 seconds, then call stopApp(0), as follows:

```
public static void main(String[] args)
{
    GatewayDeviceApp gwApp = new
GatewayDeviceApp(args);

    gwApp.startApp();

    try {
        Thread.sleep(60000L);
    } catch (InterruptedException e) {
        // ignore
    }

    gwApp.stopApp(0);
}
```