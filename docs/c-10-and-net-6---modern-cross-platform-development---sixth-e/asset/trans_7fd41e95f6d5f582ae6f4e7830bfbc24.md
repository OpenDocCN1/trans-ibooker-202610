
- **UseEndpoints**：添加中间件以执行从管道早期决策中生成响应的操作。以下子列表中添加了两个端点：
  - **MapRazorPages**：添加中间件，将诸如 `/suppliers` 这样的 URL 路径映射到 `/Pages` 文件夹中名为 `suppliers.cshtml` 的 Razor 页面文件，并将结果作为 HTTP 响应返回。
  - **MapGet**：添加中间件，将诸如 `/hello` 这样的 URL 路径映射到内联委托，该委托直接将纯文本写入 HTTP 响应。

## 可视化 HTTP 管道

HTTP 请求和响应管道可以可视化为一系列请求委托的序列，按顺序依次调用，如下简化图所示，该图排除了某些中间件委托，例如 UseHsts：

![](img/c63b00dca7192cc58cfb9e1f1e64f483_640_0.png)

图 14.16：HTTP 请求和响应管道

如前所述，UseRouting 和 UseEndpoints 方法必须一起使用。虽然在 UseEndpoints 中编写了定义映射路由（如 /hello）的代码，但关于传入 HTTP 请求 URL 路径是否匹配以及因此执行哪个端点的决策是在管道中的 UseRouting 点做出的。

## 实现匿名内联委托作为中间件

可以将委托指定为内联匿名方法。我们将注册一个在为端点做出路由决策后插入管道的委托。

它将输出所选的端点，并处理一个特定路由：/bonjour。如果匹配该路由，它将使用纯文本响应，而不会进一步调用管道：

- 1. 在 Startup.cs 中，静态导入 Console，如以下代码所示：
- 2. 在调用 UseRouting 之后、在调用 UseHttpsRedirection 之前添加语句，以使用匿名方法作为中间件委托，如以下代码所示：
- 3. 启动网站。
- 4. 在 Chrome 中，导航到 https://localhost:5001/，查看控制台输出并注意端点路由 / 匹配成功，它被处理为 /index，并且 Index.cshtml Razor 页面被执行以返回响应，如以下输出所示：
- 5. 导航到 https://localhost:5001/suppliers 并注意可以看到端点路由 /Suppliers 匹配成功，并且 Suppliers.cshtml Razor 页面被执行以返回响应，如以下输出所示：

```
using static System.Console;
```

```
app.Use(async (HttpContext context, Func<Task> next) =>
{
    RouteEndpoint? rep = context.GetEndpoint() as RouteEndpoint;
    if (rep is not null)
    {
        WriteLine($"Endpoint name: {rep.DisplayName}");
        WriteLine($"Endpoint route pattern: {rep.RoutePattern.RawText}");
    }

    if (context.Request.Path == "/bonjour")
    {
        // in the case of a match on URL path, this becomes a terminating
        // delegate that returns so does not call the next delegate
        await context.Response.WriteAsync("Bonjour Monde!");
        return;
    }

    // we could modify the request before calling the next delegate
    await next();
    // we could modify the response after calling the next delegate
});
```

```
Endpoint name: /index
Endpoint route pattern:
```

```
Endpoint name: /Suppliers
Endpoint route pattern: Suppliers
```

- 6. 导航到 https://localhost:5001/index 并注意端点路由 /index 匹配成功，并且 Index.cshtml Razor 页面被执行以返回响应，如以下输出所示：

```
Endpoint name: /index
Endpoint route pattern: index
```

- 7. 导航到 https://localhost:5001/index.html 并注意控制台没有输出，因为没有匹配端点路由，但匹配到了静态文件，因此将其作为响应返回。

- 8. 导航到 https://localhost:5001/bonjour 并注意控制台没有输出，因为没有匹配端点路由。相反，我们的委托匹配了 /bonjour，直接写入响应流并返回，未进行进一步处理。

- 9. 关闭 Chrome 并关闭 Web 服务器。

## 练习与探索

通过回答一些问题来测试你的知识和理解，进行一些实践操作，并通过深入研究探索本章主题。

## 练习 14.1 – 测试你的知识

回答以下问题：

- 1. 列出 HTTP 请求中可以指定的六个方法名称。
- 2. 列出 HTTP 响应中可以返回的六个状态码及其描述。
- 3. 在 ASP.NET Core 中，Startup 类用于什么？
- 4. 缩写 HSTS 代表什么，它做什么？
- 5. 如何为网站启用静态 HTML 页面？
- 6. 如何将 C# 代码混合到 HTML 中间以创建动态页面？
- 7. 如何为 Razor Pages 定义共享布局？
- 8. 如何在 Razor Page 中将标记与代码隐藏分离？
- 9. 如何配置 Entity Framework Core 数据上下文以与 ASP.NET Core 网站一起使用？
- 10. 如何在 ASP.NET Core 2.2 或更高版本中重用 Razor Pages？

## 练习 14.2 – 实践构建数据驱动的网页

向 Northwind.Web 网站添加一个 Razor 页面，使用户能够按国家/地区查看分组的客户列表。当用户点击客户记录时，他们将看到显示该客户完整联系详情的页面，以及他们的订单列表。

## 练习 14.3 – 实践为控制台应用构建网页

将前面章节中的一些控制台应用重新实现为 Razor 页面，例如，来自第 4 章《编写、调试和测试函数》，提供 Web 用户界面以输出乘法表、计算税、生成阶乘和斐波那契数列。

## 练习 14.4 – 探索主题

使用以下页面上的链接了解本章涵盖主题的更多信息：

```
https://github.com/markjprice/cs10dotnet6/blob/main/book-links.md#chapter-14---building-websites-using-aspnet-core-razor-pages
```

## 总结

在本章中，你学习了使用 HTTP 进行 Web 开发的基础知识，如何构建返回静态文件的简单网站，以及如何使用 ASP.NET Core Razor Pages 和 Entity Framework Core 创建从数据库信息动态生成的网页。

我们回顾了 HTTP 请求和响应管道，辅助扩展方法的作用，以及如何添加自己的中间件来影响处理。

在下一章中，你将学习如何使用 ASP.NET Core MVC 构建更复杂的网站，它将构建网站的技术问题分离为模型、视图和控制器，以便更易于管理。

## 15

## 使用模型-视图-控制器模式构建网站

本章介绍使用 Microsoft ASP.NET Core MVC 在服务器端使用现代 HTTP 架构构建网站，包括启动配置、身份验证、授权、路由、请求和响应管道，以及构成 ASP.NET Core MVC 项目的模型、视图和控制器。

本章将涵盖以下主题：

- 设置 ASP.NET Core MVC 网站
- 探索 ASP.NET Core MVC 网站
- 自定义 ASP.NET Core MVC 网站
- 查询数据库并使用显示模板
- 使用异步任务提高可扩展性

### 设置 ASP.NET Core MVC 网站

ASP.NET Core Razor Pages 对于简单网站非常好。对于更复杂的网站，最好有一个更正式的结构来管理这种复杂性。

这就是模型-视图-控制器（MVC）设计模式派上用场的地方。它使用像 Razor Pages 这样的技术，但允许在技术问题之间实现更清晰的分离，如以下列表所示：

- **Models**：表示网站上使用的数据实体和视图模型的类。
- **Views**：Razor 文件，即 .cshtml 文件，将视图模型中的数据渲染为 HTML 网页。Blazor 使用 .razor 文件扩展名，但不要将其与 Razor 文件混淆！
