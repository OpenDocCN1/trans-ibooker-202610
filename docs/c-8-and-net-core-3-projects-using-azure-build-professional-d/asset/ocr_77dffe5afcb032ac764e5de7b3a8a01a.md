That's basically our service done. The astute among you may have noticed it doesn't actually *do* anything just yet, but as it stands, it's a legitimate service.

The service needs to do essentially three things: scan a folder on the hard drive for new files, upload files to the container on Azure, and scan the Azure container. Let's start with a useful little class that monitors the filesystem for us: FileSystemWatcher. This is now available to .NET Core through the magic of the compatibility pack. It makes sense to put all of this code into a separate class, so let's just create and destroy the class in our service. Then, we can see what that class might look like a little later on:

```
public class PhotoService : ServiceBase
{
    private FileMonitor _fileMonitor;

    protected override void OnStart(string[] args)
    {
        _fileMonitor = new FileMonitor("c:\\tmp");
    }

    protected override void OnStop()
    {
        _fileMonitor.Dispose();
    }
}
```

I'll be using the c:\tmp path a lot in this chapter. It's just a directory that I typically use for testing such things. If you don't want to use this, then you can use any path that you choose (although I recommend that you make it short). We will make this configurable later on.

Just use the keyboard shortcut Ctrl + . on the undefined new class, FileMonitor, which will create the class stub for you. Let's have a look at what it needs to look like:

```
public class FileMonitor : IDisposable
{
    private FileSystemWatcher _fileSystemWatcher;

    public FileMonitor(string path)
    {
        _fileSystemWatcher = new FileSystemWatcher(path);
        _fileSystemWatcher.Filter = "*.*";
        _fileSystemWatcher.EnableRaisingEvents = true;

        _fileSystemWatcher.Changed += new
        FileSystemEventHandler(OnChanged);
        _fileSystemWatcher.Created += new
```