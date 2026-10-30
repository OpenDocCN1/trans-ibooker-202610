```
[Route("api/[controller]")]
[ApiController]
public class CarController : ControllerBase
{
    [HttpGet("{id}")]
    public IActionResult GetCarsById(int id)
    {
        ...
    }
    [HttpPost]
    public IActionResult CreateANewCar(Car entity)
    {
        ...
    }
    [HttpPut("{id}")]
    public IActionResult UpdateAnExistingCar(int id, Car entity)
    {
        ...
    }
    [HttpDelete("{id}")]
    public IActionResult DeleteACar(int id, Car entity)
    {
        ...
    }
}
```

If an action method doesn’t have an HTTP verb attribute, it is treated as the application endpoint for get requests. If the route request is matched but there isn’t an action method with the correct verb attribute, the server will return a 404 (not found).

> Note ASP.NET Web API allowed you to omit the HTTP verb for a method if the name started with Get, Put, Delete, or Post. This convention is generally considered a bad idea and has been removed in ASP.NET Core. If an action method does not have an HTTP verb specified, it will be called using an HTTP Get.

The final endpoint selector for API controllers is the optional Consumes attribute, which specifies the content type that is accepted by the endpoint. The request must use the matching content-type header, or a 415 Unsupported Media Type error will be returned. The following two example endpoints, both in the same controller, differentiate between JSON and XML:

```
[HttpPost]
[Consumes("application/json")]
public IActionResult PostJson(IEnumerable<int> values) =>
    Ok(new { Consumes = "application/json", Values = values });

[HttpPost]
[Consumes("application/x-www-form-urlencoded")]
public IActionResult PostForm([FromForm] IEnumerable<int> values) =>
    Ok(new { Consumes = "application/x-www-form-urlencoded", Values = values });
```