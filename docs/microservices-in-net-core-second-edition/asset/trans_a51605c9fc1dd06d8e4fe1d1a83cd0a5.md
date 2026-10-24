
NOTE 使用事件源实现这种事件驱动架构将在第5章中介绍。

## 1.7.3 完整视图

总共有六个不同的微服务参与处理向购物车添加商品请求，如图1.8所示。这些微服务中没有任何一个了解其他微服务的内部结构。其中五个拥有专用的私有数据存储区，仅服务于其自身用途。部分处理在用户请求的上下文中同步进行，部分则异步进行。

![](img/608267d7803dc57a1a896ad121f37868_41_0.png)

图1.8 当用户将商品添加到购物车时，前端向API网关微服务发起请求，该微服务与其他微服务协作以完成请求。在处理过程中，微服务可能会引发事件，其他微服务可以订阅并异步处理这些事件。

这是一个典型的微服务系统。请求通过多个微服务的协作来处理，每个微服务只负责单一职责，并尽可能保持与其他微服务的独立性。

现在，我们从一个高层角度具体查看了微服务系统如何处理用户请求，是时候简要了解一下基于 .NET 的微服务技术栈了。

## 1.8 .NET 微服务技术栈

是时候来介绍本书中使用的最核心技术：ASP.NET、ASP.NET MVC 和 Kubernetes 了。

### 1.8.1 ASP.NET 和 MVC

ASP.NET 是微软的 Web 平台，具有多项与本书中我们将创建的微服务相关的优势：

- 跨平台——ASP.NET 基于 .NET 5，可在 Linux、Windows 等平台上运行。这在决定在哪里运行微服务时提供了一定的灵活性。我们将在 Kubernetes 集群中的 Linux 容器内运行它们，但微服务也可以在不同的容器编排器、Windows 容器，或 Windows 或 Linux 服务器上运行。
- 快速——ASP.NET 很快，底层的 .NET 5 平台⁸ 同样如此。这为构建快速的微服务奠定了良好的基础。
- 微服务是一等公民——ASP.NET 的主要目标之一是微服务，这意味着该平台已经实现了我们需要自行实现的一些重要基础功能，例如请求日志记录和监控支持。
- 中间件管道——中间件是一种轻量级、可组合且灵活的方式来构建处理传入 HTTP 请求的管道。我们的微服务将在其中间件管道的末端使用 ASP.NET MVC。
- 可测试——我们将利用 ASP.NET 的 TestHost 测试助手，以有利于测试驱动开发中“由外到内”风格的方式，对微服务进行彻底测试。

在 ASP.NET 平台之上，我们将使用微软的 ASP.NET MVC⁹ Web 框架，它在实现 HTTP 端点处理器时提供了额外的便利性。

### 1.8.2 Kubernetes

Kubernetes 是一个容器编排器，我们将使用它来运行微服务。它允许我们自动化部署，并在某种程度上管理我们的微服务。在第3章中，我们将在 Azure 中创建一个 Kubernetes 集群，并查看如何

⁸ 根据 TechEmpower 基准测试：https://www.techempower.com/benchmarks/。

⁹ 在整个书中，我将经常把 ASP.NET MVC 简称为 MVC。

将我们的微服务部署到 Kubernetes 集群。随后，我们将继续将微服务部署到 Kubernetes。

### 1.8.3 设置开发环境

在开始编写第一个微服务代码之前，你需要具备合适的工具。要跟随本书中的示例，你需要四个主要工具：IDE、.NET 命令行、HTTP 工具和 Docker。

首先，你需要一个用于创建 ASP.NET 应用程序的开发环境。三种最常见的选择如下：

- Visual Studio——Visual Studio 是一个功能完备的 IDE，对编写和调试 ASP.NET 应用程序提供了很好的支持。它仅支持 Windows，可能是 ASP.NET 开发中最广泛使用的 IDE。
- Visual Studio Code——Visual Studio Code 是一个功能丰富的编辑器，自带 C# 插件，使其成为编写 ASP.NET 应用程序的得力工具。它轻量级，并支持 Windows、Linux 和 Mac。
- JetBrains Rider——Rider 是另一个功能完备的 IDE，对编写和调试 ASP.NET 应用程序提供了强大的支持。Rider 在 Windows、Linux 和 Mac 上均可运行。

所有这些工具都支持开发、运行和调试 ASP.NET 应用程序。它们都提供了带有 IntelliSense 和重构支持的 C# 编辑器。它们都支持 ASP.NET，并且都可以启动和调试 ASP.NET 应用程序。

其次，你需要拥有 .NET 命令行工具的一个版本。如果 IDE 没有附带该工具，请按照 http://dot.net 上的说明安装 .NET。这将为你提供一个名为 `dotnet` 的命令行工具，用于执行与微服务相关的各种任务，包括还原、构建、创建 NuGet 包以及运行微服务。在整个书中，我将使用 `dotnet` 命令行来完成这些任务，但如果你愿意，也可以在 IDE 中完成大部分工作。

第三，你需要一个发起 HTTP 请求的工具。我推荐使用 Visual Studio Code 的 REST 客户端插件，但 Fiddler、Postman 和 cURL 也是优秀且流行的工具。你可以使用其中任何一个来跟随本书中的示例。

第四，你需要安装 Docker，并需要在 Docker 设置中启用 Kubernetes。

NOTE 在附录A中，你可以找到 Visual Studio、Visual Studio Code、JetBrains Rider 和 Docker 的下载、安装及快速使用信息。现在是时候设置你选择的工具了——在书的剩余部分你将需要它们。

## 1.9 一个简单的微服务示例

一旦你拥有了可运行的开发环境，是时候来一个“Hello World”式的微服务示例了。你将使用 ASP.NET 创建一个微服务，

该微服务只有一个 API 端点。通常，一个微服务会有多个端点，但对于这个示例来说，一个就足够了。该端点响应当前的 UTC 日期和时间。图1.9对此进行了说明。

![](img/608267d7803dc57a1a896ad121f37868_44_0.png)

**图1.9** 一个“Hello World”式的微服务，响应当前的日期和时间

> **NOTE** 当我提到*API端点*、*HTTP端点*或仅仅*端点*时，我指的是你的某个微服务响应 HTTP 请求的 URL。

要实现这个示例，你将遵循以下三个步骤：

1. 创建一个空的 ASP.NET 应用程序。
2. 向应用程序添加 ASP.NET MVC。
3. 添加一个包含端点实现的 MVC 控制器。

接下来的部分将详细讲解每一步。

### 1.9.1 创建一个空的 ASP.NET 应用程序

你需要做的第一件事是使用 .NET 命令行工具创建一个名为 `Hello-Microservices` 的空 ASP.NET 应用程序。如果你愿意，也可以使用 IDE 创建项目——结果是一样的。要使用 .NET 命令行工具，打开一个 shell 并执行命令 `dotnet new web --name HelloMicroservices`。

一旦你创建了空的 ASP.NET 应用程序并将其命名为 Hello-Microservices，你应该会看到一个名为 HelloMicroservices 的文件夹，其中包含以下文件：

- appsettings.Development.json
- appsettings.json
- HelloMicroservices.csproj
- Program.cs
- Startup.cs

项目中还有其他文件，但目前你只需要关注这些文件。

这是一个完整的应用程序，随时可以运行。它将响应路径“/”的 HTTP GET 请求，并返回字符串 Hello World。你可以通过进入 HelloMicroservices 文件夹并输入命令 `dotnet run` 从命令行启动该应用程序。

该应用程序在本地主机端口 5000 上运行。（请注意，如果你选择在 Visual Studio 内部运行，可能会分配到另一个端口。）如果在浏览器中访问 http://localhost:5000，你将得到“Hello World”响应。

