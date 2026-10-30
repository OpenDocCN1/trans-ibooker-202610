
尽管 ProducesResponseType 特性将响应代码添加到了文档中，但这些信息无法自定义。幸运的是，Swashbuckle 正是为此目的添加了 SwaggerResponse 特性。将 Get() 方法更新为以下代码：

```
[HttpGet]
[Produces("application/json")]
[ProducesResponseType(StatusCodes.Status200OK)]
[ProducesResponseType(StatusCodes.Status400BadRequest)]
[SwaggerResponse(200, "The execution was successful")]
[SwaggerResponse(400, "The request was invalid")]
public ActionResult<IEnumerable<string>> Get()
{
    return new string[] {"value1", "value2"};
}
```

在 Swagger 注解被识别并添加到生成的文档中之前，必须先启用它们。打开 Startup.cs 并导航到 Configure() 方法。将 AddSwaggerGen() 的调用更新为以下代码：

```
services.AddSwaggerGen(c =>
{
    c.EnableAnnotations();
    ...
});
```

现在，当你在 Swagger UI 中查看 responses 部分时，你将看到自定义的消息，如图 30-5 所示。

![](img/587021f4d5ba039be70a11107130377b_1278_0.png)

图 30-5. Swagger UI 中更新后的响应

> 注意：Swashbuckle 还支持许多其他自定义功能。请参阅 https://github.com/domaindrivendev/Swashbuckle.AspNetCore 上的文档以获取更多信息。

## 构建 API 操作方法

AutoLot.Api 应用程序的大部分功能可以归类为以下方法之一：

- GetOne()
- GetAll()
- UpdateOne()
- AddOne()
- DeleteOne()

主要的 API 方法将在通用的基础 API 控制器中实现。首先在 AutoLot.Api 项目的 Controllers 目录中创建一个名为 Base 的新文件夹。在此文件夹中，添加一个名为 BaseCrudController.cs 的新类。将 using 语句和类定义更新为以下代码：

```
using System;
using System.Collections.Generic;
using AutoLot.Dal.Exceptions;
using AutoLot.Models.Entities.Base;
using AutoLot.Dal.Repos.Base;
using AutoLot.Services.Logging;
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Swashbuckle.AspNetCore.Annotations;
```

```
namespace AutoLot.Api.Controllers.Base
{
    [ApiController]
    public abstract class BaseCrudController<T, TController> : ControllerBase
        where T : BaseEntity, new()
        where TController : BaseCrudController<T, TController>
    {
    }
}
```

该类是公共抽象类，继承自 ControllerBase。该类接受两个泛型参数。第一个类型被约束为派生自 BaseEntity 并且具有默认构造函数，第二个类型派生自 BaseCrudController（用于表示派生控制器）。当将 ApiController 特性添加到基类时，派生控制器将获得该特性提供的功能。

> 注意：此类上未定义路由。它将通过派生类进行设置。

### 构造函数

下一步是添加两个受保护的类级变量：一个用于保存 IRepo<T> 的实例，另一个用于保存 IAppLogging<T> 的实例。这两个变量都应通过构造函数进行设置。

```
protected readonly IRepo<T> MainRepo;
protected readonly IAppLogging<TController> Logger;
protected BaseCrudController(IRepo<T> repo, IAppLogging<TController> logger)
{
    MainRepo = repo;
    Logger = logger;
}
```

### Get 方法

有两个 HTTP Get 方法，即 GetOne() 和 GetAll()。两者都使用传入控制器的 repo。首先，添加 GetAll() 方法。此方法作为派生控制器路由模板的端点。

```
/// <summary>
/// Gets all records
/// </summary>
/// <returns>All records</returns>
/// <response code="200">Returns all items</response>
[Produces("application/json")]
[ProducesResponseType(StatusCodes.Status200OK)]
[SwaggerResponse(200, "The execution was successful")]
[SwaggerResponse(400, "The request was invalid")]
[HttpGet]
public ActionResult<IEnumerable<T>> GetAll()
{
    return Ok(MainRepo.GetAllIgnoreQueryFilters());
}
```

下一个方法根据 id 获取单个记录，id 作为必需的路由参数传入，并添加到派生控制器的路由中。

```
/// <summary>
/// Gets a single record
/// </summary>
/// <param name="id">Primary key of the record</param>
/// <returns>Single record</returns>
/// <response code="200">Found the record</response>
/// <response code="204">No content</response>
[Produces("application/json")]
[ProducesResponseType(StatusCodes.Status200OK)]
[ProducesResponseType(StatusCodes.Status204NoContent)]
[SwaggerResponse(200, "The execution was successful")]
[SwaggerResponse(204, "No content")]
[HttpGet("{id}")]
public ActionResult<T> GetOne(int id)
{
    var entity = MainRepo.Find(id);
    if (entity == null)
    {
        return NotFound();
    }
    return Ok(entity);
}
```

路由值会自动分配给 id 参数（隐式 [FromRoute]）。

### UpdateOne 方法

HTTP Put 谓词表示对记录的更新。此处列出该方法，随后进行解释：

```
/// <summary>
/// Updates a single record
/// </summary>
/// <remarks>
/// Sample body:
/// <pre>
/// {
///   "Id": 1,
///   "TimeStamp": "AAAAAAAAAB+E="
///   "MakeId": 1,
///   "Color": "Black",
///   "PetName": "Zippy",
///   "MakeColor": "VW (Black)",
/// }
/// </pre>
/// </remarks>
/// <param name="id">Primary key of the record to update</param>
/// <returns>Single record</returns>
/// <response code="200">Found and updated the record</response>
/// <response code="400">Bad request</response>
[Produces("application/json")]
[ProducesResponseType(StatusCodes.Status200OK)]
[ProducesResponseType(StatusCodes.Status400BadRequest)]
[SwaggerResponse(200, "The execution was successful")]
[SwaggerResponse(400, "The request was invalid")]
[HttpPut("{id}")]
public IActionResult UpdateOne(int id,T entity)
{
    if (id != entity.Id)
    {
        return BadRequest();
    }

    try
    {
        MainRepo.Update(entity);
    }
    catch (CustomException ex)
    {
        //This shows an example with the custom exception
        //Should handle more gracefully
        return BadRequest(ex);
    }
```

```
catch (Exception ex)
{
    //Should handle more gracefully
    return BadRequest(ex);
}

return Ok(entity);
}
```

该方法首先根据派生控制器的路由将路由设置为 HttpPut 请求，并带有必需的 Id 路由参数。路由值分配给 id 参数（隐式 [FromRoute]），实体从请求体中分配（隐式 [FromBody]）。另请注意，没有对 ModelState 有效性进行检查。这也是由 ApiController 特性自动完成的。如果 ModelState 无效，将向客户端返回 400（BadRequest）。

该方法检查以确保路由值（id）与请求体中的 id 匹配。如果不匹配，则返回 BadRequest。如果匹配，则使用 repo 更新记录。如果更新因异常失败，将向客户端返回 400。如果全部成功，将向客户端返回 200（OK），并将更新后的记录作为响应体传递。

> 注意：此示例（以及其余示例）中的异常处理非常不充分。生产应用程序应利用您迄今为止在本书中学到的所有知识，根据需求妥善处理问题。

### AddOne 方法

HTTP Post 谓词表示对记录的插入。此处列出该方法，随后进行解释：

```
/// <summary>
/// Adds a single record
/// </summary>
/// <remarks>
/// Sample body:
/// <pre>
/// {
/// "Id": 1,
/// "TimeStamp": "AAAAAAAAAB+E="
/// "MakeId": 1,
/// "Color": "Black",
/// "PetName": "Zippy",
/// "MakeColor": "VW (Black)",
/// }
/// </pre>
/// </remarks>
/// <returns>Added record</returns>
/// <response code="201">Found and updated the record</response>
/// <response code="400">Bad request</response>
[Produces("application/json")]
```

