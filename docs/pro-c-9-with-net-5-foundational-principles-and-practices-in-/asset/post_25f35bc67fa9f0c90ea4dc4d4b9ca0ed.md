
Now, update appsettings.development.json with the RebuildDataBase property (set the node to false for now).

```
{
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft": "Warning",
      "Microsoft.Hosting.Lifetime": "Information"
    }
  },
  "RebuildDataBase": false,
  "ConnectionStrings": {
    "AutoLot": "Server=db;Database=AutoLotPresentation;User ID=sa;Password=P@ssw0rd;"
  }
}
```

## AutoLot.Mvc

The Configure() method for web applications is a bit more involved than the API counterpart. The full method is listed here and discussed afterward:

```
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseExceptionHandler("/Home/Error");
        app.UseHsts();
    }
    app.UseHttpsRedirection();
    app.UseStaticFiles();
    app.UseRouting();
    app.UseAuthorization();
    app.UseEndpoints(endpoints =>
    {
        endpoints.MapControllerRoute(
            name: "default",
            pattern: "{controller=Home}/{action=Index}/{id?}");
    });
}
```

The method also checks for the environment and, if set to development, adds in the DeveloperExceptionPage middleware. If the environment is anything but development, the generic ExceptionHandler middleware is added to the pipeline as well as HTTP Strict Transport Security Protocol (HSTS).

Back in the main execution path, like its API counterpart, a call to app.UseHttpsRedirection() is added. The next step is adding support for static files with app.UseStaticFiles(). Support for static files is opt-in as a security measure. If your app doesn’t need them (like APIs), then don’t add support, and they can’t be a security risk. The routing, authorization, and end point middleware are added.

Add ApplicationDbContext as a parameter to the method, and call InitializeData() from AutoLot.Dal. The updated code is listed here:

```
public void Configure(
    IApplicationBuilder app,
    IWebHostEnvironment env,
    ApplicationDbContext context)
{
    if (env.IsDevelopment())
    {
        //If in development environment, display debug info
        app.UseDeveloperExceptionPage();
        //Initialize the database
        if (Configuration.GetValue<bool>("RebuildDataBase"))
        {
            SampleDataInitializer.InitializeData(context);
        }
    }
    ...
}
```

Now, update appsettings.development.json with the RebuildDataBase property (set the node to false for now).

```
{
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft": "Warning",
      "Microsoft.Hosting.Lifetime": "Information"
    }
  },
  "RebuildDataBase": false,
  "ConnectionStrings": {
    "AutoLot": "Server=db;Database=AutoLotPresentation;User ID=sa;Password=P@ssw0rd;"
  }
}
```

In the UseEndpoints() method, the default template sets up conventional routing. We are going to turn that off and use attribute routing throughout the entire application. Comment out (or delete) the call to MapControllerRoute() and replace that with MapControllers(), like this:

```
app.UseEndpoints(endpoints =>
{
    endpoints.MapControllers();
});
```

The next change is to add the route attributes to the HomeController in the AutoLot.Mvc application. First, add the controller/action pattern to the controller itself:

```
[Route("[controller]/[action]")]
public class HomeController : Controller
{
    ...
}
```

Next, add the three routes to the Index() method so that it is the default action when there isn’t an action specified or when there isn’t a controller or an action specified. Also, put the HttpGet attribute on the method to explicitly declare it as a “get” action:

```
[Route("/")]
[Route("/[controller]")]
[Route("/[controller]/[action]")]
[HttpGet]
public IActionResult Index()
{
    return View();
}
```

## Logging

As part of the startup and configuration process, basic logging is added into the dependency injection container. The ILogger<T> is the logging interface used by the logging infrastructure and is quite simple. The workhorse for logging is the LoggerExtensions class, with the method definitions listed here:

```
public static class LoggerExtensions
{
    public static void LogDebug(this ILogger logger, EventId eventId, Exception exception, string message, params object[] args)
    public static void LogDebug(this ILogger logger, EventId eventId, string message, params object[] args)
    public static void LogDebug(this ILogger logger, Exception exception, string message, params object[] args)
    public static void LogDebug(this ILogger logger, string message, params object[] args)

    public static void LogTrace(this ILogger logger, EventId eventId, Exception exception, string message, params object[] args)
    public static void LogTrace(this ILogger logger, EventId eventId, string message, params object[] args)
    public static void LogTrace(this ILogger logger, Exception exception, string message, params object[] args)
    public static void LogTrace(this ILogger logger, string message, params object[] args)
```

```
public static void LogInformation(this ILogger logger, EventId eventId,
    Exception exception, string message, params object[] args)
public static void LogInformation(this ILogger logger, EventId eventId, string message,
params object[] args)
public static void LogInformation(this ILogger logger, Exception exception, string
message, params object[] args)
public static void LogInformation(this ILogger logger, string message, params object[] args)

public static void LogWarning(this ILogger logger, EventId eventId,
    Exception exception, string message, params object[] args)
public static void LogWarning(this ILogger logger, EventId eventId, string message, params
object[] args)
public static void LogWarning(this ILogger logger, Exception exception, string message,
params object[] args)
public static void LogWarning(this ILogger logger, string message, params object[] args)

public static void LogError(this ILogger logger, EventId eventId,
    Exception exception, string message, params object[] args)
public static void LogError(this ILogger logger, EventId eventId, string message, params
object[] args)
public static void LogError(this ILogger logger, Exception exception, string message,
params object[] args)
public static void LogError(this ILogger logger, string message, params object[] args)

public static void LogCritical(this ILogger logger, EventId eventId,
    Exception exception, string message, params object[] args)
public static void LogCritical(this ILogger logger, EventId eventId, string message,
params object[] args)
public static void LogCritical(this ILogger logger, Exception exception, string message,
params object[] args)
public static void LogCritical(this ILogger logger, string message, params object[] args)

public static void Log(this ILogger logger, LogLevel logLevel, string message, params
object[] args)
public static void Log(this ILogger logger, LogLevel logLevel, EventId eventId, string
message, params object[] args)
public static void Log(this ILogger logger, LogLevel logLevel,
    Exception exception, string message, params object[] args)
public static void Log(this ILogger logger, LogLevel logLevel, EventId eventId,
    Exception exception, string message, params object[] args)
}
```

A powerful feature of ASP.NET Core is the extensibility of the pipeline as a whole and logging in particular. The default logger can be swapped out with another logging framework as long as the new framework can integrate with the logging pattern. Serilog is one framework that integrates with ASP.NET Core. The next sections cover creating a logging infrastructure based on Serilog and configuring the ASP.NET Core applications to use the new logging code.

## The IAppLogging Interface

Begin by adding new directory named Logging in the AutoLot.Service project. In this directory, add a new interface named IAppLogging<T>. Update the code in this interface to match the following:

```
using System;
using System.Runtime.CompilerServices;

namespace AutoLot.Services.Logging
{
    public interface IAppLogging<T>
    {
        void LogAppError(Exception exception, string message,
            [CallerMemberName] string memberName = "",
            [CallerFilePath] string sourceFilePath = "",
            [CallerLineNumber] int sourceLineNumber = 0);

        void LogAppError(string message,
            [CallerMemberName] string memberName = "",
            [CallerFilePath] string sourceFilePath = "",
            [CallerLineNumber] int sourceLineNumber = 0);

        void LogAppCritical(Exception exception, string message,
            [CallerMemberName] string memberName = "",
            [CallerFilePath] string sourceFilePath = "",
            [CallerLineNumber] int sourceLineNumber = 0);

        void LogAppCritical(string message,
            [CallerMemberName] string memberName = "",
            [CallerFilePath] string sourceFilePath = "",
            [CallerLineNumber] int sourceLineNumber = 0);

        void LogAppDebug(string message,
            [CallerMemberName] string memberName = "",
            [CallerFilePath] string sourceFilePath = "",
            [CallerLineNumber] int sourceLineNumber = 0);

        void LogAppTrace(string message,
            [CallerMemberName] string memberName = "",
            [CallerFilePath] string sourceFilePath = "",
            [CallerLineNumber] int sourceLineNumber = 0);

        void LogAppInformation(string message,
            [CallerMemberName] string memberName = "",
            [CallerFilePath] string sourceFilePath = "",
            [CallerLineNumber] int sourceLineNumber = 0);

        void LogAppWarning(string message,
            [CallerMemberName] string memberName = "",
            [CallerFilePath] string sourceFilePath = "",
            [CallerLineNumber] int sourceLineNumber = 0);
    }
}
```
