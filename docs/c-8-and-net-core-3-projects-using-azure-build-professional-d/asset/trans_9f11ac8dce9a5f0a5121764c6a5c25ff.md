

此工具无疑让人对这项技术可能实现的功能有所了解；然而，如果你对 C 或 Rust 不太熟悉，可能很难在浏览器中显示 42 之外使用它。

幸运的是，Microsoft 正在开发一项名为 Blazor 的实验性技术，它将大量工作隐藏在一个看似熟悉的项目结构背后。

## 了解 Blazor

在撰写本书时，Blazor 大约有两年历史（它最初于 2017 年进行演示）。其理念是将 WebAssembly 的概念与 Web 开发者熟悉的现有概念（例如 Razor）相结合。

> 此部分作为附录的部分原因是，仅服务器端 Blazor 随 .NET Core 3 一起发布。虽然客户端处于可用状态，但在撰写本书时，它尚未包含在官方发布版本中。

Blazor 有两种变体：客户端和服务器端。服务器端 Blazor 随 .NET Core 3 一起发布。它的工作原理是在服务器上运行 C# 代码，然后使用 SignalR 将屏幕更新发送到客户端。虽然这是一种有趣的方法，但由于它不使用 WebAssembly，我们将不在本章中进一步讨论。

要创建 Blazor 项目，你需要 Visual Studio 插件：https://marketplace.visualstudio.com/items?itemName=aspnet.blazor。

你还需要安装 Blazor 的 Visual Studio 模板；从命令行中，键入以下内容：

```
dotnet new -i Microsoft.AspNetCore.Blazor.Templates
```

> 到本书出版时，此步骤可能不再必要。

现在我们可以创建一个新的 Blazor 项目：

- 1. 从 Visual Studio 2019 创建一个新的 **Blazor 应用程序** 项目：

![](img/762492a8287f369cc1ac4621a212f193_506_0.png)

- 2. 在下一屏幕中，像平常一样选择你的 **项目名称** 和 **位置**：

![](img/762492a8287f369cc1ac4621a212f193_507_0.png)

- 3. 在下一屏幕中，你应该会看到几个选项。在本示例中，我们将创建一个客户端 Blazor 应用程序：

![](img/762492a8287f369cc1ac4621a212f193_507_1.png)

- 4. 如果你现在选择 **创建**，你将看到一个完整且可运行的 Blazor 示例。尝试运行它：你应该会看到一个功能齐全的单页应用程序。如果你曾经见过 React 模板，这个结构应该对你来说非常熟悉。

> > 尽管在撰写本书时情况属实，但到你阅读本书时，模板可能已经发生了变化。

如果你在应用程序中点击浏览，它应该很像现代单页应用程序。让我们深入了解一下。我们将从项目结构开始：

![](img/762492a8287f369cc1ac4621a212f193_508_0.png)

这个结构应该对你来说非常熟悉。值得注意的是 `.razor` 扩展名：事实上，**Blazor** 这个名称是由 **浏览器** 和 **Razor** 两个词合成的。

为了快速概览，我们不会过多深入细节，但让我们来看看 **Counter** 页面；当你运行应用程序时，你应该会看到一个允许你点击按钮并增加数值的页面：

![](img/762492a8287f369cc1ac4621a212f193_509_0.png)

这个页面能很好地让你了解语言的工作原理；让我们来看看驱动它的代码。`Counter.razor` 文件如下：

```
@page "/counter"

<h1>Counter</h1>

<p>Current count: @currentCount</p>

<button class="btn btn-primary" @onclick="@IncrementCount">Click me</button>

@code {
    int currentCount = 0;

    void IncrementCount()
    {
        currentCount++;
    }
}
```

我们有一些 HTML：页面标题、当前计数等等。然后，我们有 Razor 语法——基本上就是你在任何地方看到 `at` 符号 (@) 的地方。请注意，按钮将 `click` 事件处理程序映射到 `@code` 块内的方法。Blazor 会跟踪 DOM，当有变化时（例如 `currentCount`），页面将被刷新。

让我们再次运行应用程序，但这次，打开浏览器中的开发者工具：

![](img/762492a8287f369cc1ac4621a212f193_510_0.png)

对于大多数浏览器，可以通过按 F12 来实现。

在处理 Blazor 时，这告诉我们两点非常重要的信息。首先，查看运行应用程序时下载的所有 DLL。本质上，这是下载 Mono 运行时。如你所见，这些文件并不特别大，但在选择 Blazor 技术时这是一个需要牢记的方面。

需要注意的第二点是，点击按钮不会发起服务器调用。所有代码都在客户端执行。这就是 Blazor 的真正吸引力：你可以编写 C# 代码，并在浏览器中运行它。

我们已经快速了解了 WebAssembly 的原始形式。我们讨论了为什么 WebAssembly 是一项如此吸引人、新兴的 Web 技术。接着，我们查看了 Blazor 项目，以了解它如何简化 Web 应用程序的创建过程。

## 你可能还喜欢的其他图书

如果你喜欢这本书，可能会对 Packt 出版的这些其他图书感兴趣：

![](img/762492a8287f369cc1ac4621a212f193_511_0.png)

## 使用 C# 8 和 .NET Core 3 的软件架构实战

Gabriel Baptista, Francesco Abbruzzese

ISBN: 978-1-78980-093-7

- 克服现实世界中的架构挑战并解决设计考虑问题
- 应用分层架构、面向服务架构 (SOA) 和微服务等架构方法
- 学习使用容器、Docker 和 Kubernetes 等工具来管理微服务
- 掌握 Azure Cosmos DB 以交付多大洲解决方案
- 学习如何使用 C# 编程和维护 Azure Functions
- 了解何时将测试驱动开发 (TDD) 用作软件开发方法
- 为你的项目编写自动化功能测试用例

![](img/762492a8287f369cc1ac4621a212f193_512_0.png)

## 使用 ASP.NET Core 3 构建 RESTful Web 服务实战

Samuele Resca

ISBN: 978-1-78953-761-1

- 全面掌握 ASP.NET Core 的工作原理
- 集成第三方工具和框架，构建可维护且高效的服务
- 使用依赖注入实现模式，以减少样板代码并提高灵活性
- 使用 ASP.NET Core 的现成工具测试你的应用程序
- 使用 Docker 在隔离且自包含的环境中运行 ASP.NET Core Web 服务
- 使用 HTTPS 和基于令牌的身份验证保护你的信息
- 使用复原能力模式和消息传递技术集成多个 Web 服务

## 留下评论 - 让其他读者了解你的想法

请通过在购买该书的网站上留下评论，与其他人分享你对本书的看法。如果你从 Amazon 购买了本书，请在本书的 Amazon 页面上留下真诚的评论。这至关重要，以便其他潜在读者可以看到并使用你不偏不倚的意见来做出购买决定，我们可以了解客户对我们产品的看法，并且我们的作者可以看到你对他们与 Packt 合作创作的书名的反馈。这只需要花费你几分钟时间，但对其他潜在客户、我们的作者和 Packt 来说都很有价值。谢谢！

## 索引

.NET Core 3
benefits 59
compilation errors, fixing 52
upgrading to 49, 50, 51, 52

### A

Access Azure Storage Client, using for Photo Storage App service, configuring 323, 324, 325
Android device, setup reference link 424
application running, on physical device 424, 425, 427, 428, 429, 431
testing, on physical device 424, 425, 427, 428, 429, 431
ASP.NET Core MVC application, connecting to Cosmos DB about 86, 87, 88
MongoDbRepository class, creating 96, 97
NuGet package, adding 89, 90, 92, 95, 96
ASP.NET Core Web Application, packages code generation design 158, 159
controller, creating 170, 171
database, creating 163, 164, 165, 167
database, seeding with test data 167, 168
EF Core SQL Server 158
EF Core tools 158
installing 157
models, creating 160, 161
services, configuring 161
ASP.NET Core Web Application, publishing options custom targets 191, 192
File System 195
FTP 193
Import Profile 196, 197
Microsoft Azure App Service 181, 183, 185, 186, 188, 190
Web Deploy 194
Web Deploy Package 195
ASP.NET Core Web Application arrange/act/assert 179
creating 154, 157
deploying 181
in-memory database 179
running 171, 173, 174
testing 174, 175, 176, 178, 179, 180
testing, with FIRST 180
authorization, Stock Checker Application API 294, 295, 296, 298, 299, 300, 301, 302, 303, 304
about 292
roles 292, 293
users 292, 293
Azure Cognitive Services configuring 403, 404, 405, 406, 407
Azure Container Registry (ACR) 385, 386, 387
Azure Cosmos DB account name 81
API 81
calculator, reference link 83
geo-redundancy/multi-region writes 82
location 82
reference link 76
setting up 76, 77, 79
subscription/resource group 79, 81
Azure Functions overview 199
Azure Kubernetes Service (AKS) about 347, 349, 384, 388, 389
Azure Container Registry (ACR) 385, 386, 387
container registry 385
Docker image, building 384

