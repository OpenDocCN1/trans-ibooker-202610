```
global using global::Microsoft.AspNetCore.Hosting;
global using global::Microsoft.AspNetCore.Http;
global using global::Microsoft.AspNetCore.Routing;
global using global::Microsoft.Extensions.Configuration;
global using global::Microsoft.Extensions.DependencyInjection;
global using global::Microsoft.Extensions.Hosting;
global using global::Microsoft.Extensions.Logging;
global using global::System;
global using global::System.Collections.Generic;
global using global::System.IO;
global using global::System.Linq;
global using global::System.Net.Http;
global using global::System.Net.Http.Json;
global using global::System.Threading;
global using global::System.Threading.Tasks;
```

7. Collapse the obj folder.

8. Open Program.cs, and note the following:

+   - An ASP.NET Core project is like a top-level console application, with a hidden Main method as its entry point that has an argument passed using the name args.
+   - It calls WebApplication.CreateBuilder, which creates a host for the website using defaults for a web host that is then built.
+   - The website will respond to all HTTP GET requests with plain text: Hello World!.
+   - The call to the Run method is a blocking call, so the hidden Main method does not return until the web server stops running, as shown in the following code:

```
var builder = WebApplication.CreateBuilder(args);
var app = builder.Build();

app.MapGet("/", () => "Hello World!");

app.Run();
```

9. At the bottom of the file, add a statement to write a message to the console after the call to the Run method and therefore after the web server has stopped, as shown highlighted in the following code:

```
app.Run();

Console.WriteLine("This executes after the web server has stopped!");
```