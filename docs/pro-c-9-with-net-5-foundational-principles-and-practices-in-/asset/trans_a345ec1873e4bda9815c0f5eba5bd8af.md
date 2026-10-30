

`GetFiles()`方法返回一个`FileInfo`对象数组，其中每个对象都公开了某个特定文件的详细信息（本章稍后会介绍`FileInfo`类型的完整细节）。在`Program`类中创建以下静态方法：

```csharp
static void DisplayImageFiles()
{
    DirectoryInfo dir = new
        DirectoryInfo(@"C:\Windows\Web\Wallpaper");
    // Get all files with a *.jpg extension.
    FileInfo[] imageFiles =
        dir.GetFiles("*.jpg", SearchOption.AllDirectories);

    // How many were found?
    Console.WriteLine("Found {0} *.jpg files\n", imageFiles.Length);

    // Now print out info for each file.
    foreach (FileInfo f in imageFiles)
    {
        Console.WriteLine("****************************");
        Console.WriteLine("File name: {0}", f.Name);
        Console.WriteLine("File size: {0}", f.Length);
        Console.WriteLine("Creation: {0}", f.CreationTime);
        Console.WriteLine("Attributes: {0}", f.Attributes);
        Console.WriteLine("****************************\n");
    }
}
```

请注意，调用`GetFiles()`时需要指定搜索选项；这样做是为了搜索根目录下的所有子目录。运行应用程序后，你将看到所有匹配搜索模式的文件列表。

## 使用 DirectoryInfo 类型创建子目录

你可以使用`DirectoryInfo.CreateSubdirectory()`方法以编程方式扩展目录结构。此方法既可以创建单个子目录，也可以在一次函数调用中创建多个嵌套子目录。下面的方法演示了如何使用该方法：在应用程序执行目录（用`.`表示）中创建一些自定义子目录，从而扩展应用程序的目录结构：

```csharp
static void ModifyAppDirectory()
{
    DirectoryInfo dir = new DirectoryInfo(".");

    // Create \MyFolder off application directory.
    dir.CreateSubdirectory("MyFolder");

    // Create \MyFolder2\Data off application directory.
    dir.CreateSubdirectory(
        $@"MyFolder2{Path.DirectorySeparatorChar}Data");
}
```

你不必捕获`CreateSubdirectory()`方法的返回值，但应注意，在执行成功时，该方法会返回一个表示新创建项目的`DirectoryInfo`对象。请考虑对前面方法进行如下修改：

```csharp
static void ModifyAppDirectory()
{
    DirectoryInfo dir = new DirectoryInfo(".");

    // Create \MyFolder off initial directory.
    dir.CreateSubdirectory("MyFolder");

    // Capture returned DirectoryInfo object.
    DirectoryInfo myDataFolder = dir.CreateSubdirectory(
        $@"MyFolder{Path.DirectorySeparatorChar}Data");

    // Prints path to ..\MyFolder2\Data.
    Console.WriteLine("New Folder is: {0}", myDataFolder);
}
```

如果你从顶级语句中调用此方法，然后使用 Windows 资源管理器检查 Windows 目录，就会看到新建的子目录已经存在。

## 使用 Directory 类型

你已经看到了`DirectoryInfo`类型的实际应用；现在可以开始了解`Directory`类型了。在大多数情况下，`Directory`的静态成员仿照了`DirectoryInfo`所定义的实例成员提供的功能。不过请记住，`Directory`的成员通常返回字符串数据，而不是强类型的`FileInfo`/`DirectoryInfo`对象。

现在来看看`Directory`类型的一些功能。下面这个最终的辅助函数使用`Directory.GetLogicalDrives()`方法显示当前计算机上所有映射驱动器的名称，并使用静态的`Directory.Delete()`方法删除之前创建的`\MyFolder`和`\MyFolder2\Data`子目录。

```csharp
static void FunWithDirectoryType()
{
    // List all drives on current computer.
    string[] drives = Directory.GetLogicalDrives();
    Console.WriteLine("Here are your drives:");
    foreach (string s in drives)
    {
        Console.WriteLine("--> {0} ", s);
    }

    // Delete what was created.
    Console.WriteLine("Press Enter to delete directories");
    Console.ReadLine();
    try
    {
        Directory.Delete("MyFolder");
        // The second parameter specifies whether you
        // wish to destroy any subdirectories.
        Directory.Delete("MyFolder2", true);
    }
    catch (IOException e)
    {
        Console.WriteLine(e.Message);
    }
}
```

## 使用 DriveInfo 类类型

`System.IO`命名空间提供了一个名为`DriveInfo`的类。与`Directory.GetLogicalDrives()`类似，静态的`DriveInfo.GetDrives()`方法允许你发现计算机驱动器的名称。不过，与`Directory.GetLogicalDrives()`不同，`DriveInfo`还提供了许多其他详细信息（例如驱动器类型、可用空间和卷标）。请考虑以下`Program`类，该类定义在名为`DriveInfoApp`的新控制台应用程序项目中：

```csharp
using System;
using System.IO;

// Get info regarding all drives.
DriveInfo[] myDrives = DriveInfo.GetDrives();
// Now print drive stats.
foreach (DriveInfo d in myDrives)
{
    Console.WriteLine("Name: {0}", d.Name);
    Console.WriteLine("Type: {0}", d.DriveType);

    // Check to see whether the drive is mounted.
    if (d.IsReady)
    {
        Console.WriteLine("Free space: {0}", d.TotalFreeSpace);
        Console.WriteLine("Format: {0}", d.DriveFormat);
        Console.WriteLine("Label: {0}", d.VolumeLabel);
    }
    Console.WriteLine();
}
Console.ReadLine();
```

下面是可能的输出：

```text
***** Fun with DriveInfo *****
Name: C:\
Type: Fixed
Free space: 284131119104
Format: NTFS
Label: OS

Name: M:\
Type: Network
Free space: 4711871942656
Format: NTFS
Label: DigitalMedia
```

到这里，你已经了解了`Directory`、`DirectoryInfo`和`DriveInfo`类的一些核心行为。接下来，你将学习如何创建、打开、关闭和销毁填充给定目录的文件。

## 使用 FileInfo 类

如前面`DirectoryApp`示例所示，`FileInfo`类允许你获取硬盘上现有文件的详细信息（例如创建时间、大小和文件属性），并帮助你创建、复制、移动和销毁文件。除了从`FileSystemInfo`继承的功能集合外，你还可以找到一些`FileInfo`类特有的核心成员，这些成员列在表20-4中。

表20-4。FileInfo 核心成员

| 成员 | 用途 |
|---|---|
| AppendText() | 创建一个`StreamWriter`对象（稍后介绍），用于向文件追加文本 |
| CopyTo() | 将现有文件复制到新文件 |
| Create() | 创建一个新文件，并返回一个`FileStream`对象（稍后介绍），用于操作新创建的文件 |
| CreateText() | 创建一个用于写入新文本文件的`StreamWriter`对象 |
| Delete() | 删除`FileInfo`实例绑定的文件 |
| Directory | 获取父目录的实例 |
| DirectoryName | 获取父目录的完整路径 |
| Length | 获取当前文件的大小 |
| MoveTo() | 将指定文件移动到新位置，并可选择指定新的文件名 |
| Name | 获取文件名 |
| Open() | 以各种读写及共享权限打开文件 |
| OpenRead() | 创建一个只读的`FileStream`对象 |
| OpenText() | 创建一个`StreamReader`对象（稍后介绍），用于读取现有文本文件 |
| OpenWrite() | 创建一个只写的`FileStream`对象 |

请注意，`FileInfo`类的大多数方法都会返回特定的 I/O 对象（例如`FileStream`和`StreamWriter`），这些对象允许你以各种格式开始向关联文件写入数据，或从中读取数据。稍后你就会了解这些类型；不过，在查看实际示例之前，先了解使用`FileInfo`类类型获取文件句柄的各种方式会很有帮助。

## FileInfo.Create() 方法

下面的示例都位于名为`SimpleFileIO`的控制台应用程序中。创建文件句柄的一种方式是使用`FileInfo.Create()`方法，如下所示：

```csharp
using System;
using System.IO;

Console.WriteLine("***** Simple IO with the File Type ****\n");
// Change to a folder on your machine that you have read/write access to, or run as administrator
var fileName = $@"C{Path.VolumeSeparatorChar}{Path.DirectorySeparatorChar}temp{Path.DirectorySeparatorChar}Test.dat";
// Make a new file on the C drive.
FileInfo f = new FileInfo(fileName);
FileStream fs = f.Create();

// Use the FileStream object...

// Close down file stream.
fs.Close();
```

> 注意 根据用户权限和系统配置，这些示例可能需要以管理员身份运行 Visual Studio。

请注意，`FileInfo.Create()`方法返回一个`FileStream`对象，该对象公开了针对底层文件的同步和异步写入/读取操作（稍后会详细介绍）。需要注意的是，`FileInfo.Create()`返回的`FileStream`对象授予所有用户完整的读写访问权限。

还要注意，在完成对当前`FileStream`对象的使用后，必须确保关闭该句柄，以释放底层非托管流资源。由于`FileStream`实现了`IDisposable`，因此你可以使用 C# 的`using`作用域，让编译器生成资源清理逻辑（详情请参阅第8章），如下所示：

```csharp
var fileName = $@"C{Path.VolumeSeparatorChar}{Path.DirectorySeparatorChar}Test.dat";
...
// wrap the file stream in a using statement
// Defining a using scope for file I/O
FileInfo f1 = new FileInfo(fileName);
using (FileStream fs1 = f1.Create())
{
    // Use the FileStream object...
}
f1.Delete();
```

> 注意 本章中的几乎所有示例都包含`using`语句。本可以使用新的 using 声明语法，但在本次重写中选择不使用，以便让示例专注于我们正在研究的`System.IO`组件。

