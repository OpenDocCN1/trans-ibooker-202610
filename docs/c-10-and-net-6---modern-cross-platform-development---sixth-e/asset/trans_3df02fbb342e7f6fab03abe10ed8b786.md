
3. 将 Swashbuckle.AspNetCore 包的版本更新到最新，例如，截至 2021 年 9 月撰写本文时，其版本为 6.2.1。

4. 在 Program.cs 中，注意导入 Microsoft 的 OpenAPI 模型命名空间，如以下代码所示：

```
using Microsoft.OpenApi.Models;
```

5. 导入 Swashbuckle 的 SwaggerUI 命名空间，如以下代码所示：

```
using Swashbuckle.AspNetCore.SwaggerUI; // SubmitMethod
```

6. 在 Program.cs 大约中间位置，注意添加 Swagger 支持的语句，包括 Northwind 服务的文档，表明这是您服务的第一个版本，并更改标题，如以下代码中突出显示所示：

```
builder.Services.AddSwaggerGen(c =>
{
    c.SwaggerDoc("v1", new()
    { Title = "Northwind Service API", Version = "v1" });
});
```

7. 在配置 HTTP 请求管道的部分，注意在开发模式下使用 Swagger 和 Swagger UI 的语句，并为 OpenAPI 规范 JSON 文档定义一个端点。

8. 添加代码以明确列出我们在 Web 服务中想要支持的 HTTP 方法，并更改端点名称，如以下代码中突出显示所示：

```
var app = builder.Build();

// Configure the HTTP request pipeline.
if (builder.Environment.IsDevelopment())
{
    app.UseSwagger();
    app.UseSwaggerUI(c =>
    {
        c.SwaggerEndpoint("/swagger/v1/swagger.json", "Northwind Service API Version 1");
        c.SupportedSubmitMethods(new[] {
            SubmitMethod.Get, SubmitMethod.Post,
            SubmitMethod.Put, SubmitMethod.Delete });
    });
}
```

## 使用 Swagger UI 测试请求

现在您已准备好使用 Swagger 测试 HTTP 请求：

- 1. 启动 Northwind.WebApi Web 服务。

2. 在 Chrome 中，导航到 https://localhost:5001/swagger/ 并注意 **Customers** 和 **WeatherForecast** Web API 控制器已被发现并记录，以及 API 使用的 **Schemas**。

3. 点击 **GET** `/api/Customers/{id}` 以展开该端点并注意客户 **id** 的必填参数，如 *图 16.10* 所示：

![](img/c63b00dca7192cc58cfb9e1f1e64f483_725_0.png)

Figure 16.10: Checking the parameters for a GET request in Swagger

4. 点击 **Try it out**，输入 **id** 为 ALFKI，然后点击宽大的蓝色 **Execute** 按钮，如 *图 16.11* 所示：

![](img/c63b00dca7192cc58cfb9e1f1e64f483_725_1.png)

Figure 16.11: Inputting a customer id before clicking the Execute button

5. 向下滚动并注意 Request URL、Server response with Code、以及包括 Response body 和 Response headers 在内的 Details，如 Figure 16.12 所示：

![](img/c63b00dca7192cc58cfb9e1f1e64f483_726_0.png)

Figure 16.12: Information on ALFKI in a successful Swagger request

6. 滚动回页面顶部，点击 POST /api/Customers 以展开该部分，然后点击 Try it out。

7. 在 Request body 框内点击，并修改 JSON 以定义一个新客户，如以下 JSON 所示：

```
{
  "customerID": "SUPER",
  "companyName": "Super Company",
  "contactName": "Rasmus Ibensen",
  "contactTitle": "Sales Leader",
  "address": "Rotterslef 23",
  "city": "Billund",
  "region": null,
  "postalCode": "4371",
  "country": "Denmark",
  "phone": "31 21 43 21",
  "fax": "31 21 43 22"
}
```

8. 点击 **Execute**，并注意 **Request URL**、带 **Code** 的 **Server response**，以及包括 **Response body** 和 **Response headers** 在内的 **Details**，注意响应代码 201 表示客户已成功创建，如 *图 16.13* 所示：

![](img/c63b00dca7192cc58cfb9e1f1e64f483_727_0.png)

Figure 16.13: Successfully adding a new customer

9. 滚动回页面顶部，点击 GET /api/Customers，点击 Try it out，为 country 参数输入 Denmark，然后点击 Execute，以确认新客户已添加到数据库，如 Figure 16.14 所示：

![](img/c63b00dca7192cc58cfb9e1f1e64f483_728_0.png)

Figure 16.14: Successfully getting customers in Denmark including the newly added customer

10. 点击 DELETE /api/Customers/{id}，点击 Try it out，为 id 输入 super，点击 Execute，并注意 Server response Code 为 204，表示已成功删除，如 Figure 16.15 所示：

![](img/c63b00dca7192cc58cfb9e1f1e64f483_729_0.png)

Figure 16.15: Successfully deleting a customer

11. 再次点击 Execute，并注意 Server response Code 为 404，表示客户已不存在，且 Response body 包含一个 problem details JSON 文档，如 Figure 16.16 所示：

![](img/c63b00dca7192cc58cfb9e1f1e64f483_729_1.png)

Figure 16.16: The deleted customer does not exist anymore

12. 为 **id** 输入 bad，再次点击 **Execute**，并注意 **Server response Code** 为 400，表示客户确实存在但删除失败（在本例中，因为 Web 服务正在模拟此错误），并且 **Response body** 包含自定义的 problem details JSON 文档，如 Figure 16.17 所示：

![](img/c63b00dca7192cc58cfb9e1f1e64f483_730_0.png)

Figure 16.17: The customer did exist but failed to be deleted

13. 使用 GET 方法确认新客户已从数据库中删除（最初丹麦只有两个客户）。

> 我将把使用 PUT 测试更新现有客户的工作留给读者。

14. 关闭 Chrome 并关闭 Web 服务器。

## 启用 HTTP 日志记录

HTTP logging 是一个可选的中间件组件，用于记录有关 HTTP 请求和 HTTP 响应的信息，包括以下内容：

- 有关 HTTP 请求的信息
- Headers
- Body
- 有关 HTTP 响应的信息

这在 Web 服务的审计和调试场景中很有价值，但请注意，因为它可能会对性能产生负面影响。您还可能记录 **个人身份信息 (PII)**，这在某些司法管辖区可能导致合规问题。

让我们看看 HTTP 日志记录的实际效果：

- 1. 在 Program.cs 中，导入用于处理 HTTP 日志记录的命名空间，如以下代码所示：
- 2. 在服务配置部分，添加一条语句来配置 HTTP 日志记录，如以下代码所示：
- 3. 在 HTTP 管道配置部分，添加一条语句在调用使用路由之前添加 HTTP 日志记录，如以下代码所示：
- 4. 启动 Northwind.WebApi Web 服务。
- 5. 启动 Chrome。
- 6. 导航到 https://localhost:5001/api/customers。
- 7. 在命令提示符或终端中，注意请求和响应已被记录，如以下输出所示：

```
using Microsoft.AspNetCore.HttpLogging; // HttpLoggingFields
```

```
builder.Services.AddHttpLogging(options =>
{
    options.LoggingFields = HttpLoggingFields.All;
    options.RequestBodyLogLimit = 4096; // default is 32k
    options.ResponseBodyLogLimit = 4096; // default is 32k
});
```

```
app.UseHttpLogging();
```

```
info: Microsoft.AspNetCore.HttpLogging.HttpLoggingMiddleware[1]
    Request:
    Protocol: HTTP/1.1
    Method: GET
    Scheme: https
    PathBase:
    Path: /api/customers
    QueryString:
    Connection: keep-alive
    Accept: */*
    Accept-Encoding: gzip, deflate, br
    Host: localhost:5001

info: Microsoft.AspNetCore.HttpLogging.HttpLoggingMiddleware[2]
    Response:
```

```
StatusCode: 200
Content-Type: application/json; charset=utf-8
...
Transfer-Encoding: chunked
```

8. 关闭 Chrome 并关闭 Web 服务器。

您现在已准备好构建使用您的 Web 服务的应用程序。

## 使用 HTTP 客户端使用 Web 服务

现在我们已经构建并测试了 Northwind 服务，我们将学习如何使用 HttpClient 类及其工厂从任何 .NET 应用调用它。

### 了解 HttpClient

使用 Web 服务最简单的方法是使用 HttpClient 类。然而，许多人使用它不正确，因为它实现了 IDisposable，而 Microsoft 自己的文档也显示了其不佳的使用方式。请参阅 GitHub 仓库中的书籍链接，了解对此的更多讨论文章。

通常，当一个类型实现 IDisposable 时，您应该在 using 语句中创建它，以确保它尽快被释放。HttpClient 不同，因为它是共享的、可重入的，并且部分线程安全。

问题与底层网络套接字如何管理有关。底线是，您应该在应用程序的生命周期内为每个使用的 HTTP 端点使用一个实例。这将允许每个 HttpClient 实例具有适合其工作的端点的默认值，同时有效地管理底层网络套接字。

### 使用 HttpClientFactory 配置 HTTP 客户端

Microsoft 知道这个问题，并在 ASP.NET Core 2.1 中引入了 HttpClientFactory 以鼓励最佳实践；这就是我们将使用的技术。

在以下示例中，我们将使用 Northwind MVC 网站作为 Northwind Web API 服务的客户端。由于两者需要在 Web 服务器上同时托管，我们首先需要配置它们使用不同的端口号，如以下列表所示：

- Northwind Web API 服务将使用 HTTPS 在端口 5002 上监听。
- Northwind MVC 网站将继续使用 HTTP 在端口 5000 上监听，使用 HTTPS 在端口 5001 上监听。
