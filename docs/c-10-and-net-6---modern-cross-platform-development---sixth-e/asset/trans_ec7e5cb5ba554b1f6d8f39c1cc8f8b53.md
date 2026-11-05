
```
<script src="_framework/blazor.server.js"></script>
</body>
</html>
```

在审阅前面的标记时，请注意以下事项：

- `<div id="blazor-error-ui">` 用于显示 Blazor 错误，当发生错误时它会作为网页底部的黄色条出现
- 用于 blazor.server.js 的脚本块管理着返回服务器的 SignalR 连接

9. 在 Northwind.BlazorServer 文件夹中，打开 App.razor 并注意它为当前程序集中找到的所有组件定义了一个 Router，如以下代码所示：

```
<Router AppAssembly="@typeof(App).Assembly">
    <Found Context="routeData">
        <RouteView RouteData="@routeData"
                    DefaultLayout="@typeof(MainLayout)" />
        <FocusOnNavigate RouteData="@routeData" Selector="h1" />
    </Found>
    <NotFound>
        <PageTitle>Not found</PageTitle>
        <LayoutView Layout="@typeof(MainLayout)">
            <p>Sorry, there's nothing at this address.</p>
        </LayoutView>
    </NotFound>
</Router>
```

在审阅前面的标记时，请注意以下事项：

- 如果找到匹配的路由，则执行 RouteView，它将组件的默认布局设置为 MainLayout 并将任何路由数据参数传递给组件。
- 如果未找到匹配的路由，则执行 LayoutView，它将在 MainLayout 内部渲染内部标记（在本例中，是一个简单的段落元素，其中包含一条消息，告知访问者此地址没有任何内容）。

10. 在 Shared 文件夹中，打开 MainLayout.razor 并注意它定义了 <div> 用于包含导航菜单的侧边栏，该导航菜单由本项目中的 NavMenu.razor 组件文件实现，以及用于内容的 HTML5 元素，如 <main> 和 <article>，如以下代码所示：

```
@inherits LayoutComponentBase

<PageTitle>Northwind.BlazorServer</PageTitle>

<div class="page">
    <div class="sidebar">
    <NavMenu />
    </div>

    <main>
        <div class="top-row px-4">
            <a href="https://docs.microsoft.com/aspnet/"
            target="_blank">About</a>
        </div>
        <article class="content px-4">
            @Body
        </article>
    </main>
</div>
```

11. 在 Shared 文件夹中，打开 MainLayout.razor.css 并注意它包含组件的隔离 CSS 样式。

12. 在 Shared 文件夹中，打开 NavMenu.razor 并注意它有三个菜单项用于 Home、Counter 和 Fetch data。这些是通过使用 Microsoft 提供的名为 NavLink 的 Blazor 组件创建的，如以下标记所示：

```
<div class="top-row ps-3 navbar navbar-dark">
    <div class="container-fluid">
        <a class="navbar-brand" href="">Northwind.BlazorServer</a>
        <button title="Navigation menu" class="navbar-toggler"
            @onclick="ToggleNavMenu">
            <span class="navbar-toggler-icon"></span>
        </button>
    </div>
</div>

<div class="@NavMenuCssClass" @onclick="ToggleNavMenu">
    <nav class="flex-column">
        <div class="nav-item px-3">
            <NavLink class="nav-link" href="" Match="NavLinkMatch.All">
                <span class="oi oi-home" aria-hidden="true"></span> Home
            </NavLink>
        </div>
        <div class="nav-item px-3">
            <NavLink class="nav-link" href="counter">
                <span class="oi oi-plus" aria-hidden="true"></span> Counter
            </NavLink>
        </div>
        <div class="nav-item px-3">
            <NavLink class="nav-link" href="fetchdata">
                <span class="oi oi-list-rich" aria-hidden="true"></span> Fetch data
            </NavLink>
        </div>
    </nav>
</div>

@code {
    private bool collapseNavMenu = true;

    private string? NavMenuCssClass => collapseNavMenu ? "collapse" : null;

    private void ToggleNavMenu()
    {
        collapseNavMenu = !collapseNavMenu;
    }
}
```

13. 在 Pages 文件夹中，打开 FetchData.razor 并注意它定义了一个组件，该组件从注入的依赖项天气服务中获取天气预报，然后将它们渲染到表格中，如以下代码所示：

```
@page "/fetchdata"

<PageTitle>Weather forecast</PageTitle>

@using Northwind.BlazorServer.Data
@inject WeatherForecastService ForecastService

<h1>Weather forecast</h1>

<p>This component demonstrates fetching data from a service.</p>

@if (forecasts == null)
{
    <p><em>Loading...</em></p>
}
else
{
    <table class="table">
        <thead>
            <tr>
                <th>Date</th>
                <th>Temp. (C)</th>
                <th>Temp. (F)</th>
                <th>Summary</th>
            </tr>
        </thead>
        <tbody>
        @foreach (var forecast in forecasts)
{
    <tr>
        <td>@forecast.Date.ToShortDateString()</td>
        <td>@forecast.TemperatureC</td>
        <td>@forecast.TemperatureF</td>
        <td>@forecast.Summary</td>
    </tr>
}
    </tbody>
</table>
}

@code {
    private WeatherForecast[]? forecasts;

    protected override async Task OnInitializedAsync()
    {
        forecasts = await ForecastService.GetForecastAsync(DateTime.Now);
    }
}
```

14. 在 Data 文件夹中，打开 WeatherForecastService.cs 并注意它不是 Web API 控制器类；它只是一个普通的类，返回随机的天气数据，如以下代码所示：

```
namespace Northwind.BlazorServer.Data
{
    public class WeatherForecastService
    {
        private static readonly string[] Summaries = new[]
        {
            "Freezing", "Bracing", "Chilly", "Cool", "Mild", "Warm",
            "Balmy", "Hot", "Sweltering", "Scorching"
        };

        public Task<WeatherForecast[]> GetForecastAsync(DateTime startDate)
        {
            return Task.FromResult(Enumerable.Range(1, 5)
                .Select(index => new WeatherForecast
                {
                    Date = startDate.AddDays(index),
                    TemperatureC = Random.Shared.Next(-20, 55),
                    Summary = Summaries[Random.Shared.Next(Summaries.Length)]
                }).ToArray());
        }
    }
}
```

## 理解 CSS 和 JavaScript 隔离

Blazor 组件通常需要提供自己的 CSS 来应用样式，或提供 JavaScript 来执行纯 C# 无法完成的活动，例如访问浏览器 API。为了确保这不会与站点级 CSS 和 JavaScript 冲突，Blazor 支持 CSS 和 JavaScript 隔离。如果您有一个名为 Index.razor 的组件，只需创建一个名为 Index.razor.css 的 CSS 文件。此文件中定义样式将覆盖项目中的任何其他样式。

## 理解 Blazor 路由到页面组件

我们在 App.razor 文件中看到的 Router 组件使路由到组件成为可能。创建组件实例的标记看起来像一个 HTML 标签，其中标签名是组件类型。组件可以使用元素嵌入到网页中，例如 <Rating Stars="5" />，也可以像 Razor Page 或 MVC 控制器一样进行路由。

## 如何定义可路由的页面组件

要创建可路由的页面组件，请在组件的 .razor 文件顶部添加 @page 指令，如以下标记所示：

```
@page "customers"
```

前面的代码等同于用 [Route] 属性装饰的 MVC 控制器，如以下代码所示：

```
[Route("customers")]
public class CustomersController
{
```

Router 组件会扫描其 AppAssembly 参数中指定的程序集，查找用 [Route] 属性装饰的组件并注册其 URL 路径。

任何单页组件都可以有多个 @page 指令来注册多个路由。

在运行时，页面组件会与您指定的任何特定布局合并，就像 MVC 视图或 Razor Page 一样。默认情况下，Blazor Server 项目模板将 MainLayout.razor 定义为页面组件的布局。

> **良好实践：** 按惯例，将可路由的页面组件放在 Pages 文件夹中。

## 如何导航 Blazor 路由

Microsoft 提供了一个名为 NavigationManager 的依赖服务，它理解 Blazor 路由和 NavLink 组件。
