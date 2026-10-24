Middleware in this style takes in the next piece of middleware in the constructor and stores it in a private variable such that it can be called as needed in the Invoke method, which implements the middleware's behavior. To add this kind of middleware to the ASP.NET pipeline, you instantiate it and then use the Invoke method as a delegate:

```
app.Use(next => new ConsoleMiddleware(next).Invoke());
```

Or, we can use the more convenient extension method UseMiddleware, which essentially does the same thing by using reflection to find the Invoke method:

```
app.UseMiddleware<ConsoleMiddleware>()
```

Calling UseMiddleware is the preferred way to add class-based middleware. It's preferred because it's easier to read, but also because it allows us to use dependency injection: any dependencies in the middleware class's constructor, other than next, will be resolved from the service collection, just like dependencies in our MVC controllers. That opens the door to doing even more with middleware.

Now you know how to implement middleware as lambdas as well as classes. Next, let's look at testing middleware.

# 11.7 Testing middleware and pipelines

Testing ASP.NET middleware is straightforward: you create an HttpContext for the cases you want to test, and then you call the middleware, passing in that HttpContext.

> NOTE As detailed in chapter 8, you can create test projects from Visual Studio or using dotnet. These projects use xUnit, and that's also the test framework used here.

There's one small issue to overcome: in order to call middleware, you first need to provide it with a value for next. You get around that by passing in a RequestDelegate that does nothing:

```
RequestDelegate noOp = _ => Task.CompletedTask;
```

![](bbox=[0.66, 0.64, 0.84, 0.68])

You'll use no-operation middleware in tests as a stub to provide to middleware under test.

With this in place, you can write tests for your middleware. Suppose you want to test the following class-based middleware that handles a redirect from oldpath to newpath.

# Listing 11.9 An example piece of middleware

```
namespace Middleware
{
    public class RedirectingMiddleware
    {
        private readonly RequestDelegate next;
```