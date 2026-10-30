```
{
    }
}
}
```

The CarsController extends the base class with another action method that gets all of the cars for a particular make. Add the following code, and the explanation will follow:

```
/// <summary>
/// Gets all cars by make
/// </summary>
/// <returns>All cars for a make</returns>
/// <param name="id">Primary key of the make</param>
/// <response code="200">Returns all cars by make</response>
[Produces("application/json")]
[ProducesResponseType(StatusCodes.Status200OK)]
[ProducesResponseType(StatusCodes.Status204NoContent)]
[SwaggerResponse(200, "The execution was successful")]
[SwaggerResponse(204, "No content")]

[HttpGet("bymake/{id?}")]
public ActionResult<IEnumerable<Car>> GetCarsByMake(int? id)
{
    if (id.HasValue && id.Value>0)
    {
        return Ok(((ICarRepo)MainRepo).GetAllBy(id.Value));
    }
    return Ok(MainRepo.GetAllIgnoreQueryFilters());
}
```

The HTTP Get attribute extends the route with the bymake constant and then the optional id of the make to filter on, for example:

```
https://localhost:5021/api/cars/bymake/5
```

Next, it checks if a value was passed in for the id. If not, it gets all vehicles. If a value was passed in, it uses the GetAllBy() method of the CarRepo to get the cars by make. Since the MainRepo protected property of the base class is defined as IRepo<T>, it must be cast back to the ICarRepo interface.

# The Remaining Controllers

The remaining entity-specific controllers all derive from the BaseCrudController but don’t add any additional functionality. Add four more empty API controllers named CreditRisksController, CustomersController, MakesController, and OrdersController to the Controllers folder. The remaining controllers are all shown here:

```
//CreditRisksController.cs
using AutoLot.Api.Controllers.Base;
using AutoLot.Models.Entities;
using AutoLot.Dal.Repos.Interfaces;
```