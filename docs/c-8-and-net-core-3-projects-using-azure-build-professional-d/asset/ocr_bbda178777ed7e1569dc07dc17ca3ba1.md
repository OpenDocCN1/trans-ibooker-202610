```
void DiscoverFiles(string directory, Action<string> action);
}
```

> You can have Visual Studio automatically generate this for you by simply using the Ctrl + . shortcut on all the preceding unknown methods and interfaces.

Before we create the method itself, let's have a look at the Program.cs code that passes this in:

```
static void Main(string[] args)
{
    var path = args[0];
    var logger = new FileLogger(path);
    var fileDiscoverer = new FileDiscoverer(logger);

    using var service = new PhotoService(path, logger, fileDiscoverer);
    ServiceBase.Run(service);
}
```

We're just instantiating the object and passing it in.

> You may notice that this is starting to look like we would benefit from an IoC container. I'm purposely not using one because it detracts from the code that we're demonstrating; however, should you decide to extend this project, that would be an excellent place to start.

We'll need a method of scanning the directory for files that are already present. To this end, we'll be able to use a feature that was introduced in .NET Core 2.1 called FileSystemEnumerable. This builds on the work that was done in .NET Core 2.1 to introduce Span and, therefore, provides some performance benefits over the previous System.IO alternatives. Let's have a look at the IFileDiscoverer implementation:

```
public class FileDiscoverer : IFileDiscoverer
{
    private readonly ILogger _logger;

    public FileDiscoverer(ILogger logger)
    {
        _logger = logger;
    }

    public void DiscoverFiles(string directory, Action<string> action)
    {
        var enumerationOptions = new EnumerationOptions()
```