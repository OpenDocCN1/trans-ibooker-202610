```
Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
Content root path: C:\Code\PracticalApps\Northwind.Web
```

Visual Studio will also start your chosen browser automatically. If you are using Visual Studio Code, you will have to start Chrome manually.

+   4. Leave the web server running.
+   5. In Chrome, show Developer Tools, and click the Network tab.
+   6. Enter the address http://localhost:5000/, or whatever port number was assigned to HTTP, and note the response is Hello World! in plain text, from the cross-platform Kestrel web server, as shown in Figure 14.6:

![](bbox=[0.175, 0.397, 0.935, 0.718])

Figure 14.6: Plain text response from http://localhost:5000/

Chrome also requests a favicon.ico file automatically to show in the browser tab but this is missing so it shows as a 404 Not Found error.