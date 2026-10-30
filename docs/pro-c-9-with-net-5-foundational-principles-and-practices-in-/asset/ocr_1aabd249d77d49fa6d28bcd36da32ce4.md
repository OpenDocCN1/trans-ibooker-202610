# Create the CustomExceptionFilter

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