It will output which endpoint was chosen, as well as handling one specific route: /bonjour. If that route is matched, it will respond with plain text, without calling any further into the pipeline:

+   1. In Startup.cs, statically import Console, as shown in the following code:
+   2. Add statements after the call to UseRouting and before the call to UseHttpsRedirection to use an anonymous method as a middleware delegate, as shown in the following code:
+   3. Start the website.
+   4. In Chrome, navigate to https://localhost:5001/, look at the console output and note that there was a match on an endpoint route /, it was processed as /index, and the Index.cshtml Razor Page was executed to return the response, as shown in the following output:
+   5. Navigate to https://localhost:5001/suppliers and note that you can see that there was a match on an endpoint route /Suppliers and the Suppliers.cshtml Razor Page was executed to return the response, as shown in the following output:

```
using static System.Console;
```

```
app.Use(async (HttpContext context, Func<Task> next) =>
{
    RouteEndpoint? rep = context.GetEndpoint() as RouteEndpoint;
    if (rep is not null)
    {
        WriteLine($"Endpoint name: {rep.DisplayName}");
        WriteLine($"Endpoint route pattern: {rep.RoutePattern.RawText}");
    }

    if (context.Request.Path == "/bonjour")
    {
        // in the case of a match on URL path, this becomes a terminating
        // delegate that returns so does not call the next delegate
        await context.Response.WriteAsync("Bonjour Monde!");
        return;
    }

    // we could modify the request before calling the next delegate
    await next();
    // we could modify the response after calling the next delegate
});
```

```
Endpoint name: /index
Endpoint route pattern:
```

```
Endpoint name: /Suppliers
Endpoint route pattern: Suppliers
```