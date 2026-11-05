
It is on by default with ASP.NET Core 2.2 or later if compatibility is set to 2.2 or later. Traditional routes registered using the MapRoute method or with attributes are mapped to the new system.

The new routing system includes a link generation service registered as a dependency service that does not need an HttpContext.

## Configuring endpoint routing

Endpoint routing requires a pair of calls to the UseRouting and UseEndpoints methods:

- UseRouting marks the pipeline position where a routing decision is made.
- UseEndpoints marks the pipeline position where the selected endpoint is executed.

Middleware such as localization that runs in between these methods can see the selected endpoint and can switch to a different endpoint if necessary.

Endpoint routing uses the same route template syntax that has been used in ASP.NET MVC since 2010 and the [Route] attribute introduced with ASP.NET MVC 5 in 2013. Migration often only requires changes to the Startup configuration.

MVC controllers, Razor Pages, and frameworks such as SignalR used to be enabled by a call to UseMvc or similar methods, but they are now added inside the UseEndpoints method call because they are all integrated into the same routing system along with middleware.

## Reviewing the endpoint routing configuration in our project

Review the Startup.cs class file, as shown in the following code:

```csharp
using Packt.Shared; // AddNorthwindContext extension method

namespace Northwind.Web;

public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddRazorPages();

        services.AddNorthwindContext();
    }

    public void Configure(
        IApplicationBuilder app, IWebHostEnvironment env)
    {
        if (!env.IsDevelopment())
        {
            app.UseHsts();
        }

        app.UseRouting();

        app.UseHttpsRedirection();

        app.UseDefaultFiles(); // index.html, default.html, and so on
        app.UseStaticFiles();

        app.UseEndpoints(endpoints =>
        {
            endpoints.MapRazorPages();

            endpoints.MapGet("/hello", () => "Hello World!");
        });
    }
}
```

The Startup class has two methods that are called automatically by the host to configure the website.

The ConfigureServices method registers services that can then be retrieved when the functionality they provide is needed using dependency injection. Our code registers two services: Razor Pages and an EF Core database context.

## Registering services in the ConfigureServices method

Common methods that register dependency services, including services that combine other method calls that register services, are shown in the following table:

| Method | Services that it registers |
|---|---|
| AddMvcCore | Minimum set of services necessary to route requests and invoke controllers. Most websites will need more configuration than this. |
| AddAuthorization | Authentication and authorization services. |
| AddDataAnnotations | MVC data annotations service. |
| AddCacheTagHelper | MVC cache tag helper service. |
| AddRazorPages | Razor Pages service including the Razor view engine. Commonly used in simple website projects. It calls the following additional methods: AddMvcCore AddAuthorization AddDataAnnotations AddCacheTagHelper |
| AddApiExplorer | Web API explorer service. |
| AddCors | CORS support for enhanced security. |
| AddFormatterMappings | Mappings between a URL format and its corresponding media type. |
| AddControllers | Controller services but not services for views or pages. Commonly used in ASP.NET Core Web API projects. It calls the following additional methods: AddMvcCore AddAuthorization AddDataAnnotations AddCacheTagHelper AddApiExplorer AddCors AddFormatterMappings |
| AddViews | Support for .cshtml views including default conventions. |
| AddRazorViewEngine | Support for Razor view engine including processing the @ symbol. |
| AddControllersWithViews | Controller, views, and pages services. Commonly used in ASP.NET Core MVC website projects. It calls the following additional methods: AddMvcCore AddAuthorization AddDataAnnotations AddCacheTagHelper AddApiExplorer AddCors AddFormatterMappings AddViews AddRazorViewEngine |
| AddMvc | Similar to AddControllersWithViews, but you should only use it for backward compatibility. |
| AddDbContext<T> | Your DbContext type and its optional DbContextOptions<TContext>. |
| AddNorthwindContext | A custom extension method we created to make it easier to register the NorthwindContext class for either SQLite or SQL Server based on the project referenced. |

You will see more examples of using these extension methods for registering services in the next few chapters when working with MVC and Web API services.

## Setting up the HTTP request pipeline in the Configure method

The Configure method configures the HTTP request pipeline, which is made up of a connected sequence of delegates that can perform processing and then decide to either return a response themselves or pass processing on to the next delegate in the pipeline. Responses that come back can also be manipulated.

Remember that delegates define a method signature that a delegate implementation can plug into. The delegate for the HTTP request pipeline is simple, as shown in the following code:

```
public delegate Task RequestDelegate(HttpContext context);
```

You can see that the input parameter is an HttpContext. This provides access to everything you might need to process the incoming HTTP request, including the URL path, query string parameters, cookies, and user agent.

These delegates are often called middleware because they sit in between the browser client and the website or service.

Middleware delegates are configured using one of the following methods or a custom method that calls them itself:

- Run: Adds a middleware delegate that terminates the pipeline by immediately returning a response instead of calling the next middleware delegate.
- Map: Adds a middleware delegate that creates a branch in the pipeline when there is a matching request usually based on a URL path like /hello.
- Use: Adds a middleware delegate that forms part of the pipeline so it can decide if it wants to pass the request to the next delegate in the pipeline and it can modify the request and response before and after the next delegate.

For convenience, there are many extension methods that make it easier to build the pipeline, for example, UseMiddleware<T>, where T is a class that has:

- 1. A constructor with a RequestDelegate parameter that will be passed the next pipeline component
- 2. An Invoke method with a HttpContext parameter and returns a Task

## Summarizing key middleware extension methods

Key middleware extension methods used in our code include the following:

- UseDeveloperExceptionPage: Captures synchronous and asynchronous System.Exception instances from the pipeline and generates HTML error responses.
- UseHsts: Adds middleware for using HSTS, which adds the Strict-Transport-Security header.
- UseRouting: Adds middleware that defines a point in the pipeline where routing decisions are made and must be combined with a call to UseEndpoints where the processing is then executed. This means that for our code, any URL paths that match / or /index or /suppliers will be mapped to Razor Pages and a match on /hello will be mapped to the anonymous delegate. Any other URL paths will be passed on to the next delegate for matching, for example, static files. This is why, although it looks like the mapping for Razor Pages and /hello happen after static files in the pipeline, they actually take priority because the call to UseRouting happens before UseStaticFiles.
- UseHttpsRedirection: Adds middleware for redirecting HTTP requests to HTTPS, so in our code a request for http://localhost:5000 would be modified to https://localhost:5001.
- UseDefaultFiles: Adds middleware that enables default file mapping on the current path, so in our code it would identify files such as index.html.
- UseStaticFiles: Adds middleware that looks in wwwroot for static files to return in the HTTP response.
