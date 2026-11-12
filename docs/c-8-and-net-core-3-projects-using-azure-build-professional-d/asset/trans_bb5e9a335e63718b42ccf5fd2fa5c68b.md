

var configuration = builder.Build();

appSettings.ConnectionString =
    configuration["ConnectionStrings:netcodephotostorage"];
appSettings.MonitorPath = configuration["MonitorPath"];

return appSettings;
}
}

由于我们已创建一个服务来加载配置设置，因此可以将其传递给 AzureStorageClientService。以下是该文件中修订后的构造函数和类变量：

```
private readonly AppSettings _appSettings;
public AzureStorageClientService(AppSettings appSettings)
{
	_appSettings = appSettings;
}
```

我想你会同意，现在我们不在服务内部加载配置，代码看起来更加清晰了；GetBlockBlobReference 也需要像下面这样更新：

```
private CloudBlockBlob GetBlockBlobReference(string fileName)
{
	if (CloudStorageAccount.TryParse(_appSettings.ConnectionString, out
	CloudStorageAccount storageAccount))
	{
		var client = storageAccount.CreateCloudBlobClient();
		var container = client.GetContainerReference("photos");
		var blob = container.GetBlockBlobReference(fileName);
		return blob;
	}
	else
	{
		throw new Exception("Unable to parse the storage account");
	}
}
```

我们的 PhotoService.cs 类将在启动时需要实例化这个新的配置服务：

```
private FileMonitor _fileMonitor;
private AppSettings _appSettings;

protected override void OnStart(string[] args)
{
    var configurationService = new ConfigurationService();
    _appSettings = configurationService.Load();

    var cloudStorageClientService = new
    AzureStorageClientService(_appSettings);
    _fileMonitor = new FileMonitor(_appSettings.MonitorPath,
    cloudStorageClientService);
}
```

最后，如果你创建了一个用于测试的控制台应用程序，也需要更新它（在 Program.cs 中）：

```
static void TestFileMonitorNewFile()
{
    var configurationService = new ConfigurationService();
    var appSettings = configurationService.Load();

    var cloudStorageClientService = new
    AzureStorageClientService(appSettings);

    using var fileMonitor = new FileMonitor(appSettings.MonitorPath,
    cloudStorageClientService);
    for (; ; ) { }
}
```

这样一来，我们就完成了。现在，应用程序应该会从 appsettings.json 文件读取配置，并且，如果我们在 c:\tmp 文件夹中创建或放入文件，应该会看到它被上传到云端。当文件被更改时，更改后的版本会被上传；然而，我们缺少三个关键功能。

接下来我们需要添加的是日志记录。关于 Windows 服务（或任何类型的服务）的不幸现实是，它们臭名昭著地难以调试。

其次，如果我们运行应用程序且目标目录为空，那么放入该目录的所有文件都会被传输到云端；但是，如果我们将其指向一个已充满照片的现有目录，该怎么办呢？最后，目前我们正在备份所有文件，而我们只关心图像格式。

让我们解决这三个剩余的功能。

## 日志记录

归根结底，日志记录只是将信息写入持久化存储，这意味着你可以记录到数据库或文件中。你甚至可以将日志输出到屏幕，但那样持久性可能不足。在我们的例子中，我们将文本输出到一个文件。让我们来看看日志代码：

```
public class FileLogger : ILogger
{
    private readonly string _loggingPath;

    public FileLogger(string loggingPath)
    {
        _loggingPath = loggingPath;
    }

    public void Log(string message)
    {
        File.AppendAllText($@"{_loggingPath}\PhotoStorage.Log.txt",
            $"{DateTime.Now} : {message}{Environment.NewLine}");
    }
}
```

这里，我们正在实现 ILogger 接口，我们稍后会介绍它。这个类并没有太多复杂之处——我们只是接受一个路径，然后将其输出到该路径下的一个文件中。让我们看看这个接口长什么样：

```
public interface ILogger
{
    void Log(string message);
}
```

关于这个接口，除了它的存在之外，可能没有太多需要解释的。日志记录是那些你肯定希望能够为单元测试替换的功能之一，因此接口至关重要。我不会详细说明我记录日志的每个位置，但让我们来看看 PhotoService 构造函数：

```
private readonly string _path;
private readonly ILogger _logger;
private FileMonitor _fileMonitor;
private AppSettings _appSettings;

public PhotoService(string path, ILogger logger)
{
    ServiceName = "PhotoService";
    AutoLog = true;
    _path = path;
    _logger = logger;
}
```

正如你所见，我们将日志记录器传入；因此，我们在 Main 方法中对其实例化：

```
static void Main(string[] args)
{
    var path = args[0];
    var logger = new FileLogger(path);

    using var service = new PhotoService(path, logger);
    ServiceBase.Run(service);
}
```

这里做了一点调整：现在，我们获取参数并将其存储在 path 变量中。

现在，我们有了日志功能；如果你想看几条日志消息，可以尝试在 OnStart 和 OnStop 中各放一条。

> 虽然我不会列出所有记录日志的地方，但你可以自由下载本书的 GitHub 仓库并查看。如果在跟随学习时遇到任何问题，此日志功能将非常宝贵；不过，如果你想了解更多，后面有一个关于测试和调试的章节。

## 仅上传图像

为了确定是否有图像，我们将在处理任何文件之前添加一个检查。以下是 OnCreated 和 OnChanged 的修订方法：

```
private async void OnCreated(object sender, FileSystemEventArgs e)
{
    if (!FileHelper.IsImage(e.Name)) return;
    await _cloudStorageClientService.UploadFile(e.FullPath);
}
```

```
private async void OnChanged(object sender, FileSystemEventArgs e)
{
    if (!FileHelper.IsImage(e.Name)) return;
    await _cloudStorageClientService.UploadFile(e.FullPath);
}
```

正如你所见，我们只有一个非常基础的门控检查，用于返回我们是否正在处理图像。当然，我们需要编写这个方法。这里，我们接触到了一个尚未提及的 C# 8 新特性。在 C# 8 之前，这个方法可能如下所示：

```
public static bool IsImage(string fileName)
{
    string ext = Path.GetExtension(fileName);
    switch (ext)
    {
        case "png":
        case "jpg":
        case "jpeg":
        case "bmp":
        case "gif":
            return true;
        default: return false;
    }
}
```

然而，借助新的 switch 语句，我们可以像下面这样编写这个方法：

```
public static bool IsImage(string fileName)
{
    string ext = Path.GetExtension(fileName);
    return ext switch
    {
        "png" => true,
        "jpg" => true,
        "jpeg" => true,
        "bmp" => true,
        "gif" => true,
        _ => false
    };
}
```

如果我们在 switch 语句本身中包含一个 GetExtension 调用，可以使其更加简洁，但通常这样写更易读。

新的 switch 语句实际上比这更强大，事实上，它支持模式匹配。

让我们看看如何在启动时上传目录中找到的任何图像。

## 上传现有图像

启动后，我们的应用程序会监控目录中新创建或更改的任何文件，但如果我们启动应用程序并将其指向一个已满文件的目录，该怎么办呢？我们可以修改 OnStart 代码，使其调用一个为我们扫描目录的方法。推测，我们需要传递关于它可以扫描哪里以及找到文件后要做什么的信息：

```
protected override void OnStart(string[] args)
{
    var configurationService = new ConfigurationService();
    _appSettings = configurationService.Load(_path);

    var cloudStorageClientService = new
    AzureStorageClientService(_appSettings);

    _fileDiscoverer.DiscoverFiles(_appSettings.MonitorPath, (file) =>
    cloudStorageClientService.UploadFile(file));
    _fileMonitor = new FileMonitor(_appSettings.MonitorPath,
    cloudStorageClientService, _logger);
}
```

这里新增的一行是对名为 _fileDiscoverer 的新对象的调用。这里，我们传递要监控的目录以及一个指向 UploadFile 方法的回调。首先，让我们将此依赖项添加到构造函数中：

```
private readonly string _path;
private readonly ILogger _logger;
private readonly IFileDiscoverer _fileDiscoverer;
private FileMonitor _fileMonitor;
private AppSettings _appSettings;

public PhotoService(string path, ILogger logger, IFileDiscoverer fileDiscoverer)
{
    ServiceName = "PhotoService";
    AutoLog = true;
    _path = path;
    _logger = logger;
    _fileDiscoverer = fileDiscoverer;
}
```

与所有良好的依赖注入一样，我们使用接口来传入对象；让我们看看我们的接口应该是什么样子：

```
public interface IFileDiscoverer
{

