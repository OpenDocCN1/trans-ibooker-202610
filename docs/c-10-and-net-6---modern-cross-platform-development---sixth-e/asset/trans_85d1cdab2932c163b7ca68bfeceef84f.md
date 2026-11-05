
```
{
    public void ConfigureServices(IServiceCollection services)
    {
    }

    public void Configure(
        IApplicationBuilder app, IWebHostEnvironment env)
    {
        if (!env.IsDevelopment())
        {
            app.UseHsts();
        }

        app.UseRouting(); // start endpoint routing

        app.UseHttpsRedirection();

        app.UseEndpoints(endpoints =>
        {
            endpoints.MapGet("/", () => "Hello World!");
        });
    }
}
```

注意以下关于代码的说明：

- ConfigureServices 方法目前为空，因为我们目前还不需要添加任何依赖服务。
- Configure 方法设置 HTTP 请求管道并启用端点路由的使用。它配置了一个路由端点，等待使用相同的映射对根路径 / 的每个 HTTP GET 请求进行请求，并通过返回纯文本 "Hello World!" 来响应这些请求。我们将在本章末尾学习有关路由端点及其好处。

现在我们必须指定要在应用程序入口点中使用 Startup 类。

3. 修改 Program.cs，如以下代码所示：

```
using Northwind.Web; // Startup

Host.CreateDefaultBuilder(args)
    .ConfigureWebHostDefaults(webBuilder =>
    {
        webBuilder.UseStartup<Startup>();
    }).Build().Run();

Console.WriteLine("This executes after the web server has stopped!");
```

- 4. 启动网站并注意其行为与之前相同。
- 5. 关闭 Web 服务器。

> 在本书中我们创建的所有其他网站和服务项目中，我们将使用 .NET 6 项目模板创建的单个 Program.cs 文件。如果你喜欢使用 Startup.cs 的方式，那么你将在本章中看到如何使用它。

## 启用网站提供静态内容

只返回单一纯文本消息的网站并不是很有用！

至少，它应该返回静态 HTML 页面、网页用于样式的 CSS 以及任何其他静态资源，例如图像和视频。

按照惯例，这些文件应存储在名为 wwwroot 的目录中，以将其与网站项目的动态执行部分分开。

## 创建静态文件文件夹和网页

你现在将为静态网站资源创建一个文件夹，并创建一个使用 Bootstrap 进行样式设置的基本 index 页面：

- 1. 在 Northwind.Web 项目/文件夹中，创建一个名为 wwwroot 的文件夹。
- 2. 在 wwwroot 文件夹中添加一个名为 index.html 的新 HTML 页面文件。
- 3. 修改其内容以链接到 CDN 托管的 Bootstrap 用于样式设置，并使用诸如设置视口等现代良好实践，如以下标记所示：

```
<!doctype html>
<html lang="en">

<head>
    <!-- Required meta tags -->
    <meta charset="utf-8" />
    <meta name="viewport" content=
    "width=device-width, initial-scale=1 " />

    <!-- Bootstrap CSS -->
    <link href=
    "https://cdn.jsdelivr.net/npm/bootstrap@5.1.0/dist/css/bootstrap.min.css"
    rel="stylesheet" integrity="sha384-KyZXEAg3QhqLMpG8r+8fhAXLRk2vvoC2f3B09zV Xn8CA5QIVFZ0J3BCsw2P0p/We" crossorigin="anonymous">

    <title>Welcome ASP.NET Core!</title>
</head>
```

```
<body>
    <div class="container">
        <div class="jumbotron">
            <h1 class="display-3">Welcome to Northwind B2B</h1>
            <p class="lead">We supply products to our customers.</p>
            <hr />
            <h2>This is a static HTML page.</h2>
            <p>Our customers include restaurants, hotels, and cruise lines.</p>
            <p>
                <a class="btn btn-primary"
                   href="https://www.asp.net/">Learn more</a>
            </p>
        </div>
    </div>
</body>

</html>
```

> 良好实践：要获取 Bootstrap 的最新 <link> 元素，请从以下链接的文档中复制并粘贴：https://getbootstrap.com/docs/5.0/getting-started/introduction/#starter-template。

## 启用静态文件和默认文件

如果你现在启动网站并在地址栏中输入 http://localhost:5000/index.html，网站将返回 404 Not Found 错误，提示未找到网页。要使网站能够返回 index.html 等静态文件，我们必须显式配置该功能。

即使我们启用了静态文件，如果你启动网站并在地址栏中输入 http://localhost:5000/，网站仍会返回 404 Not Found 错误，因为如果未请求命名文件，Web 服务器不知道默认返回什么。

你现在将启用静态文件，显式配置默认文件，并更改注册的 URL 路径，使其返回纯文本 Hello World! 响应：

- 1. 在 Startup.cs 中，在 Configure 方法中，在启用 HTTPS 重定向后添加语句以启用静态文件和默认文件，并修改映射 GET 请求以返回 Hello World! 纯文本响应的语句，使其仅响应 URL 路径 /hello，如以下代码中突出显示所示：

```
app.UseHttpsRedirection();

app.UseDefaultFiles(); // index.html, default.html, and so on
```

```
app.UseStaticFiles();

app.UseEndpoints(endpoints =>
{
    endpoints.MapGet("/hello", () => "Hello World!");
});
```

![](img/c63b00dca7192cc58cfb9e1f1e64f483_616_0.png)

> 对 UseDefaultFiles 的调用必须在对 UseStaticFiles 的调用之前，否则将无法工作！你将在本章末尾了解有关中间件和端点路由顺序的更多信息。

- 2. 启动网站。
- 3. 启动 **Chrome** 并显示 **开发者工具**。
- 4. 在 Chrome 中，输入 http://localhost:5000/ 并注意你被重定向到端口 5001 上的 HTTPS 地址，并且 index.html 文件现在通过该安全连接返回，因为它是该网站的可能默认文件之一。
- 5. 在 **开发者工具** 中，注意对 Bootstrap 样式表的请求。
- 6. 在 Chrome 中，输入 http://localhost:5000/hello 并注意它像之前一样返回纯文本 `Hello World!`。
- 7. 关闭 Chrome 并关闭 Web 服务器。

如果所有网页都是静态的，即它们只能由网页编辑器手动更改，那么我们的网站编程工作就完成了。但几乎所有网站都需要动态内容，这意味着通过执行代码在运行时生成的网页。

实现此目的最简单的方法是使用名为 **Razor Pages** 的 ASP.NET Core 功能。

## 探索 ASP.NET Core Razor Pages

ASP.NET Core Razor Pages 允许开发人员轻松地将 C# 代码语句与 HTML 标记混合，以使生成的网页具有动态性。这就是它们使用 .cshtml 文件扩展名的原因。

按照惯例，ASP.NET Core 会在名为 Pages 的文件夹中查找 Razor Pages。

## 启用 Razor Pages

你现在将把静态 HTML 页面复制并更改为动态 Razor 页面，然后添加并启用 Razor Pages 服务：

- 1. 在 `Northwind.Web` 项目文件夹中，创建一个名为 Pages 的文件夹。
- 2. 将 `index.html` 文件复制到 Pages 文件夹中。

3. 对于 Pages 文件夹中的文件，将文件扩展名从 .html 重命名为 .cshtml。

4. 删除说这是静态 HTML 页面的 <h2> 元素。

5. 在 Startup.cs 中，在 ConfigureServices 方法中，添加一条语句以将 ASP.NET Core Razor Pages 及其相关服务（如模型绑定、授权、防伪、视图和标记助手）添加到生成器中，如以下代码所示：

```
services.AddRazorPages();
```

6. 在 Startup.cs 中，在 Configure 方法中，在配置使用端点的部分中，添加一条语句以调用 MapRazorPages 方法，如以下代码中突出显示所示：

```
app.UseEndpoints(endpoints =>
{
    endpoints.MapRazorPages();

    endpoints.MapGet("/hello", () => "Hello World!");
});
```

## 向 Razor 页面添加代码

在网页的 HTML 标记中，Razor 语法由 @ 符号表示。Razor Pages 可以描述如下：

- 它们要求文件顶部有 @page 指令。
- 它们可以可选地具有 @functions 部分，定义以下任何内容：
  - 用于存储数据值的属性，如类定义中。名为 Model 的该类的实例会自动实例化，可以在特殊方法中设置其属性，并且可以在 HTML 中获取属性值。
  - 当发出 HTTP 请求（如 GET、POST 和 DELETE）时执行的名为 OnGet、OnPost、OnDelete 等的方法。

现在让我们将静态 HTML 页面转换为 Razor 页面：

- 1. 在 Pages 文件夹中，打开 index.cshtml。
- 2. 在文件顶部添加 @page 语句。
- 3. 在 @page 语句之后，添加一个 @functions 语句块。
- 4. 定义一个属性以将当前星期几的名称作为字符串值存储。
- 5. 定义一个在对页面发出 HTTP GET 请求时执行以设置 DayName 的方法，如以下代码所示：

```
@page

@functions
{
```
