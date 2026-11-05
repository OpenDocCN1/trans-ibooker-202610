| Property | Description |
|---|---|
| Request | Just the HTTP request. For example, headers, query string parameters, the body of the request as a stream that you can read from, the content type and length, and cookies. |
| Response | Just the HTTP response. For example, headers, the body of the response as a stream that you can write to, the content type and length, status code, and cookies. There are also delegates like OnStarting and OnCompleted that you can hook a method up to. |
| HttpContext | Everything about the current HTTP context including the request and response, information about the connection, a collection of features that have been enabled on the server with middleware, and a User object for authentication and authorization. |

# Understanding the Controller class

Microsoft provides another class named Controller that your classes can inherit from if they do need view support, as shown in the following code:

```
```csharp
namespace Microsoft.AspNetCore.Mvc
{
    //
    // Summary:
    // A base class for an MVC controller with view support.
    public abstract class Controller : ControllerBase,
        IActionFilter, IFilterMetadata, IAsyncActionFilter, IDisposable
    {
        ...
    }
```
```

Controller has many useful properties for working with views, as shown in the following table:

| Property | Description |
|---|---|
| ViewData | A dictionary that the controller can store key/value pairs in that is accessible in a view. The dictionary's lifetime is only for the current request/response. |
| ViewBag | A dynamic object that wraps the ViewData to provide a friendlier syntax for setting and getting dictionary values. |
| TempData | A dictionary that the controller can store key/value pairs in that is accessible in a view. The dictionary's lifetime is for the current request/response and the next request/response for the same visitor session. This is useful for storing a value during an initial request, responding with a redirect, and then reading the stored value in the subsequent request. |