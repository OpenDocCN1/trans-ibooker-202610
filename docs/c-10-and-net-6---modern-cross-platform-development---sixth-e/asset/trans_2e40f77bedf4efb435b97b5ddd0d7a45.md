
让我们配置这些端口：

- 1. 在 Northwind.WebApi 项目中，在 Program.cs 中，添加对 UseUrls 的扩展方法调用以指定 HTTPS 端口 5002，如以下代码中突出显示所示：
```
var builder = WebApplication.CreateBuilder(args);
builder.WebHost.UseUrls("https://localhost:5002/");
```
- 2. 在 Northwind.Mvc 项目中，打开 Program.cs 并导入用于操作 HTTP 客户端工厂的命名空间，如以下代码所示：
```
using System.Net.Http.Headers; // MediaTypeWithQualityHeaderValue
```
- 3. 添加一条语句以启用 HttpClientFactory 并使用命名客户端，通过 HTTPS 在端口 5002 上调用 Northwind Web API 服务并请求 JSON 作为默认响应格式，如以下代码所示：
```
builder.Services.AddHttpClient(name: "Northwind.WebApi",
    configureClient: options =>
    {
        options.BaseAddress = new Uri("https://localhost:5002/");
        options.DefaultRequestHeaders.Accept.Add(
            new MediaTypeWithQualityHeaderValue(
            "application/json", 1.0));
    });
```

## 在控制器中以 JSON 形式获取客户

我们现在可以创建一个 MVC 控制器操作方法，该方法使用工厂创建 HTTP 客户端，对客户执行 GET 请求，并使用 .NET 5 中在 System.Net.Http.Json 程序集和命名空间中引入的便捷扩展方法反序列化 JSON 响应：

- 1. 打开 Controllers/HomeController.cs 并声明一个字段来存储 HTTP 客户端工厂，如以下代码所示：
```
private readonly IHttpClientFactory clientFactory;
```
- 2. 在构造函数中设置该字段，如以下代码中突出显示所示：
```
public HomeController(
    ILogger<HomeController> logger,
    NorthwindContext injectedContext,
    IHttpClientFactory httpClientFactory)
{
    _logger = logger;
    db = injectedContext;
    clientFactory = httpClientFactory;
}
```
- 3. 创建新的操作方法以调用 Northwind Web API 服务、获取所有客户并将它们传递给视图，如以下代码所示：
```
public async Task<IActionResult> Customers(string country)
{
    string uri;

    if (string.IsNullOrEmpty(country))
    {
        ViewData["Title"] = "All Customers Worldwide";
        uri = "api/customers/";
    }
    else
    {
        ViewData["Title"] = $"Customers in {country}";
        uri = $"api/customers/?country={country}";
    }

    HttpClient client = clientFactory.CreateClient(
        name: "Northwind.WebApi");

    HttpRequestMessage request = new(
        method: HttpMethod.Get, requestUri: uri);

    HttpResponseMessage response = await client.SendAsync(request);

    IEnumerable<Customer>? model = await response.Content
        .ReadFromJsonAsync<IEnumerable<Customer>>();

    return View(model);
}
```
- 4. 在 Views/Home 文件夹中，创建一个名为 Customers.cshtml 的 Razor 文件。
- 5. 修改 Razor 文件以呈现客户，如以下标记所示：
```
@using Packt.Shared
@model IEnumerable<Customer>
<h2>@ViewData["Title"]</h2>
<table class="table">
    <thead>
        <tr>
            <th>Company Name</th>
            <th>Contact Name</th>
            <th>Address</th>
            <th>Phone</th>
        </tr>
    </thead>
<tbody>
    @if (Model is not null)
    {
        @foreach (Customer c in Model)
        {
            <tr>
                <td>
                    @Html.DisplayFor(modelItem => c.CompanyName)
                </td>
                <td>
                    @Html.DisplayFor(modelItem => c.ContactName)
                </td>
                <td>
                    @Html.DisplayFor(modelItem => c.Address)
                    @Html.DisplayFor(modelItem => c.City)
                    @Html.DisplayFor(modelItem => c.Region)
                    @Html.DisplayFor(modelItem => c.Country)
                    @Html.DisplayFor(modelItem => c.PostalCode)
                </td>
                <td>
                    @Html.DisplayFor(modelItem => c.Phone)
                </td>
            </tr>
        }
    }
</tbody>
</table>
```
- 6. 在 Views/Home/Index.cshtml 中，在呈现访问者计数后添加一个表单，允许访问者输入国家并查看客户，如以下标记所示：
```
<h3>Query customers from a service</h3>
<form asp-action="Customers" method="get">
    <input name="country" placeholder="Enter a country" />
    <input type="submit" />
</form>
```

## 启用跨域资源共享

跨域资源共享 (CORS) 是一种基于 HTTP 头的标准，用于在客户端和服务器位于不同域（源）时保护 Web 资源。它允许服务器指示除其自身以外的哪些源（由域、方案或端口的组合定义）可以加载资源。

由于我们的 Web 服务托管在端口 5002 上，而我们的 MVC 网站托管在端口 5000 和 5001 上，它们被视为不同的源，因此资源无法共享。

在服务器上启用 CORS 并配置我们的 Web 服务仅允许来自 MVC 网站的请求将很有用：

- 1. 在 Northwind.WebApi 项目中，打开 Program.cs。
- 2. 在服务配置部分添加一条语句以添加 CORS 支持，如以下代码所示：
- 3. 在 HTTP 管道配置部分中，在调用 UseEndpoints 之前添加一条语句以使用 CORS 并允许来自具有源 https://localhost:5001 的类似 Northwind MVC 的任何网站的 GET、POST、PUT 和 DELETE 请求，如以下代码所示：
- 4. 启动 Northwind.WebApi 项目并确认 Web 服务仅在端口 5002 上监听，如以下输出所示：
- 5. 启动 Northwind.Mvc 项目并确认网站在端口 5000 和 5002 上监听，如以下输出所示：
- 6. 启动 Chrome。
- 7. 在客户表单中，输入像 Germany、UK 或 USA 这样的国家，点击提交，并注意客户列表，如图 16.18 所示：
```
builder.Services.AddCors();
```
```
app.UseCors(configurePolicy: options =>
{
    options.WithMethods("GET", "POST", "PUT", "DELETE");
    options.WithOrigins(
        "https://localhost:5001" // allow requests from the MVC client
    );
});
```
```
info: Microsoft.Hosting.Lifetime[14]
  Now listening on: https://localhost:5002
```
```
info: Microsoft.Hosting.Lifetime[14]
  Now listening on: https://localhost:5001
info: Microsoft.Hosting.Lifetime[14]
  Now listening on: http://localhost:5000
```

![](img/c63b00dca7192cc58cfb9e1f1e64f483_737_0.png)

图 16.18：英国的客户

- 8. 在浏览器中点击后退按钮，清除国家文本框，点击提交，并注意全球客户列表。
- 9. 在命令提示符或终端中，注意 HttpClient 记录其发出的每个 HTTP 请求和收到的 HTTP 响应，如以下输出所示：
- 10. 关闭 Chrome 并关闭 Web 服务器。
```
info: System.Net.Http.HttpClient.Northwind.WebApi.ClientHandler[100]
  Sending HTTP request GET https://localhost:5002/api/customers/?country=UK
info: System.Net.Http.HttpClient.Northwind.WebApi.ClientHandler[101]
  Received HTTP response headers after 931.864ms - 200
```

您已成功构建 Web 服务并从 MVC 网站调用它。

## 为 Web 服务实现高级功能

现在您已经了解了构建 Web 服务然后从客户端调用它的基础知识，让我们来看一些更高级的功能。

## 实现健康检查 API

有许多付费服务执行站点可用性测试，这些测试是基本的 ping，一些还对 HTTP 响应进行更高级的分析。

ASP.NET Core 2.2 及更高版本使实现更详细的网站健康检查变得容易。例如，您的网站可能已上线，但它准备好了吗？它能否从数据库中检索数据？

让我们为我们的 Web 服务添加基本的健康检查功能：

- 1. 在 Northwind.WebApi 项目中，添加项目引用以启用 Entity Framework Core 数据库健康检查，如以下标记所示：
- 2. 构建项目。
- 3. 在 Program.cs 中，在服务配置部分的底部，添加一条语句以添加健康检查，包括 Northwind 数据库上下文，如以下代码所示：
- 4. 在 HTTP 管道配置部分中，在调用 MapControllers 之前，添加一条语句以使用基本的健康检查，如以下代码所示：
- 5. 启动 Web 服务。
- 6. 启动 Chrome。
- 7. 导航到 https://localhost:5002/howdoyoufeel 并注意 Web 服务使用纯文本响应 Healthy 进行响应。
- 8. 在命令提示符或终端中，注意执行以测试数据库健康状况的 SQL 语句，如以下输出所示：
- 9. 关闭 Chrome 并关闭 Web 服务器。
```
<PackageReference Include=
"Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore"
Version="6.0.0" />
```
```
builder.Services.AddHealthChecks()
.AddDbContextCheck<NorthwindContext>();
```
> 默认情况下，数据库上下文检查调用 EF Core 的 CanConnectAsync 方法。您可以通过调用 AddDbContextCheck 方法来自定义运行的操作。
```
app.UseHealthChecks(path: "/howdoyoufeel");
```
```
Level: Debug, Event Id: 20100, State: Executing DbCommand [Parameters=[]],
CommandType='Text', CommandTimeout='30']
SELECT 1
```
