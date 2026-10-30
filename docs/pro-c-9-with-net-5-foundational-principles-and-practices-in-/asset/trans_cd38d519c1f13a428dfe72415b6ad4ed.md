

```csharp
[HttpPost]
public IActionResult Delete(int id, Car recordToDelete)
{
    ...
}
```

路由也可以使用 HTTP 动词特性来代替 Route 特性进行修改。例如，下面演示了如何在两个 `Delete()` 方法的路由模板中添加可选的 `id` 路由标记：

```csharp
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

路由也可以使用 HTTP 动词来重新声明；只需在路由模板前加上正斜杠（`/`），如下例所示：

```csharp
[HttpGet("/[controller]/[action]/{makeId}/{makeName}")]
public IActionResult ByMake(int makeId, string makeName)
{
    ViewBag.MakeName = makeName;
    return View(_repo.GetAllBy(makeId));
}
```

> **注意：** 如果操作方法没有使用 HTTP 动词特性进行修饰，则默认为 `GET` 方法。但是，在 MVC Web 应用程序中，未标记的操作方法也可以响应 `POST` 请求。因此，最佳实践是显式地为所有操作方法标记正确的动词特性。

## API 服务路由

用于 MVC 风格应用程序的路由定义与用于 RESTful 服务的路由定义之间的一个显著区别是，服务路由定义不指定操作方法。操作方法根据请求的 HTTP 动词（以及可选的内容类型）而非名称来选择。下面的代码显示了一个 API 控制器，其中四个方法都匹配相同的路由模板。请注意 HTTP 动词特性：

```csharp
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

如果操作方法没有 HTTP 动词特性，则它将被视为 `GET` 请求的应用程序端点。如果路由请求匹配，但没有具有正确动词特性的操作方法，服务器将返回 404（未找到）。

> **注意：** ASP.NET Web API 允许你省略方法的 HTTP 动词，只要方法名以 `Get`、`Put`、`Delete` 或 `Post` 开头。这种约定通常被认为是一种不好的做法，并且已在 ASP.NET Core 中移除。如果操作方法没有指定 HTTP 动词，则将使用 HTTP `GET` 调用它。

API 控制器的最终端点选择器是可选的 `Consumes` 特性，它指定端点接受的内容类型。请求必须使用匹配的 content-type 标头，否则将返回 415 不支持的媒体类型错误。以下两个示例端点都位于同一个控制器中，用于区分 JSON 和 XML：

```csharp
[HttpPost]
[Consumes("application/json")]
public IActionResult PostJson(IEnumerable<int> values) =>
    Ok(new { Consumes = "application/json", Values = values });

[HttpPost]
[Consumes("application/x-www-form-urlencoded")]
public IActionResult PostForm([FromForm] IEnumerable<int> values) =>
    Ok(new { Consumes = "application/x-www-form-urlencoded", Values = values });
```

## 使用路由进行重定向

路由的另一个优点是，你不必再为站点中的其他页面硬编码 URL。路由条目既用于匹配传入请求，也用于生成 URL。生成 URL 时，会基于当前请求的值添加方案、主机和端口。

## 筛选器

ASP.NET Core 中的筛选器会在请求处理管道的特定阶段之前或之后运行代码。有用于授权和缓存的内置筛选器，也有用于分配自定义筛选器的选项。表 29-7 列出了可以添加到管道中的筛选器类型，并按其执行顺序列出。

## 表 29-7 ASP.NET Core 中可用的筛选器

| 筛选器 | 作用 |
|---|---|
| 授权筛选器 | 首先运行，并确定用户是否有权处理当前请求。 |
| 资源筛选器 | 在授权筛选器之后立即运行，并可在管道其余部分完成后运行。在模型绑定之前运行。 |
| 操作筛选器 | 在操作执行之前和/或操作执行之后立即运行。可以更改传递给操作的值以及操作返回的结果。 |
| 异常筛选器 | 用于对写入响应正文之前发生的未处理异常应用全局策略。 |
| 结果筛选器 | 在操作结果成功执行后立即运行代码。对于围绕视图或格式化程序执行的逻辑很有用。 |

## 授权筛选器

授权筛选器与 ASP.NET Core Identity 系统配合使用，以防止用户访问其无权使用的控制器或操作。不建议构建自定义授权筛选器，因为使用 ASP.NET Core Identity 时，内置的 `AuthorizeAttribute` 和 `AllowAnonymousAttribute` 通常已经足够覆盖。

## 资源筛选器

前置代码在授权筛选器之后、任何其他筛选器之前执行，后置代码在所有其他筛选器之后执行。这使资源筛选器能够短路整个响应管道。资源筛选器的一个常见用途是缓存。如果响应在缓存中，筛选器可以跳过管道的其余部分。

## 操作筛选器

前置代码在操作方法执行之前立即执行，后置代码在操作方法执行之后立即执行。操作筛选器可以短路操作方法以及由操作筛选器包装的任何筛选器（执行顺序和包装顺序将在稍后介绍）。

## 异常筛选器

异常筛选器在应用程序中实现横切错误处理。它们没有 before 或 after 事件，但会处理控制器创建、模型绑定、操作筛选器或操作方法中出现的任何未处理异常。

## 结果筛选器

结果筛选器包装操作方法中 `IActionResult` 的执行。一个常见场景是使用结果筛选器将标头信息添加到 HTTP 响应消息中。

## ASP.NET Core 的新增功能

除了支持 ASP.NET MVC 和 ASP.NET Web API 的基本功能外，开发团队还在之前的框架基础上添加了大量新功能和改进。除了框架和控制器的统一之外，以下是一些额外的改进和创新：

- 内置依赖注入。
- 面向云、基于环境的配置系统。
- 轻量、高性能、模块化的 HTTP 请求管道。
- 整个框架基于细粒度的 NuGet 包。
- 与现代客户端框架和开发工作流的集成。
- 引入标记帮助程序。
- 引入视图组件。
- 性能的显著提升。

## 内置依赖注入

依赖注入（DI）是一种支持对象之间松散耦合的机制。不再直接创建依赖对象或将特定实现传递给类和/或方法，而是将参数定义为接口。这样，接口的任何实现都可以传递给类或方法，极大地提高了应用程序的灵活性。

DI 支持是 ASP.NET Core 重写中的主要原则之一。`Startup` 类（本章稍后介绍）不仅通过依赖注入接受所有配置和中间件服务，你的自定义类也可以（并且应该）添加到服务容器中，以注入到应用程序的其他部分。当某项配置到 ASP.NET Core DI 容器中时，有三种生存期选项，如表 29-8 所示。

## 表 29-8 服务的生存期选项

| 生存期选项 | 提供的功能 |
|---|---|
| 瞬态 | 每次需要时创建。 |
| 作用域 | 每个请求创建一次。建议用于 Entity Framework `DbContext` 对象。 |
| 单例 | 在首次请求时创建一次，然后在对象的生存期内重用。与将你的类实现为单例相比，这是推荐的方法。 |

