

## 第30章：使用ASP.NET Core构建RESTful服务

前一章介绍了ASP.NET Core，讨论了一些新特性，创建了项目，并更新了AutoLot.Mvc和AutoLot.Api中的代码，以加入AutoLot.Dal和Serilog日志记录。本章重点在于完成AutoLot.Api RESTful服务。

> 注意 本章示例代码位于本书仓库的Chapter 30目录中。你可以继续使用在第二十九章中开始构建的解决方案。

### 介绍ASP.NET Core RESTful服务

ASP.NET MVC框架一经发布便迅速获得关注，微软在ASP.NET MVC 4和Visual Studio 2012中发布了ASP.NET Web API。ASP.NET Web API 2随Visual Studio 2013发布，随后在Visual Studio 2013 Update 1中更新至2.2版本。

ASP.NET Web API从一开始就被设计为基于服务的框架，用于构建表述性状态转移（RESTful）服务。它基于MVC框架减去V（视图），并针对创建无头服务进行了优化。这些服务可以被任何技术调用，而不仅仅是微软旗下的技术。对Web API服务的调用基于核心HTTP动词（Get、Put、Post、Delete），通过统一资源标识符（URI）进行，例如：

```
http://www.skimedic.com:5001/api/cars
```

如果这看起来像统一资源定位符（URL），那是因为它确实是！URL只是一种指向网络中物理资源的URI。

对Web API的调用使用特定主机（本例中为www.skimedic.com）上的超文本传输协议（HTTP）方案，位于特定端口（上例中为5001），后跟路径（api/cars）以及可选的查询和片段（本例中未显示）。Web API调用也可以在消息正文中包含文本，本章将展示这一点。如前一章所述，ASP.NET Core将Web API和MVC统一为一个框架。

### 使用RESTful服务的控制器操作

回想一下，操作返回IActionResult（对于异步操作返回`Task<IActionResult>`）。除了ControllerBase中返回特定HTTP状态码的辅助方法外，操作方法还可以将内容作为格式化后的JavaScript对象表示法（JSON）响应返回。

> 注意 严格来说，操作方法可以返回多种格式。本书介绍JSON是因为它最为常见。

### 格式化JSON响应结果

大多数RESTful API使用JSON（读作“jay-sawn”）从客户端接收数据并向客户端发送数据。下面是一个包含两个值的简单JSON示例：

```
[ "value1", "value2" ]
```

> 注意 第二十章深入介绍了使用System.Text.Json进行JSON序列化。

API还使用HTTP状态码来传达成功或失败。ControllerBase类中可用的部分HTTP状态辅助方法已在上一章的表29-3中列出。成功的请求会返回200范围内的状态码，其中200（OK）是最常见的成功代码。事实上，它非常常见，你无需显式返回OK。如果没有抛出异常且代码未指定状态码，则会向客户端返回200以及任何数据。

为了准备以下示例，请在AutoLot.Api项目中添加一个新的控制器。在Controllers目录中添加一个名为ValuesController.cs的新文件，并更新代码以匹配以下内容：

```
using System.Collections.Generic;
using Microsoft.AspNetCore.Mvc;

[Route("api/[controller]")]
[ApiController]
public class ValuesController : ControllerBase
{
}
```

> 注意 如果使用Visual Studio，其中提供了控制器的脚手架工具。要访问此工具，请右键单击AutoLot.Api项目中的Controllers文件夹，然后选择“添加 ► 控制器”。选择“MVC控制器 – 空”。

这段代码使用一个值（api）和一个标记（[controller]）为控制器设置路由。此路由模板将匹配类似于www.skimedic.com/api/values的URL。下一个特性（ApiController）启用了多个API专用功能（将在下一节介绍）。最后，控制器继承自ControllerBase。如第二十九章所述，ASP.NET Core将经典ASP.NET中所有不同的控制器类型合并为一个名为Controller的类型，其基类为ControllerBase。Controller类提供视图特定功能（MVC中的V），而ControllerBase提供MVC风格应用其余的所有核心功能。

有多种方法可以从操作方法中以JSON形式返回内容。以下示例都返回相同的JSON以及200状态码。差异主要在于风格。请将以下代码添加到你的ValuesController类中：

```
[HttpGet]
public IActionResult Get()
{
    return Ok(new string[] { "value1", "value2" });
}
[HttpGet("one")]
public IEnumerable<string> Get1()
{
    return new string[] { "value1", "value2" };
}
[HttpGet("two")]
public ActionResult<IEnumerable<string>> Get2()
{
    return new string[] { "value1", "value2" };
}
[HttpGet("three")]
public string[] Get3()
{
    return new string[] { "value1", "value2" };
}
[HttpGet("four")]
public IActionResult Get4()
{
    return new JsonResult(new string[] { "value1", "value2" });
}
```

要测试这一点，请运行AutoLot.Api应用程序，你将在Swagger UI中看到ValuesController中的所有方法，如图30-1所示。回想一下，在确定路由时，名称中的Controller后缀会被去掉，因此ValuesController上的端点映射为Values，而不是ValuesController。

![](img/587021f4d5ba039be70a11107130377b_1268_0.png)

图30-1. Swagger文档页面

要执行其中一个方法，请单击Get按钮、“Try it out”按钮，然后执行Execute按钮。方法执行后，UI会更新以显示结果，图30-2仅显示Swagger UI的相关部分。

![](img/587021f4d5ba039be70a11107130377b_1268_1.png)

图30-2. Swagger服务器响应信息

你将看到执行每个方法都会产生相同的JSON结果。

### ApiController特性

ApiController特性是在ASP.NET Core 2.1中添加的，与ControllerBase类结合使用时，提供REST特定的规则、约定和行为。这些约定和行为将在以下各节中概述。

### 特性路由要求

使用ApiController特性时，控制器必须使用特性路由。这只是在强制执行许多人认为的最佳实践。

### 自动400响应

如果模型绑定出现问题，操作将自动返回HTTP 400（错误请求）响应代码。这将替代以下代码：

```
if (!ModelState.IsValid)
{
    return BadRequest(ModelState);
}
```

ASP.NET Core使用ModelStateInvalidFilter操作过滤器来执行上述检查。当存在绑定或验证错误时，HTTP 400响应的正文中会包含错误详细信息。以下是一个示例：

```
{
  "type": "https://tools.ietf.org/html/rfc7231#section-6.5.1",
  "title": "One or more validation errors occurred.",
  "status": 400,
  "traceId": "|7fb5e16a-4c8f23bbfc974667.",
  "errors": {
    "": [
      "A non-empty request body is required."
    ]
  }
}
```

可以通过Startup.cs类的ConfigureServices()方法中的配置来禁用此行为。

```
services.AddControllers()
    .ConfigureApiBehaviorOptions(options =>
    {
        options.SuppressModelStateInvalidFilter = true;
    });
```

### 绑定源参数推断

模型绑定引擎将根据表30-1中列出的约定推断值的获取位置。

**表30-1. 绑定源推断约定**

| 源 | 绑定的参数 |
| --- | --- |
| FromBody | 对于复杂类型参数进行推断，但具有特殊意义的内置类型（如IFormCollection或CancellationToken）除外。只能存在一个FromBody参数，否则将抛出异常。如果需要对简单类型（例如string或int）进行绑定，则仍然需要FromBody特性。 |
| FromForm | 对于类型为IFormFile和IFormFileCollection的操作参数进行推断。当参数标记为FromForm时，会推断出multipart/form-data内容类型。 |
| FromRoute | 对于与路由标记名称匹配的任何参数名称进行推断。 |
| FromQuery | 对于任何其他操作参数进行推断。 |

可以通过Startup.cs类的ConfigureServices()方法中的配置来禁用此行为。

```
services.AddControllers().ConfigureApiBehaviorOptions(options =>
{
    //suppress all binding inference
    options.SuppressInferBindingSourcesForParameters = true;
    //suppress multipart/form-data content type inference
    options.SuppressConsumesConstraintForFormFileParameters = true;
});
```

### 错误状态码的问题详情

ASP.NET Core将错误结果（状态码为400或更高）转换为带有ProblemDetails的结果。

ProblemDetails类型如下所示：

```
public class ProblemDetails
{
    public string Type { get; set; }
    public string Title { get; set; }
    public int? Status { get; set; }
    public string Detail { get; set; }
    public string Instance { get; set; }
    public IDictionary<string, object> Extensions { get; }
        = new Dictionary<string, object>(StringComparer.Ordinal);
}
```

