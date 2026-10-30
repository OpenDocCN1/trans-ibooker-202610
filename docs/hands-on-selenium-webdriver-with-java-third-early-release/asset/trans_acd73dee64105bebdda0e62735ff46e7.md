

会话超时（session-timeout），即节点在无活动时被终止的超时时间（默认 300 秒）。

- 使用 TOML 文件。TOML（Tom's Obvious Minimal Language）是一种设计为人类可读的配置格式。与命令行选项一样，这些文件允许配置 Selenium Grid 参数，但使用 TOML 语法表示。

## 云服务提供商

如第 1 章所述，Selenium 生态中的云服务提供商是提供自动化 Web 和移动测试托管服务（通常为商业服务）的公司。云服务提供商提供的常见服务包括：

- 浏览器即服务：按需请求由提供商托管的 Web 浏览器。这些浏览器通常具有不同的类型、版本和操作系统。该功能通常用于跨浏览器自动化测试或实时测试。
- 分析能力：监控和调试自动化测试。为此，云服务提供商通常支持会话录制或丰富的错误报告功能。
- 移动测试：在不同平台（如 Android 和 iOS）上请求模拟（和真实）移动设备。
- 视觉测试：自动检查 UI，确保最终用户获得正确的视觉体验。

目前面向 Selenium 的云服务提供商示例有 Sauce Labs、BrowserStack、LambdaTest、CrossBrowserTesting、Moon Cloud、TestingBot、Perfecto 或 Testinium。这些公司都提供不同定价方案的具体服务。它们之间的共同点是，每个云服务提供商都维护一个 Selenium Server 端点，我们可以在 RemoteWebDriver 测试中使用。示例 6-9 演示了如何使用其中之一（具体来说是 Sauce Labs）创建 WebDriver 对象。你可以在示例仓库中找到其他云服务提供商（BrowserStack、LambdaTest、CrossBrowserTesting、Perfecto 和 Testinium）的等效测试。这些测试允许使用由云服务提供商管理的远程浏览器。

## 示例 6-9. 使用 Sauce Labs 的测试设置

```java
@BeforeEach
void setup() throws MalformedURLException {
    String username = System.getProperty("sauceLabsUsername"); ①
    String accessKey = System.getProperty("sauceLabsAccessKey");
    assumeThat(username).isNotEmpty(); ②
    assumeThat(accessKey).isNotEmpty();

    MutableCapabilities capabilities = new MutableCapabilities();
    capabilities.setCapability("username", username); ③
    capabilities.setCapability("access_key", accessKey);
    capabilities.setCapability("name", "My SauceLabs test"); ④
    capabilities.setCapability("browserVersion", "latest"); ⑤

    ChromeOptions options = new ChromeOptions();
    options.setCapability("sauce:options", capabilities); ⑥
    URL remoteUrl = new URL(
        "https://ondemand.eu-central-1.saucelabs.com:443/wd/hub"); ⑦

    driver = new RemoteWebDriver(remoteUrl, options); ⑧
}
```

① 要使用 Sauce Labs，我们需要一个有效账户。换言之，我们需要以用户名和访问密钥形式提供的凭据。为了避免在测试逻辑中硬编码这些凭据，我在此测试中使用 Java 系统属性。这些属性可以在执行命令中提供（例如 `mvn test -DsauceLabsUsername=myname -DsauceLabsAccessKey=mykey`）。另一种指定这些数据的方法是使用环境变量（例如 `String username = System.getenv("SAUCELABS_USERNAME");`）。

② 当用户名或密钥不可用时，我们使用假设（assumptions）跳过此测试。

③ 我们需要将用户名和密钥作为 Selenium 能力包含进来。

④ 我们可以指定自定义标签，以便在 Sauce Labs 仪表板中标识此测试（见图 6-8）。

⑤ 我们使用给定浏览器的最新版本（Chrome，如下一行所指定）。

⑥ 我们使用名为 `sauce:options` 的自定义标签来选择 Sauce Labs 云中所需的能力。

⑦ 我们使用 Sauce Labs 的公共端点作为远程 URL。Sauce Labs 在不同区域提供端点。在此示例中，我使用欧盟中部数据中心。其他可能为美国西部、美国东部或亚太东南部。

⑧ 我们同时使用 URL 和能力来创建 RemoteWebDriver 实例。

![](img/b67ec8e49249e340add6ed66ab413d78_351_0.png)

图 6-8. Sauce Labs 仪表板显示自动化测试结果

## Docker 容器中的浏览器

Docker 是一个开源平台，允许将应用程序创建、部署和运行为轻量级、可移植的容器。Docker 平台由两个主要组件组成：

- Docker Engine，这是一个允许在主机上创建和运行容器的应用程序。Docker Engine 是一个客户端-服务器应用程序，由三个元素组成：
  - 一个以守护进程（dockerd）形式实现的服务器。
  - 一个 REST API，供应用程序客户端用来指示守护进程。
  - 一个 CLI 工具（docker 命令）。
- Docker Hub，这是一种用于分发容器的云服务。

在 Selenium 中，Docker 可以成为支持基于 Selenium WebDriver 的自动化测试所需浏览器基础架构的相关技术。如下面各小节所述，在 Docker 容器中执行浏览器有不同的替代方案。

## 用于 Selenium Grid 的 Docker 镜像

Selenium 套件的一个官方子项目是 docker-selenium。该项目为不同的 Selenium Grid 组件（即 standalone、hub、nodes、router、distributor、session queue 等）和 Web 浏览器（Chrome、Firefox 和 Edge）维护 Docker 镜像。这些 Docker 镜像是开源的，并发布在 Docker Hub 上。使用这些镜像的一个简单方式是使用 shell（通过 docker 命令）启动它们，并使用 RemoteWebDriver 实例来驱动容器化的浏览器。以下小节将解释如何操作。

> 注意  
> 本节中提供的命令和测试假定你的系统中已安装 Docker。换句话说，你需要在机器上安装 Docker Engine 才能正确执行这些示例。

## Standalone

我们可以在 Docker Hub 中找到用于独立浏览器（Chrome、Firefox 和 Edge）的 Selenium 镜像。以下命令演示了如何使用 shell 在 Docker 中启动 Chrome。

```bash
docker run -d -p 4444:4444 --shm-size="2g" selenium/standalone-chrome:latest
```

此命令启动 Docker 镜像 `selenium/standalone-chrome:latest`，即 Docker Hub 上可用的最新版 Chrome。或者，我们可以使用固定的 Chrome 版本（例如 `selenium/standalone-chrome:94.0`）。Docker 容器以分离模式（使用 `-d` 标志）启动，并使用 2 GB 的共享内存（`--shm-size="2g"`）。这个数值是已知的良好工作值，不过你可以根据资源或特定需求进行更改。最后，容器内部端口 4444 映射到执行命令的主机的同一端口（`-p 4444:4444`）。总而言之，我们可以使用以下 Java 命令来实例化一个使用此容器化 Chrome 的 WebDriver 对象：

```java
WebDriver driver = new RemoteWebDriver("http://localhost:4444/", new ChromeOptions());
```

## Hub 和节点

我们可以使用官方 Selenium Docker 镜像轻松地以 hub-节点模式启动 Selenium Grid。以下命令演示了如何在 shell 中执行此操作。

```bash
docker network create grid ①
```

```bash
docker run -d -p 4442-4444:4442-4444 --net grid \
    --name selenium-hub selenium/hub:4.0.0 ②
```

```bash
docker run -d --net grid -e SE_EVENT_BUS_HOST=selenium-hub --shm-size="2g" \
    -e SE_EVENT_BUS_PUBLISH_PORT=4442 -e \
SE_EVENT_BUS_SUBSCRIBE_PORT=4443 \
    selenium/node-chrome:4.0.0 ③
```

```bash
docker network rm grid ④
```

① 首先，我们创建一个名为 grid 的 Docker 网络。该网络允许 hub 和节点之间使用其主机名（例如 selenium-hub）进行通信。

② 我们启动 Selenium Hub。我们需要映射端口 4444（用于 Selenium Server URL）和 4442–4443（用于注册节点）。

③ 我们注册节点。在此命令中，我们使用 Chrome（`selenium/node-chrome`）。可以使用其他 Docker 镜像（例如 `selenium/node-firefox` 或 `selenium/node-edge`）在 hub 中注册其他浏览器。

④ 如果不再需要，我们可以在最后删除 grid 网络。

## 更多功能

docker-selenium 项目提供了各种各样的功能。我建议你查看其 README 以了解更多详细信息。这些功能的摘要如下：

- Docker Compose 脚本。这些脚本允许轻松启动 Selenium Grid hub-节点模式和完全分布式模式。
- 视频录制。我们可以使用另一个 Docker 容器来录制节点中浏览器的桌面会话。

