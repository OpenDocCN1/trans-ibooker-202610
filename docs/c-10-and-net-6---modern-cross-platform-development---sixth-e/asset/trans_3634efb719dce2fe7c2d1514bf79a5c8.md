
name: "Minimal.WebApi");

HttpRequestMessage request = new(
    method: HttpMethod.Get, requestUri: "api/weather");

HttpResponseMessage response = await client.SendAsync(request);

ViewData["weather"] = await response.Content
    .ReadFromJsonAsync<WeatherForecast[]>();
}
catch (Exception ex)
{
    _logger.LogWarning($"The Minimal.WebApi service is not responding.
Exception: {ex.Message}");
    ViewData["weather"] = Enumerable.Empty<WeatherForecast>().ToArray();
}
```

4. 在 Views/Home 中的 Index.cshtml 中，导入 Northwind.Common 命名空间，然后在顶部的代码块中从 ViewData 字典获取天气预报，如以下标记所示：

```
@{
    ViewData["Title"] = "Home Page";
    string currentItem = "";
    WeatherForecast[]? weather = ViewData["weather"] as WeatherForecast[];
}
```

5. 在第一个 <div> 中，在渲染当前时间之后，添加标记以枚举天气预报（如果有的话），并将其渲染为表格，如以下标记所示：

```
<p>
    <h4>Five-Day Weather Forecast</h4>
    @if ((weather is null) || (!weather.Any()))
    {
        <p>No weather forecasts found.</p>
    }
    else
    {
    <table class="table table-info">
        <tr>
        @foreach (WeatherForecast w in weather)
        {
            <td>@w.Date.ToString("ddd d MMM") will be @w.Summary</td>
        }
        </tr>
    </table>
    }
</p>
```

6. 启动 Minimal.WebApi 服务。

7. 启动 Northwind.Mvc 网站。

8. 导航到 https://localhost:5001/，并注意天气预报，如图 16.20 所示：

![](img/c63b00dca7192cc58cfb9e1f1e64f483_746_0.png)

图 16.20：Northwind 网站主页上的五天天气预报

9. 查看 MVC 网站的命令提示符或终端，并注意信息消息，表明请求已在约 83ms 内发送到最小 API Web 服务 api/weather 端点，如以下输出所示：

```
info: System.Net.Http.HttpClient.Minimal.WebApi.LogicalHandler[100]
  Start processing HTTP request GET https://localhost:5003/api/weather
info: System.Net.Http.HttpClient.Minimal.WebApi.ClientHandler[100]
  Sending HTTP request GET https://localhost:5003/api/weather
info: System.Net.Http.HttpClient.Minimal.WebApi.ClientHandler[101]
  Received HTTP response headers after 76.8963ms - 200
info: System.Net.Http.HttpClient.Minimal.WebApi.LogicalHandler[101]
  End processing HTTP request after 82.951ms - 200
```

10. 停止 Minimal.WebApi 服务，刷新浏览器，并注意几秒后 MVC 网站主页将显示但没有天气预报。

11. 关闭 Chrome 并关闭 Web 服务器。

## 练习与探索

通过回答问题测试你的知识和理解，进行一些实践操作，并通过更深入的研究探索本章主题。

## 练习 16.1 – 测试你的知识

回答以下问题：

- 1. 创建 ASP.NET Core Web API 服务的控制器类时应继承哪个类？
- 2. 如果你用 [ApiController] 属性装饰控制器类以获得默认行为（如对无效模型自动返回 400 响应），还必须做什么？
- 3. 必须做什么来指定响应 HTTP 请求时将执行哪个控制器操作方法？

- 4. 必须做什么来指定调用操作方法时应期望什么响应？
- 5. 列出三种可以调用以返回不同状态码响应的 方法。
- 6. 列出四种可以测试 Web 服务的方法。
- 7. 为什么即使 HttpClient 实现了 IDisposable 接口，也不应在完成时用 using 语句将其包装以释放它，而应使用什么替代方案？
- 8. CORS 缩写代表什么，为什么在 Web 服务中启用它很重要？
- 9. 如何使用 ASP.NET Core 2.2 及更高版本使客户端能够检测你的 Web 服务是否健康？
- 10. 端点路由提供了什么好处？

## 练习 16.2 – 使用 HttpClient 练习创建和删除客户

将 Northwind.Mvc 网站项目扩展为具有页面，让访客可以填写表单创建新客户，或搜索客户然后删除他们。MVC 控制器应调用 Northwind Web 服务来创建和删除客户。

## 练习 16.3 – 探索主题

使用以下页面上的链接了解本章所涵盖主题的更多详细信息：

```
https://github.com/markjprice/cs10dotnet6/blob/main/book-links.md#chapter-16---building-and-consuming-web-services
```

## 摘要

本章中，你学习了如何构建 ASP.NET Core Web API 服务，该服务可以被任何能够发出 HTTP 请求并处理 HTTP 响应的平台上的任何应用程序调用。

你还学习了如何使用 Swagger 测试和记录 Web 服务 API，以及如何高效地使用服务。

在下一章中，你将学习使用 Blazor 构建用户界面，Blazor 是 Microsoft 的酷炫新组件技术，使开发人员能够使用 C# 而不是 JavaScript 为网站构建客户端单页应用程序（SPA）、桌面混合应用程序以及潜在的移动应用程序。

## 17

## 构建用户界面
使用 Blazor

本章介绍使用 Blazor 构建用户界面。我将描述 Blazor 的不同风格及其优缺点。

你将学习如何构建 Blazor 组件，使其代码可以在 Web 服务器或 Web 浏览器中执行。当使用 Blazor Server 托管时，它使用 SignalR 将用户界面所需的更新通信到浏览器中。当使用 Blazor WebAssembly 托管时，组件在客户端执行其代码，并且必须发出 HTTP 调用与服务器交互。

本章将涵盖以下主题：

- 了解 Blazor
- 比较 Blazor 项目模板
- 使用 Blazor Server 构建组件
- 为 Blazor 组件抽象服务
- 使用 Blazor WebAssembly 构建组件
- 改进 Blazor WebAssembly 应用

## 了解 Blazor

Blazor 让你可以使用 C# 而不是 JavaScript 构建共享组件和交互式 Web 用户界面。2019 年 4 月，Microsoft 宣布 Blazor“不再是实验性的，我们致力于将其作为受支持的 Web UI 框架发布，包括在 WebAssembly 上在浏览器中客户端运行的支持。”Blazor 在所有现代浏览器上均受支持。

## JavaScript 及相关技术

传统上，任何需要在 Web 浏览器中执行的代码都是使用 JavaScript 编程语言或更高级别的技术编写的，该技术 **转译**（transforms or compiles）为 JavaScript。这是因为所有浏览器大约二十年来都支持 JavaScript，因此它已成为在客户端实现业务逻辑的最低公分母。

然而 JavaScript 也有一些问题。尽管它与 C# 和 Java 等 C 风格语言有表面相似性，但一旦深入探究，其实际非常不同。它是一种动态类型的伪函数式语言，使用原型而不是类继承来实现对象重用。它看起来像是人类，但当它被揭露实际上是一个斯克鲁尔人时，你会感到惊讶。

如果我们能在 Web 浏览器中使用与服务器端相同的语言和库，不是很好吗？

## Silverlight – 使用插件的 C# 和 .NET

Microsoft 曾尝试使用名为 Silverlight 的技术实现这一目标。当 Silverlight 2.0 于 2008 年发布时，C# 和 .NET 开发人员可以使用他们的技能构建库和视觉组件，这些组件由 Silverlight 插件在 Web 浏览器中执行。

到 2011 年和 Silverlight 5.0 时，苹果凭借 iPhone 的成功以及史蒂夫·乔布斯对 Flash 等浏览器插件的厌恶，最终导致 Microsoft 放弃 Silverlight，因为与 Flash 一样，Silverlight 被禁止在 iPhone 和 iPad 上使用。

## WebAssembly – Blazor 的目标

浏览器中的最新发展给了 Microsoft 再次尝试的机会。2017 年，**WebAssembly 共识**完成，所有主要浏览器现在都支持它：Chromium（Chrome、Edge、Opera、Brave）、Firefox 和 WebKit（Safari）。Blazor 不受 Microsoft 的 Internet Explorer 支持，因为它是一个旧版 Web 浏览器。

**WebAssembly (Wasm)** 是一种虚拟机的二进制指令格式，提供了一种在接近原生速度下在 Web 上运行用多种语言编写的代码的方法。Wasm 被设计为 C# 等高级语言编译的可移植目标。

## 了解 Blazor 托管模型

Blazor 是一个单一的编程或应用模型，具有多种托管模型：

- **Blazor Server** 在服务器端运行，因此你编写的 C# 代码可以完全访问业务逻辑可能需要的全部资源，而无需进行身份验证。然后它使用 SignalR 将用户界面更新通信到客户端。
