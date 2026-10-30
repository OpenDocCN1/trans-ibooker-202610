```
[HttpPost]
public IActionResult Delete(int id, Car recordToDelete)
{
    ...
}
```

Routes can also be modified using the HTTP verbs attributes instead of the Route attribute. For example, the following shows the optional id route token added to the route template for both Delete() methods:

```
[Route("[controller]/[action]")]
public class CarController : Controller
{
    [HttpGet("{id?}")]
    public IActionResult Delete(int? id)
    {
        ...
    }
    [HttpPost("{id}")]
    public IActionResult Delete(int id, Car recordToDelete)
    {
        ...
    }
}
```

Routes can also be restarted using the HTTP verbs; just preface the route templated with a forward slash (/), as the following example demonstrates:

```
[HttpGet("/[controller]/[action]/{makeId}/{makeName}")]
public IActionResult ByMake(int makeId, string makeName)
{
    ViewBag.MakeName = makeName;
    return View(_repo.GetAllBy(makeId));
}
```

> Note If an action method isn't decorated with an HTTP verb attribute, it defaults to a get method. However, in MVC web applications, unmarked action methods can also respond to post requests. For this reason, it's considered a best practice to mark all action methods explicitly with the correct verb attribute.

# API Service Routing

A significant difference between route definitions used for MVC-style applications and the route definitions used for RESTful services is that service route definitions do not specify action methods. The action methods are selected based on the HTTP verb of the request (and optionally the content type) and not by name. The following code shows an API controller with four methods that all match the same route template. Notice the HTTP verb attributes: