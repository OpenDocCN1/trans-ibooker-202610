# # Uploading existing images

Once started, our app will monitor any new files that are created or changed inside the
directory, but what if we start the application and point it to a directory that's already full?
We can change our OnStart code so that it calls a method that will scan the directory for
us. Presumably, we'll need to pass information on where it can scan and what to do once it
finds something:

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

The new line here is a call to a new object called _fileDiscoverer. Here, we're passing
through the directory that we're monitoring and a callback to the UploadFile method. To
begin with, let's add this dependency to the constructor:

```
private readonly string _path;
private readonly ILogger _logger;
private readonly IFileDiscoverer _fileDiscoverer;
private FileMonitor _fileMonitor;
private AppSettings _appSettings;
```

```
public PhotoService(string path, ILogger logger, IFileDiscoverer
fileDiscoverer)
{
    ServiceName = "PhotoService";
    AutoLog = true;
    _path = path;
    _logger = logger;
    _fileDiscoverer = fileDiscoverer;
}
```

As with all good dependency injections, we're using an interface to pass the object in; let's
see what our interface needs to look like:

```
public interface IFileDiscoverer
{
```