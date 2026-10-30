
## Routing

Routing is how ASP.NET Core matches HTTP requests to controllers and actions (the executable *endpoints*) in your application, instead of the old Web Forms process of matching URLs to the project file structure. It also provides a mechanism for creating URLs from within the app based on those endpoints. An endpoint in an MVC- or Web API-style app consists of a controller, action (MVC only), HTTP verb, and optional values (called *route values*).

> Note Routes also apply to Razor pages, SignalR, gRPC services, and more. This book covers MVC- and Web API-style controllers.

ASP.NET Core uses routing middleware to match URLs of incoming requests and to generate URLs sent out in responses. The middleware is registered in the `Startup` class, and endpoints are added in the `Startup` class or through route attributes, both covered later in this chapter.

## URL Patterns and Route Tokens

Routing endpoints are composed of URL patterns that contain variable placeholders (called *tokens*) and of literals placed into an ordered collection known as the *route table*. Each entry defines a different URL pattern to match. Placeholders can be custom variables or from a predefined list. Table 29-6 lists the reserved routing names.

*Table 29-6 Reserved Route Tokens for MVC and API Applications*

| Token | Meaning in Life |
|---|---|
| Area | Defines the MVC area for the route |
| Controller | Defines the controller (minus the controller suffix) |
| Action | Defines the action name in MVC applications |

In addition to the reserved tokens, routes can contain custom tokens that are mapped (model bound) to action method parameters.

## Routing and ASP.NET Core RESTful Services

When defining routes for ASP.NET services, an action method is not specified. Instead, once the controller is located, the action method to execute is based on the HTTP verb of the request and HTTP verb assignments to action methods. More on this shortly.

## Conventional Routing

Conventional routing builds the route table in the `UseEndpoints()` method in the `Startup` class. The `MapControllerRoute()` method adds an endpoint into the route table. The method specifies a name, URL pattern, and any default values for the variables in the URL pattern. In the following code sample, the predefined `{controller}` and `{action}` placeholders refer to a controller and an action method contained in that controller. The placeholder {id} is custom and is translated into a parameter (named id) for the action method. Adding a question mark to a route token indicates that it is optional.

```
app.UseEndpoints(endpoints =>
{
    endpoints.MapControllerRoute(
        name: "default",
        template: "{controller=Home}/{action=Index}/{id?}");
});
```

When a URL is requested, it is checked against the route table. If there is a match, the code located at that application endpoint is executed. An example URL that would be serviced by this route is Car/Delete/5. This invokes the Delete() action method on the CarController, passing 5 to the id parameter.

The defaults specify how to fill in the blanks for URLs that don’t contain all of the defined components. In the previous code, if nothing was specified in the URL (such as http://localhost:5001), then the routing engine would call the Index() action method of the HomeController class, without an id parameter. The defaults are progressive, meaning that they can be excluded from right to left. However, route parts can’t be skipped. Entering a URL like http://localhost:60466/Delete/5 will fail the {controller}/{action}/{id} pattern.

The routing engine will attempt to find the first route based on the controller, action, custom tokens, and HTTP verb. If the routing engine can’t determine the best route, it will throw an AmbiguousMatchException.

Notice that the route template doesn’t contain a protocol or hostname. The routing engine automatically prepends the correct information when creating routes and uses the HTTP verb, path, and parameters to determine the correct application endpoint. For example, if your site is running on https://www.skimedic.com, the protocol (HTTPS) and hostname (www.skimedic.com) is automatically prepended to the route when created (e.g., https://www.skimedic.com/Car/Delete/5). For an incoming request, the routing engine uses the Car/Delete/5 portion of the URL.

## Named Routes

Route names can be used as a shorthand to generate URLs from within the application. In the preceding conventional route, the endpoint is assigned the name default.

## Attribute Routing

In attribute routing, routes are defined using C# attributes on controllers and their action methods. This can lead to more precise routing, but can also increase the amount of configuration, since every controller and action needs to have routing information specified.

For example, take the following code snippet. The four Route attributes on the Index() action method equate to the same route defined earlier. The Index() action method is the application endpoint for mysite.com, mysite.com/Home, mysite.com/Home/Index, or mysite.com/Home/Index/5.

```
public class HomeController : Controller
{
    [Route("/")]
    [Route("/Home")]
    [Route("/Home/Index")]
    [Route("/Home/Index/{id?}")]
    
    public IActionResult Index(int? id)
    {
        ...
    }
}
```

The major difference between conventional routing and attribute routing is that conventional routing covers the application, while attribute routing covers the controller with the Route attribute. If conventional routing is not used, every controller will need to have their route defined or they will not be able to be accessed. For example, if there wasn’t a default route defined in the route table, the following code is not discoverable since the controller doesn’t have any routing configured:

```
public class CarController : Controller
{
    public IActionResult Delete(int id)
    {
        ...
    }
}
```

Note Conventional and attribute routing can be used together. If the default controller route was set up on UseEndpoints() (as in the conventional routing example), the preceding controller would be located by the route table.

When routes are added at the controller level, the action methods derive from that base route. For example, the following controller route covers the Delete() (and any other) action method:

```
[Route("[controller]/[action]/{id?}")]
public class CarController : Controller
{
    public IActionResult Delete(int id)
    {
        ...
    }
}
```

Note The built-in tokens are distinguished with square brackets ([]) in attribute routing instead of the curly braces ({}) used in conventional routing. Custom tokens still use curly braces.

If an action method needs to restart the route pattern, prefix the route with a forward slash (/). For example, if the delete method should follow the URL pattern mysite.com/Delete/Car/5, configure the action as follows:

```
[Route("[controller]/[action]/{id?}")]
public class CarController : Controller
{
    [Route("/[action]/[controller]/{id}")]
    
    public IActionResult Delete(int id)
    {
        ...
    }
}
```

Routes can also hard-code the route values instead of using token replacement. The following code will produce the same result as the previous code sample:

```
[Route("[controller]/[action]/{id?}")]
public class CarController : Controller
{
    [Route("/Delete/Car/{id}")]
    public IActionResult Delete(int id)
    {
        ...
    }
}
```

## Named Routes

Routes can also be assigned a name. This creates a shorthand method for redirecting to a particular route just by using the name. For example, the following route attribute has the name of GetOrderDetails:

```
[HttpGet("{orderId}", Name = "GetOrderDetails")]
```

## Routing and HTTP Verbs

You might have noticed that neither of the route template definition methods defines an HTTP verb. This is because the routing engine (in MVC- and API-style applications) uses the route template and the HTTP verb in combination to select the proper application endpoint.

## HTTP Verbs in Web Application (MVC) Routing

Quite often when building web applications using the MVC pattern, there will be two application endpoints that match a particular route template. The discriminator in these instances is the HTTP verb. For example, if the CarController contains two action methods named Delete() and they both match the route template, the selection of which method to execute is based on the verb used in the request. The first Delete() method is decorated with the HttpGet attribute and will be executed when the incoming request is a get request. The second Delete() method is decorated with the HttpPost attribute and will be executed when the incoming request is a post.

```
[Route("[controller]/[action]/{id?}")]
public class CarController : Controller
{
    [HttpGet]
    public IActionResult Delete(int id)
    {
        ...
    }
}
```
