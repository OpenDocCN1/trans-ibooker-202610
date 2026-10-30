
## Create the CustomExceptionFilter

Create a new directory named Filters, and in that directory add a new class named CustomExceptionFilterAttribute.cs. Update the using statements to the following:

```
using Microsoft.AspNetCore.Hosting;
using Microsoft.AspNetCore.Mvc;
using Microsoft.AspNetCore.Mvc.Filters;
using Microsoft.EntityFrameworkCore;
using Microsoft.Extensions.Hosting;
```

Change the class to public and inherit from ExceptionFilterAttribute. Override the OnException() method, as shown here:

```
namespace AutoLot.Api.Filters
{
    public class CustomExceptionFilterAttribute : ExceptionFilterAttribute
    {
        public override void OnException(ExceptionContext context)
        {
        }
    }
}
```

Unlike most filters in ASP.NET Core that have a before and after event handler, exception filters have only one handler: OnException() (or OnExceptionAsync()). This handler has one parameter, ExceptionContext. This parameter provides access to the ActionContext as well as the exception that was thrown.

Filters also participate in dependency injection, allowing for any item in the container to be accessed in the code. In this example, we need an instance of IWebHostEnvironment injected into the filter. This will be used to determine the runtime environment. If the environment is Development, the response should also include the stack trace. Add a class-level variable to hold the instance of IWebHostEnvironment and add the constructor, as shown here:

```
private readonly IWebHostEnvironment _hostEnvironment;
public CustomExceptionFilterAttribute(IWebHostEnvironment hostEnvironment)
{
    _hostEnvironment = hostEnvironment;
}
```

The code in the OnException() event handler checks the type of exception thrown and builds an appropriate response. If the environment is Development, the stack trace is included in the response message. A dynamic object that contains the values to be sent to the calling request is built and returned in an IActionResult. The updated method is shown here:

```
public override void OnException(ExceptionContext context)
{
    var ex = context.Exception;
    string stackTrace = _hostEnvironment.IsDevelopment() ? context.Exception.StackTrace : string.Empty;
    string message = ex.Message;
    string error;
```

```
IActionResult actionResult;
switch (ex)
{
    case DbUpdateConcurrencyException ce:
        //Returns a 400
        error = "Concurrency Issue.";
        actionResult = new BadRequestObjectResult(
            new {Error = error, Message = message, StackTrace = stackTrace});
        break;
    default:
        error = "General Error.";
        actionResult = new ObjectResult(
            new {error = error, Message = message, StackTrace = stackTrace})
        {
            StatusCode = 500
        };
        break;
}
//context.ExceptionHandled = true; //If this is uncommented, the exception is swallowed
context.Result = actionResult;
}
```

If you want the exception filter to swallow the exception and set the response to a 200 (e.g., to log the error but not return it to the client), add the following line before setting the Result (commented out in the preceding example):

```
context.ExceptionHandled = true;
```

## Add Filters to the Processing Pipeline

Filters can be applied to action methods, controllers, or globally to the application. The before code of filters executes from the outside in (global, controller, action method), while the after code of filters executes from the inside out (action method, controller, global).

Adding filters at the application level is accomplished in the ConfigureServices() method of the Startup class. Open the Startup.cs class and add the following using statement to the top of the file:

```
using AutoLot.Api.Filters;
```

Update the AddControllers() method to add the custom filter.

```
services
    .AddControllers(config => config.Filters.Add(new CustomExceptionFilterAttribute(_env)))
    .AddJsonOptions(options =>
    {
        options.JsonSerializerOptions.PropertyNamingPolicy = null;
        options.JsonSerializerOptions.WriteIndented = true;
    })
    .ConfigureApiBehaviorOptions(options =>
    {
    ...
    });
```

## Test the Exception Filter

To test the exception filter, open the WeatherForecastController.cs file, and update the Get() action to the code shown here:

```
[HttpGet]
public IEnumerable<WeatherForecast> Get()
{
    _logger.LogAppWarning("This is a test");
    throw new Exception("Test Exception");
    ...
}
```

Run the application and exercise the method using Swagger. The results shown in the Swagger UI should match the following output (the stack trace has been shortened for brevity):

```
{
  "Error": "General Error.",
  "Message": "Test Exception",
  "StackTrace": "   at AutoLot.Api.Controllers.WeatherForecastController.Get() in D:\\Projects\\Books\\csharp9-wf\\Code\\New\\Chapter_30\\AutoLot.Api\\Controllers\\WeatherForecastController.cs:line 31\r\n   "
}
```

## Add Cross-Origin Requests Support

APIs should have policies in place that allow or prevent clients that originate from another server to communicate with the API. These types of requests are called cross-origin requests (CORS). While this isn’t needed when you are running locally on your machine in an all ASP.NET Core world, it is needed by JavaScript frameworks that want to communicate with your API, even when all are running locally.

Note  For more information on CORS support, refer to the document article at https://docs.microsoft.com/en-us/aspnet/core/security/cors.

## Create a CORS Policy

ASP.NET Core has rich support for configuring cores, including methods to allow/disallow headers, methods, origins, credentials, and more. In this example, we are going to leave everything as wide open as possible. Configuration starts by creating a CORS policy and adding the policy into the services collection. The policy is named (this name will be used in the Configure() method) followed by the rules. The following example creates a policy named AllowAll and then does just that. Add the following code to the ConfigureServices() method in the Startup.cs class:

```
services.AddCors(options =>
{
    options.AddPolicy("AllowAll", builder =>
    {

```
```
builder
    .AllowAnyHeader()
    .AllowAnyMethod()
    .AllowAnyOrigin();
});
});
```

## Add the CORS Policy to the HTTP Pipeline Handling

The final step is to add the CORS policy into the HTTP pipeline handling. Add the following line into the Configure() method of Startup.cs, making sure it is between the app.UseRouting() and app.UseEndpoints() method calls:

```
public void Configure(
    IApplicationBuilder app,
    IWebHostEnvironment env,
    ApplicationDbContext context)
{
    ...
    //opt-in to routing
    app.UseRouting();
    //Add CORS Policy
    app.UseCors("AllowAll");
    //enable authorization checks
    app.UseAuthorization();
    ...
}
```

## Summary

This chapter continued our study of ASP.NET Core. We first learned about returning JSON from action methods, and then we looked at the ApiController attribute and the effect it has on API controllers. Next, the general Swashbuckle implementation was updated to include the application’s XML documentation and information from action method attributes.

Next, the base controller was built, which holds most of the functionality of the application. After that, the derived, entity-specific controllers were added into the project. The final two steps added in an application-wide exception filter and support for cross-origin requests.

In the next chapter, you will finish building the ASP.NET Core Web Application, AutoLot.Mvc.

## CHAPTER 31

## MVC Applications
with ASP.NET Core

Chapter 29 laid the foundation for ASP.NET Core, and in Chapter 30, we built the RESTful service. In this chapter, we will build the web application using the MVC pattern. We begin by putting the V back into MVC.

> Note The sample code for this chapter is in the Chapter 31 directory of this book's repo. Feel free to continue working on the solution you started in Chapter 29 and updated in Chapter 30.

## Introducing the “V” in ASP.NET Core

When building ASP.NET Core services, only the M (models) and the C (controllers) of the MVC pattern are used. The user interface is created using the V, or the views of the MVC pattern. Views are built using HTML, JavaScript, CSS, and Razor code. They optionally have a base layout page and are rendered from a controller action method or a view component. If you have worked in classic ASP.NET MVC, this should sound familiar.

## ViewResults and Action Methods

As mentioned briefly in Chapter 29, ViewResults and PartialView results are ActionResults that are returned from an action method using the Controller helper methods. A PartialViewResult is designed to be rendered inside another view and doesn’t use a layout page, while a ViewResult is typically rendered in conjunction with a layout page.

The convention in ASP.NET Core (as it was in ASP.NET MVC) is for the View or PartialView to render a *.cshtml file of the same name as the method. The view must be located either in a folder named for the controller (less the controller suffix) or in the Shared folder (both located under the parent Views folder). For example, the following code will render the SampleAction.cshtml view located in the Views\Sample or Views\Shared folder:

```
[Route("[controller]/[action]")]
public class SampleController: Controller
{
    public ActionResult SampleAction()
```
