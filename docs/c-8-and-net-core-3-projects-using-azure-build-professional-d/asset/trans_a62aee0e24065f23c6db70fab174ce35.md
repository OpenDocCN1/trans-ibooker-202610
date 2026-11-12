
从这里，我们可以选择以下选项来创建新集合：

![](img/762492a8287f369cc1ac4621a212f193_97_0.png)

如你所见，我在这里保留了默认值。值得注意的是 **固定 (10 GB)** 不是推荐值，在生产级应用中，你很可能会选择 **无限**。如果这样做，则需要提供分区键。我将吞吐量保留为默认值 **1000 个请求单位/秒 (RU/s)**：这实际上允许你为所需性能付费；速度越慢，越便宜（反之亦然）！创建后，你应该能看到新集合：

![](img/762492a8287f369cc1ac4621a212f193_98_0.png)

最后要做的是导航到连接字符串选项卡并复制主连接字符串：

![](img/762492a8287f369cc1ac4621a212f193_98_1.png)

既然我们的 Cosmos DB 实例已配置，可以继续创建 Web 应用程序并连接到它。

## 将 ASP.NET Core MVC 应用程序连接到 Cosmos DB

讨论在应用程序中使用 Cosmos DB 时，人们会好奇将此功能添加到新 ASP.NET Core MVC 应用程序有多容易。这个过程真的很简单。首先，创建新项目：

![](img/762492a8287f369cc1ac4621a212f193_99_0.png)

- 1. 将项目命名为 BugTracker：

![](img/762492a8287f369cc1ac4621a212f193_99_1.png)

- 2. 选择创建 ASP.NET Core Web 应用程序选项：

![](img/762492a8287f369cc1ac4621a212f193_100_0.png)

- 3. 在下一屏幕上，选择以下选项（参考下面的屏幕截图）：

![](img/762492a8287f369cc1ac4621a212f193_101_0.png)

### 创建新的 ASP.NET Core Web 应用程序

- 4. 从下拉列表中选择 ASP.NET Core 3.0。
- 5. 选择 Web 应用程序 (Model-View-Controller)。
- 6. 取消选中 Enable Docker Support 选项。最后，单击 OK 按钮。
- 7. 单击创建，你的新 ASP.NET Core MVC 应用程序将被创建。

在创建时可以轻松为应用程序启用 Docker 支持。你也可以为现有应用程序启用 Docker 支持。

我们将在后面章节中介绍 Docker 以及如何使应用程序与 Docker 配合使用。目前，我们的应用程序不需要 Docker 支持。保持未选中状态并像往常一样创建应用程序。

### 添加 NuGet 包

我们需要将 MongoDB 客户端 API 添加到项目中。最好的方法是添加 NuGet 包。我们可以按照以下步骤操作：

- 1. 右键单击项目，并从上下文菜单中选择 **管理 NuGet 包...**，如下面的屏幕截图所示：

![](img/762492a8287f369cc1ac4621a212f193_102_0.png)

- 2. 在 NuGet 屏幕上，选择 **浏览** 选项卡，并输入 `MongoDB.Driver` 作为搜索词。

- 3. 选择由 **MongoDB.Driver** 由 **MongoDB** 选项。

- 4. 单击 **安装** 按钮以将最新稳定版包添加到项目中。如下面的屏幕截图所示：

![](img/762492a8287f369cc1ac4621a212f193_103_0.png)

- 5. 可以在 Visual Studio 的 **输出** 窗口中查看进度。

- 6. 将 MongoDB 添加到项目后，你会看到 **MongoDB.Driver (2.5.0)** 已添加到项目的 **NuGet** 依赖项下，如下面的屏幕截图所示：

![](img/762492a8287f369cc1ac4621a212f193_104_0.png)

- 7. 展开 Controllers 文件夹。你会发现，默认情况下 Visual Studio 已创建 HomeController.cs 文件。该文件中的代码应该如下所示：

```
public class HomeController : Controller
{
    private readonly ILogger<HomeController> _logger;

    public HomeController(ILogger<HomeController> logger)
    {
        _logger = logger;
    }

    public IActionResult Index()
    {
        return View();
    }

    public IActionResult Privacy()
    {
        return View();
    }

    [ResponseCache(Duration = 0, Location = ResponseCacheLocation.None, NoStore = true)]
    public IActionResult Error()
    {
        return View(new ErrorViewModel { RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier });
    }
}
```

我们希望能够从此处连接到 Cosmos DB，所以让我们创建一些代码来连接到 Mongo 客户端。

你需要向类添加 using 语句，如下所示：
using MongoDB.Driver;

连接到 MongoDB 的步骤如下：

- 1. 通过键入片段短代码 ctor 并两次制表符，或显式键入代码来创建构造函数。你的构造函数需要创建 MongoClient 的新实例。完成后，代码应如下所示：

```
public HomeController()
{
    var mclient = new MongoClient();
}
```

虽然这说明了 MongoClient 的用法，但像这样在构造函数中实例化类通常被视为不良实践，因为它几乎使单元测试不可能。

- 2. 为了让 `MongoClient` 工作，我们需要提供我们创建的 MongoDB 实例的连接字符串。在解决方案 'Bug Tracker' 窗格中打开 `appsettings.json` 文件，如下面的屏幕截图所示：

![](img/762492a8287f369cc1ac4621a212f193_106_0.png)

- 3. 打开 `appsettings.json` 文件后，它应该如下所示：

```
{
  "Logging": {
    "IncludeScopes": false,
    "LogLevel": {
      "Default": "Warning"
    }
  }
}
```

- 4. 这时你需要之前复制的连接字符串。修改文件并添加 MongoDB 连接详细信息，如下所示（将 `[connectionstring]` 替换为你之前复制的值）：

```
{
  "MongoConnection": {
    "ConnectionString": "[connectionstring]",
    "Database": "TaskLogger"
  },
}
```

```
"Logging": {
    "IncludeScopes": false,
    "LogLevel": {
        "Default": "Warning"
    }
}
}
```

- 5. 我们现在想要在 Models 文件夹中创建 Settings.cs 文件，如下面的屏幕截图所示：

![](img/762492a8287f369cc1ac4621a212f193_107_0.png)

- 6. 打开 Settings.cs 文件并添加以下代码：

```
public class Settings
{
    public string ConnectionString { get; set; }
    public string Database { get; set; }
}
```

- 7. 我们现在需要打开 Startup.cs 文件并按如下方式修改 ConfigureServices 方法，以注册服务：

```
public void ConfigureServices(IServiceCollection services)
{
    services.AddControllersWithViews();
    services.AddRazorPages();

    services.Configure<Settings>(Options => {
    Options.ConnectionString = Configuration.GetSection
    ("MongoConnection:ConnectionString").Value; Options.Database =
    Configuration.GetSection
    ("MongoConnection:Database").Value; }); }

```

- 8. 返回 HomeController.cs 文件并修改构造函数以将连接字符串传递给 MongoClient：

```
public HomeController(IOptions<Settings> settings)
{
    var mclient = new
    MongoClient(settings.Value.ConnectionString);
}
```

- 9. 此时，我们想要测试代码以查看它是否实际访问我的 MongoDB 实例。为此，修改代码以返回集群描述：

```
IMongoDatabase _database;

public HomeController(IOptions<Settings> settings)
{
    var mclient = new
    MongoClient(settings.Value.ConnectionString);
    _database = mclient.GetDatabase(settings.Value.Database);
}

public IActionResult Index()
{
    return Json(_database.Client.Cluster.Description);
}
```

- 10. 运行 ASP.NET Core MVC 应用程序并读取浏览器中输出的信息，如下面的屏幕截图所示：

![](img/762492a8287f369cc1ac4621a212f193_109_0.png)

这很好，但让我们看看如何将添加数据库连接的逻辑分离到自己的类中。

### 创建 MongoDbRepository 类

要创建 MongoDbRepository 类，我们需要执行以下步骤：

- 1. 在解决方案中创建一个名为 Data 的新文件夹。在该文件夹中，创建一个名为 MongoDbRepository 的新类：

![](img/762492a8287f369cc1ac4621a212f193_109_1.png)
