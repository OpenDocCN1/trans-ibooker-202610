

├── Pages
│   ├── Counter.razor
│   ├── FetchData.razor
│   └── Index.razor
├── Program.cs
├── Properties
│   └── launchSettings.json
├── Shared
│   ├── MainLayout.razor
│   ├── MainLayout.razor.css
│   ├── NavMenu.razor
│   ├── NavMenu.razor.css
│   └── SurveyPrompt.razor
├── _Imports.razor
├── bin
... <====== 删除了大量 .NET 依赖项
├── blazor-web.csproj
├── obj
... <====== 删除了大量 .NET 依赖项
└── wwwroot
    ├── css
    │   ├── app.css
    │   ├── bootstrap
    │   │   ├── bootstrap.min.css
    │   │   └── bootstrap.min.css.map
    │   └── open-iconic
    │       ├── FONT-LICENSE
    │       ├── ICON-LICENSE
    │       ├── README.md
    │       └── font
    │           ├── css
    │           │   └── open-iconic-bootstrap.min.css
    │           └── fonts
    │               ├── open-iconic.eot
    │               ├── open-iconic.otf
    │               ├── open-iconic.svg
    │               ├── open-iconic.ttf
    │               └── open-iconic.woff
    ├── favicon.ico
    ├── index.html
    └── sample-data
        └── weather.json

在应用特定的文件和大量 .NET 依赖项之后，我们看到一个名为 wwwroot 的目录，它代表了各种资产和资源，这些资产和资源需要被提供以完成应用程序。这包括字体、一些示例 JSON 数据，以及应用程序的精简版 Bootstrap 基础。

我无法对 Razor 组件及其包含的所有内容进行详细的教程，但我想介绍应用程序的基础知识，以便你能够把握其功能。主视图存储在 Pages 目录中。共享布局存储在 Shared 目录中。示例 13-6 中显示的主应用程序文件位于主项目目录中的 App.razor。

## 示例 13-6. 在 Razor 中定义的高层级应用程序组件

```
<Router AppAssembly="@typeof(Program).Assembly" PreferExactMatches="@true">
    <Found Context="routeData">
        <RouteView RouteData="@routeData" DefaultLayout="@typeof(MainLayout)" />
    </Found>
    <NotFound>
        <LayoutView Layout="@typeof(MainLayout)">
            <p>Sorry, there's nothing at this address.</p>
        </LayoutView>
    </NotFound>
</Router>
```

这代表了应用程序的高层级细节，包括被标识为 MainLayout 的默认布局。如果它无法找到可用的布局，请注意它有一个默认消息。这是一个我们期望通过 HTTP 提供服务的 SPA，因此我们需要一种将端点路由到页面的机制。我们很快就会看到这些细节。

在示例 13-7 中，我们有来自 Shared/MainLayout.razor 的默认布局。我们可以看到它继承自 LayoutComponentBase，并由侧边栏左侧的 <div> 元素中的 NavMenu 组成。这突出了 Razor 组件混合了 C# 对象交互和 HTML 布局。它是一种有点奇怪的语法，但一旦你习惯了，就不会特别繁重。

## 示例 13-7. 默认布局是一个共享模板

```
@inherits LayoutComponentBase

<div class="page">
<div class="sidebar">
    <NavMenu />
</div>

<div class="main">
    <div class="top-row px-4">
        <a href="http://blazor.net" target="_blank" class="ml-md-auto">About</a>
    </div>

    <div class="content px-4">
        @Body
    </div>
</div>
</div>
```

按照惯例，文件 *Index.razor* 包含着陆页，就像大多数网站由一个名为 *index.html* 的文件定义一样。在示例 13-8 中，当默认路由被调用时，此组件的主体将被插入到 `MainLayout` 中的 `@Body` 位置。

## 示例 13-8. 默认路由指向 Pages 目录中的 Index.razor

```
@page "/"

<h1>Hello, world!</h1>

Welcome to your new app.

<SurveyPrompt Title="How is Blazor working for you?" />
```

我们看到上面的第一个 `@page` 指令，它指示主应用程序将默认的 "/" HTTP 请求指向此渲染页面。我们还看到典型的友好问候以 HTML 形式呈现，以及一个用于调查的嵌入组件。

由于此页面及其资源需要通过 HTTP 提供，我们将使用以下命令启动它。

```
brian@tweezer ~/g/w/s/blazor-web> dotnet run
Building...
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: https://localhost:5001
```

```
info: Microsoft.Hosting.Lifetime[0]
    Now listening on: http://localhost:5000
info: Microsoft.Hosting.Lifetime[0]
    Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
    Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
    Content root path: /Users/brian/src/blazor-web
```

在图 13-5 中，你将看到如果你将浏览器指向 http://localhost:5000 时生成的输出。如果你想查看所有加载的 .NET 程序集和运行时，请先打开网络视图。注意，如果你点击调查链接，它会带你到一个真正的 SurveyMonkey 调查！

![](img/16b7990eb0dbf589db177bdbd2f4a189_290_0.png)

图 13-5. 正在运行的生成的 Blazor WebAssembly 应用程序

路由到只读渲染的 HTML 片段很酷，但如何维护一些状态并使用非默认路由呢？侧边栏左侧的 Counter 链接将带你到从 *Pages/Counter.razor* 文件渲染的视图，如示例 13-9 所示。

## 示例 13-9. 具有有状态字段和交互式 UI 元素的页面

```
@page "/counter"

<h1>Counter</h1>

<p>Current count: @currentCount</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>

@code {
    private int currentCount = 0;

    private void IncrementCount()
    {
        currentCount++;
    }
}
```

我们在此代码示例中看到了一些更有趣的行为。首先，你会注意到 `@page` 指令指向的不是默认路由，因此你可以想象为你的 SPA 设置多个视图。你看到了更多奇怪的语法，但这次我们定义了一个 `<button>` 元素以及当用户按下它时我们希望调用的行为。在这种情况下，`@onclick` 提示告诉我们我们想要调用组件上的一个名为 `IncrementCount` 的函数。这是在下面的 `@code` 块中定义的，就像组件将维护的状态字段 `currentCount` 一样。它被初始化为 0，但每次按下按钮时都会递增。

虽然你可能不选择像这样直接在 *.Razor* 文件中嵌入组件，但至少可以这样做，并且使得在视图中维护和反映状态变得相当简单。点击 `Counter` 链接并亲眼看看。停止服务器并尝试在每次按下按钮时使用 `Console.WriteLine` 打印一条消息到控制台。花一分钟惊叹一下，与我们早期关于 WebAssembly 不支持字符串的抱怨相比，你在这本书中已经取得了多大的进步。

我在这里想指出的最后一件事是 Pages/FetchData.razor 文件的内容，如示例 13-10 所示。这包括了对于更有用的 SPA 你需要处理的许多细节，例如获取远程数据、在视图中使用条件逻辑，以及迭代你获取的数据。

## 示例 13-10. 具有依赖注入 HttpClient 的更复杂的页面

```
@page "/fetchdata"
@inject HttpClient Http

<h1>Weather forecast</h1>

<p>This component demonstrates fetching data from the server.</p>

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
```
