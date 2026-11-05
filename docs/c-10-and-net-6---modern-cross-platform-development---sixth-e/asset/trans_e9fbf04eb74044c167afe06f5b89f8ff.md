
## 配置客户存储库和 Web API 控制器

现在您将配置存储库，以便可以从 Web API 控制器内部调用它。

您将在 Web 服务启动时为存储库注册一个作用域依赖服务实现，然后使用构造函数参数注入将其获取到一个新的 Web API 控制器中以处理客户。

为了展示使用路由区分 MVC 和 Web API 控制器的示例，我们将为 customers 控制器使用常见的 /api URL 前缀约定：

- 1. 打开 Program.cs 并导入 Northwind.WebApi.Repositories 命名空间。
- 2. 在调用 Build 方法之前添加一条语句，该语句将 CustomerRepository 注册为在运行时使用的作用域依赖项，如以下代码中突出显示所示：
- 3. 在 Controllers 文件夹中，添加一个名为 CustomersController.cs 的新类。
- 4. 在 CustomersController 类文件中，添加语句以定义用于处理客户的 Web API 控制器类，如以下代码所示：

```
builder.Services.AddScoped<ICustomerRepository, CustomerRepository>();

var app = builder.Build();
```

> 良好实践：我们的存储库使用注册为作用域依赖项的数据库上下文。您只能在其他作用域依赖项内部使用作用域依赖项，因此我们无法将存储库注册为单例。您可以在以下链接中阅读更多相关信息：https://docs.microsoft.com/en-us/dotnet/core/extensions/dependency-injection#scoped

```csharp
using Microsoft.AspNetCore.Mvc; // [Route], [ApiController], ControllerBase
using Packt.Shared; // Customer
using Northwind.WebApi.Repositories; // ICustomerRepository

namespace Northwind.WebApi.Controllers;

// base address: api/customers
[Route("api/[controller]")]
[ApiController]
public class CustomersController : ControllerBase
{
private readonly ICustomerRepository repo;

// constructor injects repository registered in Startup
public CustomersController(ICustomerRepository repo)
{
    this.repo = repo;
}

// GET: api/customers
// GET: api/customers/?country=[country]
// this will always return a list of customers (but it might be empty)
[HttpGet]
[ProducesResponseType(200, Type = typeof(IEnumerable<Customer>))]
public async Task<IEnumerable<Customer>> GetCustomers(string? country)
{
    if (string.IsNullOrWhiteSpace(country))
    {
        return await repo.RetrieveAllAsync();
    }
    else
    {
        return (await repo.RetrieveAllAsync())
            .Where(customer => customer.Country == country);
    }
}

// GET: api/customers/[id]
[HttpGet("{id}", Name = nameof(GetCustomer))] // named route
[ProducesResponseType(200, Type = typeof(Customer))]
[ProducesResponseType(404)]
public async Task<IActionResult> GetCustomer(string id)
{
    Customer? c = await repo.RetrieveAsync(id);
    if (c == null)
    {
        return NotFound(); // 404 Resource not found
    }
    return Ok(c); // 200 OK with customer in body
}

// POST: api/customers
// BODY: Customer (JSON, XML)
[HttpPost]
[ProducesResponseType(201, Type = typeof(Customer))]
[ProducesResponseType(400)]
public async Task<IActionResult> Create([FromBody] Customer c)
{
    if (c == null)
{
    return BadRequest(); // 400 Bad request
}

Customer? addedCustomer = await repo.CreateAsync(c);

if (addedCustomer == null)
{
    return BadRequest("Repository failed to create customer.");
}
else
{
    return CreatedAtRoute( // 201 Created
        routeName: nameof(GetCustomer),
        routeValues: new { id = addedCustomer.CustomerId.ToLower() },
        value: addedCustomer);
}
}

// PUT: api/customers/[id]
// BODY: Customer (JSON, XML)
[HttpPut("{id}")]
[ProducesResponseType(204)]
[ProducesResponseType(400)]
[ProducesResponseType(404)]
public async Task<IActionResult> Update(
    string id, [FromBody] Customer c)
{
    id = id.ToUpper();
    c.CustomerId = c.CustomerId.ToUpper();

    if (c == null || c.CustomerId != id)
    {
        return BadRequest(); // 400 Bad request
    }

    Customer? existing = await repo.RetrieveAsync(id);
    if (existing == null)
    {
        return NotFound(); // 404 Resource not found
    }

    await repo.UpdateAsync(id, c);

    return new NoContentResult(); // 204 No content
}

// DELETE: api/customers/[id]
[HttpDelete("{id}")]
[ProducesResponseType(204)]
[ProducesResponseType(400)]
[ProducesResponseType(404)]
public async Task<IActionResult> Delete(string id)
{
    Customer? existing = await repo.RetrieveAsync(id);
    if (existing == null)
    {
        return NotFound(); // 404 Resource not found
    }

    bool? deleted = await repo.DeleteAsync(id);

    if (deleted.HasValue && deleted.Value) // short circuit AND
    {
        return new NoContentResult(); // 204 No content
    }
    else
    {
        return BadRequest( // 400 Bad request
            $"Customer {id} was found but failed to delete.");
    }
}
}
```

在审查此 Web API 控制器类时，请注意以下事项：

- Controller 类注册的路由以 api/ 开头，并包含控制器的名称，即 api/customers。
- 构造函数使用依赖注入获取用于处理客户的已注册存储库。
- 有五个操作方法用于对客户执行 CRUD 操作——两个 GET 方法（用于所有客户或单个客户）、POST（创建）、PUT（更新）和 DELETE。
- GetCustomers 方法可以传递一个包含国家名称的字符串参数。如果缺少该参数，则返回所有客户。如果存在，则用于按国家筛选客户。
- GetCustomer 方法的路由明确命名为 GetCustomer，以便在插入新客户后可用于生成 URL。
- Create 和 Update 方法都使用 [FromBody] 装饰 customer 参数，以告诉模型绑定器使用 POST 请求正文中的值填充它。
- Create 方法返回的响应使用 GetCustomer 路由，以便客户端将来知道如何获取新创建的资源。我们将两个方法匹配起来以创建然后获取客户。
- Create 和 Update 方法无需检查 HTTP 请求正文中传递的客户的模型状态并在无效时返回包含模型验证错误详细信息的 400 Bad Request，因为控制器使用 [ApiController] 装饰，该装饰会为您执行此操作。

当服务接收到 HTTP 请求时，它将创建 Controller 类的实例，调用适当的操作方法，以客户端首选的格式返回响应，并释放控制器使用的资源，包括存储库及其数据上下文。

## 指定问题详细信息

在 ASP.NET Core 2.1 及更高版本中添加的一项功能是对指定问题详细信息的 Web 标准实现。

在启用了 ASP.NET Core 2.2 或更高版本兼容性的项目中，使用 [ApiController] 装饰的 Web API 控制器中，返回 IActionResult 并返回客户端错误状态代码（即 4xx）的操作方法将自动在响应正文中包含 ProblemDetails 类的序列化实例。

如果您想掌控主动权，则可以自行创建 ProblemDetails 实例并包含附加信息。

让我们模拟一个需要向客户端返回自定义数据的错误请求：

- 1. 在 Delete 方法的实现顶部，添加语句以检查 id 是否匹配字面字符串值 "bad"，如果是，则返回自定义问题详细信息对象，如以下代码所示：
- 2. 您稍后将测试此功能。

```csharp
// take control of problem details
if (id == "bad")
{
    ProblemDetails problemDetails = new()
    {
        Status = StatusCodes.Status400BadRequest,
        Type = "https://localhost:5001/customers/failed-to-delete",
        Title = $"Customer ID {id} found but failed to delete.",
        Detail = "More details like Company Name, Country and so on.",
        Instance = HttpContext.Request.Path
    };
    return BadRequest(problemDetails); // 400 Bad Request
}
```
