
输入 dotnet。如果 dotnet CLI 已安装，你会收到 CLI 的帮助文本；如果没有，你会从 Powershell 得到一个错误。要安装 dotnet CLI，请访问 http://dot.net，点击 Download，并按照说明安装 .NET SDK。

## A.3 在 VS Code 中设置 REST 客户端

REST 客户端是一个面向文本的简单 VS Code 扩展，允许你发出 HTTP 请求（见图 A.1）。你在本书中会一直使用此类请求与微服务交互。除了简单之外，REST 客户端还有一个优点：它处理扩展名为 .http 的文本文件，因此很容易通过版本控制共享示例 HTTP 请求。你会在本书的代码下载和 GitHub 仓库中找到此类文件。此外，同样的文件在 Rider 中也能以类似的界面工作。

![](img/608267d7803dc57a1a896ad121f37868_316_0.png)

图 A.1 REST 客户端使用简单的 .http 文件。你可以通过点击 Send Request 发出请求，响应显示在右侧窗格中。

要安装 REST 客户端，请在 VS Code 中转到“扩展”选项卡，搜索 REST client，然后点击 Install。

## A.4 Docker 和 Kubernetes

在本书中，我使用 Docker 将微服务打包为容器，以便部署到生产环境，并使用 Azure 上的 Kubernetes 作为生产环境的示例。为了能够构建 Docker 镜像，你需要安装 Docker。安装了 Docker 之后，你可以运行和调试容器，但为了能像在 Azure 上一样运行容器，你还需要 Kubernetes。

如果你使用 Windows 或 Mac，你将使用 Docker Desktop；如果你使用 Linux，你将根据你的 Linux 发行版使用 .deb 或 .rpm 包。

要安装 Docker，请访问 https://docs.docker.com/engine/install/ 并按照适用于你的平台的说明进行操作。如果你使用 Windows，在安装 Docker Desktop 时应选择使用带有 Linux 容器的 WSL2 后端。Docker 安装完成后，你应该能够从 shell 中调用 docker CLI。

在 Docker 之外，你还需要 Kubernetes。如果你使用 Windows 或 Mac，Docker Desktop 已经捆绑了 Kubernetes，你只需在 Docker Desktop 设置中启用 Kubernetes，如图 A.2 所示。第一次会花费一些时间，因为 Docker Desktop 会下载、安装并启动 Kubernetes。

![](img/608267d7803dc57a1a896ad121f37868_317_0.png)

图 A.2 可以在 Docker Desktop 设置中启用 Kubernetes。

在 Linux 上，你必须单独获取 Kubernetes，并且有很多选择。一个简单的选项是 MicroK8S，你可以按照 https://microk8s.io/ 上的说明进行安装。安装完 MicroK8S 后，你可能想添加这个别名，以便轻松访问 Kubernetes 命令行工具：

```
> snap alias microk8s.kubectl kubectl
```

安装 Kubernetes 后，你应该能够从 shell 中调用 Kubernetes 命令行工具 kubectl。

## A.5 Azure CLI

在本书中，我使用 Azure 建立一个小的 Kubernetes 集群，作为生产环境的示例，将微服务部署到该集群。要跟随操作，你需要一个 Azure 账户和 Azure CLI。

要创建 Azure 账户，请访问 https://azure.microsoft.com/en-us/free/。要获取 Azure CLI，请前往 https://docs.microsoft.com/en-us/cli/azure/install-azure-cli 并按照适用于你的平台的安装说明操作。安装 Azure CLI 后，你可以在 shell 中运行 `az login` 将 CLI 登录到你的 Azure 账户。

现在你已经拥有 IDE、`dotnet` 命令行工具、用于处理 HTTP 请求的工具、Docker、Kubernetes 和 Azure CLI，可以开始编写本书中的所有示例代码了。

## 延伸阅读

## 微服务

Cramon, Jeppe. “微服务：重要的不只是规模，还在于如何使用它们”，第1-5部分。2014-2015. https://cramonblog.wordpress.com/2014/02/25/micro-services-its-not-only-the-size-that-matters-its-also-how-you-use-them-part-1/.

Fowler, Martin. “微服务先决条件.” 2014年8月28日. http://martinfowler.com/bliki/MicroservicePrerequisites.html. “单体优先.” 2015年6月3日. http://martinfowler.com/bliki/MonolithFirst.html.

Lewis, James 和 Martin Fowler. “微服务.” 2014年3月25日. http://martinfowler.com/articles/microservices.html.

Tilkov, Stefan. “不要从单体开始.” 2015年6月9日. http://martinfowler.com/articles/dont-start-monolith.html.

Newman, Sam. 《构建微服务：设计细粒度系统》. O'Reilly Media, 2015. “模式：前端的后端.” 2015年11月18日. http://samnewman.io/patterns/architectural/bff.

## 软件设计与架构概述

Beck, Kent. 《测试驱动开发：示例》. Addison-Wesley Professional: 2002.

Conway, Melvin E. “委员会如何发明？” Datamation (1968年4月). www.melconway.com/research/committees.html.

“纵深防御” 维基百科. https://en.wikipedia.org/wiki/Defense_in_depth_(computing).

Evans, Eric. 《领域驱动设计：软件核心复杂性应对之道》. Addison-Wesley Professional: 2003.

Forsgren, Nicole, Gene Kim 和 Jez Humble. 《加速》. IT Revolution Press: 2018.

Forgren, Nicole, Dustin Smith, Jez Humble 和 Jessie Frazelle. “2018年DevOps加速状态.” https://inthecloud.withgoogle.com/state-of-devops-18/dl-cd.html.

Fowler, Martin. “测试金字塔.” 2012年5月1日. http://martinfowler.com/bliki/TestPyramid.html.

________. “集成契约测试.” 2011年1月12日. http://martinfowler.com/bliki/IntegrationContractTest.html.

________. “控制反转容器和依赖注入模式.” https://martinfowler.com/articles/injection.html.

Freeman, Steve 和 Nat Pryce. 《测试驱动的面向对象软件开发》. Addison-Wesley Professional: 2009.

Hohpe, Gregor 和 Bobby Woolf. *企业集成模式：设计、构建和部署消息传递解决方案.* Addison-Wesley Professional: 2003.

Humble, Jez 和 David Farley. *持续交付：通过构建、测试和部署自动化实现可靠的软件发布.* Addison-Wesley Professional: 2010.

Martin, Robert C. “单一职责原则.” 2014年5月5日. http://mng.bz/RZgU.
“SRP：单一职责原则.” http://mng.bz/zQyz.

Nygard, Michael T. *发布！第二版：设计和部署生产就绪软件.* Pragmatic Programmers: 2018.

Vernon, Vaughn. *实现领域驱动设计.* Addison-Wesley Professional: 2013.

## 使用的技术

- ASP.NET: https://dotnet.microsoft.com/apps/aspnet
- Azure: https://azure.microsoft.com
- Azure Kubernetes Service (AKS): https://azure.microsoft.com/en-us/services/kubernetes-service/
- Dapper: http://mng.bz/7LHZ
- Elasticsearch: https://info.elastic.co/Getting-Started-ES.html
- Event Sourcing: https://www.eventstore.com/
- Kibana: www.elastic.co/products/kibana
- Kubernetes: https://kubernetes.io/
- .NET: https://dotnet.microsoft.com/
- NuGet 文档: http://docs.nuget.org
- OAuth: http://oauth.net/2
- OpenID Connect: http://openid.net/connect
- OpenTelemetry: https://opentelemetry.io/
- Polly 文档: http://www.thepollyproject.org/
- Scrutor: https://github.com/khellang/Scrutor
- Serilog: https://serilog.net
- xUnit: https://xunit.net/

协作（续）
  为忠诚度计划设置项目 112
  类型
    命令和查询 103–106
    事件 107–109
基于命令的协作 158
命令 112–116
命令通信风格 100, 102
复合应用模式 266, 268–269
复合应用 268–271
持续交付 12
ControllerBase 25, 114, 237
ControllerFeatureProvider 187
微服务的成本 14–15
create-aks.ps1 脚本 125
横切
  关注点 244–246
  可复用的微服务平台的构建 248–255
D
Dapper 139
数据格式 109–110
数据所有权和存储
  数据存储 130–131
  实现数据存储 139–154
    准备开发环境 140
    读取和使用缓存头 152–154
    在 HTTP 响应中设置缓存头 151–152
    存储微服务拥有的数据 141–145
    存储微服务触发的事件 145–151
在微服务之间划分数据 131–139
  数据所有权遵循业务能力 132–133
  为了提高速度和稳健性而复制 133–137
  微服务在哪里存储数据 137–139
DefaultHttpContext 类型 261
DELETE 端点 42, 101, 184, 291
DELETE 请求 42, 103
依赖注入 24
部署到 Kubernetes
  AKS（Azure Kubernetes 服务）65–72
  选择生产环境 55–57
  为购物车创建 Kubernetes 部署 62–65
  将购物车微服务放入容器 57–60
  运行购物车容器 60–62
  设置 Kubernetes 本地主机 60–62
开发环境设置 22
Azure CLI 297
Docker 和 Kubernetes 295–296
设置 dotnet 命令行界面 294–295
设置 IDE 293–294
  JetBrains Rider 294
  Visual Studio 294
  Visual Studio Code 294
在 VS Code 中设置 REST 客户端 295
Docker 293, 295–296
  为忠诚度计划的两个部分构建 Docker 容器 123–125
  构建特价优惠微服务的 Docker 容器 122–123
docker build 命令 123
docker CLI 296
docker stop 命令 59
Dockerfile 57–58
dotnet add package dapper 命令 142
dotnet build 58
dotnet cli 189, 293
dotnet 命令 120
dotnet 命令行工具 293–295, 297
dotnet new web -n HelloMicroservices 命令 23
dotnet new web -n ShoppingCart 命令 32
dotnet new xunit -n LoyaltyProgramUnitTests 命令 189
dotnet pack 251, 253
dotnet publish 命令 58
dotnet restore 58
dotnet run 23, 72, 175
dotnet ShoppingCart.dll 命令 58
dotnet test 187, 196, 203
微服务的缺点 14–15
E
支持工作效率 89–90, 98
最终用户应用 266–268
  通用应用 266–267
  专用应用 267–268
端点 23
  注入模拟 193–196
  使用 TestServer 和 HttpClient 进行单元测试 190–193
ENTRYPOINT 123
ETag 标头 134–135
事件订阅源，购物车微服务 48–52
  触发事件 49–50
  简单事件订阅源 51–52
  存储事件 50–51
事件订阅者组件 111

