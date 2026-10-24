```
System.Console.WriteLine("Got request");
return next(ctx);
}
```

Let’s break down this code snippet. First, the type of next is RequestDelegate and is expected to be the next piece of middleware in the pipeline. Second, the type of ctx is HttpContext. That means that the inner lambda takes an HttpContext as argument and returns a Task:

```
ctx =>
{
    System.Console.WriteLine("Got request");
    return next(ctx);
}
```

The type of ctx is HttpContext.

next is a RequestDelegate, so it returns a Task.

Now that we’ve established that the inner lambda implements RequestDelegate, we can see how the outer lambda can be used to build up a pipeline. Calling the outer lambda with a piece of middleware creates a pipeline with two pieces of middleware: the middleware in the inner lambda first and the middleware passed in second:

```
RequestDelegate secondMiddleware = ...
var fisrstMiddleware = next =>
    ctx =>
    {
        System.Console.WriteLine("Got request");
        return next(ctx);
    }

RequestDelegate pipeline = firstMiddleware(secondMiddleware);
```

The middleware from the previous listings

A piece of middleware from somewhere

The pipeline also implements RequestDelegate.

The fact that the pipeline itself is also a RequestDelegate allows for building longer pipelines and for combining them. During startup ASP.NET will build the pipeline, so it is ready to process HTTP requests as they come in.

The HttpContext contains everything related to the HTTP request and response. On the request side, this includes all request headers, the request path, and the request body. Likewise, on the response side, the HttpContext includes response headers, status code, and the response body. With access to all that, middleware can do anything to the HTTP request and response.

The RequestDelegate is a simple type, but enables a powerful concept. The middleware pipeline is both flexible and powerful, and I find it very useful. Next, I will discuss when to use ASP.NET middleware and when to use features of MVC.

# 11.5.1 What belongs in middleware?

ASP.NET gives you a way to build a pipeline of middleware that each request flows through. The different parts of the pipeline can react to the request and write to the response. Likewise, MVC lets you handle requests and write responses. With MVC, you primarily handle requests in methods in controllers, but you can also add code that’s