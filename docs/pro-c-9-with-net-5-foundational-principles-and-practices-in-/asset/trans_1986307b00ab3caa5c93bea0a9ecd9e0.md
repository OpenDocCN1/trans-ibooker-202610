

```csharp
[ProducesResponseType(StatusCodes.Status201Created)]
[ProducesResponseType(StatusCodes.Status400BadRequest)]
[SwaggerResponse(201, "The execution was successful")]
[SwaggerResponse(400, "The request was invalid")]
[HttpPost]
public ActionResult<T> AddOne(T entity)
{
    try
    {
        MainRepo.Add(entity);
    }
    catch (Exception ex)
    {
        return BadRequest(ex);
    }
    return CreatedAtAction(nameof(GetOne), new {id = entity.Id}, entity);
}
```

该方法首先将路由定义为 HTTP Post。由于是新记录，因此没有路由参数。如果仓库成功添加记录，则响应为 CreatedAtAction()。这会向客户端返回 HTTP 201，并将新创建实体的 URL 作为 Location 头值。响应的主体是作为 JSON 的新添加实体。

## The DeleteOne Method

HTTP Delete 动词表示删除一条记录。创建实例后，使用仓库处理删除操作。整个方法如下所示：

```csharp
/// <summary>
/// Deletes a single record
/// </summary>
/// <remarks>
/// Sample body:
/// <pre>
/// {
///   "Id": 1,
///   "TimeStamp": "AAAAAAAAAB+E="
/// }
/// </pre>
/// </remarks>
/// <returns>Nothing</returns>
/// <response code="200">Found and deleted the record</response>
/// <response code="400">Bad request</response>
[Produces("application/json")]
[ProducesResponseType(StatusCodes.Status200OK)]
[ProducesResponseType(StatusCodes.Status400BadRequest)]
[SwaggerResponse(200, "The execution was successful")]
[SwaggerResponse(400, "The request was invalid")]
[HttpDelete("{id}")]
public ActionResult<T> DeleteOne(int id, T entity)
{
    if (id != entity.Id)
    {
        return BadRequest();
    }
    try
    {
        MainRepo.Delete(entity);
    }
    catch (Exception ex)
    {
        //Should handle more gracefully
        return new BadRequestObjectResult(ex.GetBaseException()?.Message);
    }
    return Ok();
}
```

该方法首先将路由定义为带必需路由参数 id 的 HTTP Delete。路由中的 id 会与随实体主体一起发送的 id 进行比较，如果不匹配，则返回 BadRequest。如果仓库成功删除记录，则响应为 OK；如果发生错误，则响应为 BadRequest。

至此，基础控制器已完成。

## The CarsController

AutoLot.Api 应用需要一个额外的 HTTP Get 方法来根据 Make 值获取 Car 记录。这将放入一个名为 CarsController 的新类中。在 Controllers 文件夹中创建一个名为 CarsController 的空 API 控制器。将 using 语句更新为以下内容：

```csharp
using System.Collections.Generic;
using AutoLot.Api.Controllers.Base;
using Microsoft.AspNetCore.Mvc;
using AutoLot.Models.Entities;
using AutoLot.Dal.Repos.Interfaces;
using AutoLot.Services.Logging;
using Microsoft.AspNetCore.Http;
using Swashbuckle.AspNetCore.Annotations;
```

CarsController 派生自 BaseCrudController 并定义了控制器路由。构造函数接收实体特定的仓库和记录器的实例。以下是初始控制器布局：

```csharp
namespace AutoLot.Api.Controllers
{
    [Route("api/[controller]")]
    public class CarsController : BaseCrudController<Car, CarsController>
    {
        public CarsController(ICarRepo carRepo, IAppLogging<CarsController> logger) :
            base(carRepo, logger)
        {
        }
    }
}
```

CarsController 通过另一个操作方法扩展基类，该方法获取特定 make 的所有汽车。添加以下代码，解释随后给出：

```csharp
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
    if (id.HasValue && id.Value > 0)
    {
        return Ok(((ICarRepo)MainRepo).GetAllBy(id.Value));
    }
    return Ok(MainRepo.GetAllIgnoreQueryFilters());
}
```

HTTP Get 属性使用 bymake 常量扩展路由，然后是可选的 make 的 id 用于过滤，例如：

```
https://localhost:5021/api/cars/bymake/5
```

接下来，它检查是否传入了 id 值。如果没有，则获取所有车辆。如果传入了值，则使用 `CarRepo` 的 `GetAllBy()` 方法按 make 获取汽车。由于基类的 `MainRepo` 受保护属性定义为 `IRepo<T>`，因此必须将其转换回 `ICarRepo` 接口。

## The Remaining Controllers

其余实体特定的控制器都派生自 BaseCrudController，但不添加任何额外功能。在 Controllers 文件夹中添加四个更多空 API 控制器，命名为 CreditRisksController、CustomersController、MakesController 和 OrdersController。其余控制器全部如下所示：

```csharp
//CreditRisksController.cs
using AutoLot.Api.Controllers.Base;
using AutoLot.Models.Entities;
using AutoLot.Dal.Repos.Interfaces;
using AutoLot.Services.Logging;
using Microsoft.AspNetCore.Mvc;

namespace AutoLot.Api.Controllers
{
    [Route("api/[controller]")]
    public class CreditRisksController : BaseCrudController<CreditRisk, CreditRisksController>
    {
        public CreditRisksController(
            ICreditRiskRepo creditRiskRepo, IAppLogging<CreditRisksController> logger)
            : base(creditRiskRepo, logger)
        {
        }
    }
}

//CustomersController.cs
using AutoLot.Api.Controllers.Base;
using AutoLot.Models.Entities;
using AutoLot.Dal.Repos.Interfaces;
using AutoLot.Services.Logging;
using Microsoft.AspNetCore.Mvc;

namespace AutoLot.Api.Controllers
{
    [Route("api/[controller]")]
    public class CustomersController : BaseCrudController<Customer, CustomersController>
    {
        public CustomersController(
            ICustomerRepo customerRepo, IAppLogging<CustomersController> logger)
            : base(customerRepo, logger)
        {
        }
    }
}

//MakesController.cs
using AutoLot.Api.Controllers.Base;
using AutoLot.Models.Entities;
using Microsoft.AspNetCore.Mvc;
using AutoLot.Dal.Repos.Interfaces;
using AutoLot.Services.Logging;

namespace AutoLot.Api.Controllers
{
    [Route("api/[controller]")]
    public class MakesController : BaseCrudController<Make, MakesController>
    {
        public MakesController(IMakeRepo makeRepo, IAppLogging<MakesController> logger)
            : base(makeRepo, logger)
        {
        }
    }
}

//OrdersController.cs
using AutoLot.Api.Controllers.Base;
using AutoLot.Dal.Repos.Interfaces;
using AutoLot.Models.Entities;
using AutoLot.Services.Logging;
using Microsoft.AspNetCore.Mvc;

namespace AutoLot.Api.Controllers
{
    [Route("api/[controller]")]
    public class OrdersController : BaseCrudController<Order, OrdersController>
    {
        public OrdersController(IOrderRepo orderRepo, IAppLogging<OrdersController> logger) :
            base(orderRepo, logger)
        {
        }
    }
}
```

至此，所有控制器均已完成，您可以使用 Swagger UI 测试所有功能。如果要添加/更新/删除记录，请在 appsettings.Development.json 文件中将 RebuildDataBase 值更新为 true。

```json
{
...
"RebuildDataBase": true,
...
}
```

## Exception Filters

当 Web API 应用中发生异常时，不会显示错误页面，因为用户通常是另一个应用程序而不是人类。任何信息都必须连同 HTTP 状态码一起作为 JSON 发送。如第 29 章所述，ASP.NET Core 允许创建在未处理异常事件中运行的过滤器。过滤器可以全局应用、在控制器级别应用或在操作级别应用。对于此应用程序，您将构建一个异常过滤器以返回格式化的 JSON（连同 HTTP 500），如果站点以调试模式运行，则包含堆栈跟踪。

> 注意：过滤器是 ASP.NET Core 的一项功能强大的特性。在本章中，我们仅检查异常过滤器，但可以创建许多其他过滤器，可以在构建 ASP.NET Core 应用时节省大量时间。有关过滤器的完整信息，请参阅 https://docs.microsoft.com/en-us/aspnet/core/mvc/controllers/filters 上的文档。

