
下面的示例（一个名为BinaryWriterReader的控制台应用程序项目，其中使用了System.IO）将一些数据类型写入一个新的*.dat文件：

```
using System;
using System.IO;

Console.WriteLine("***** Fun with Binary Writers / Readers *****\n");

// Open a binary writer for a file.
FileInfo f = new FileInfo("BinFile.dat");
using(BinaryWriter bw = new BinaryWriter(f.OpenWrite()))
{
    // Print out the type of BaseStream.
    // (System.IO.FileStream in this case).
    Console.WriteLine("Base stream is: {0}", bw.BaseStream);

    // Create some data to save in the file.
    double aDouble = 1234.67;
    int anInt = 34567;
    string aString = "A, B, C";

    // Write the data.
    bw.Write(aDouble);
    bw.Write(anInt);
    bw.Write(aString);
}
Console.WriteLine("Done!");
Console.ReadLine();
```

请注意，FileInfo.OpenWrite()返回的FileStream对象是如何传递给BinaryWriter类型的构造函数的。使用这种技术可以轻松地在写出数据之前分层放置一个流。请注意，BinaryWriter的构造函数接受任何Stream派生类型（例如FileStream、MemoryStream或BufferedStream）。因此，改为将二进制数据写入内存，只需提供一个有效的MemoryStream对象即可。

要从BinFile.dat文件中读取数据，BinaryReader类型提供了几种选项。在这里，你调用各种以读取为中心的成员来从文件流中取出每一块数据：

```
...
FileInfo f = new FileInfo("BinFile.dat");
...
// Read the binary data from the stream.
using(BinaryReader br = new BinaryReader(f.OpenRead()))
{
    Console.WriteLine(br.ReadDouble());
    Console.WriteLine(br.ReadInt32());
    Console.WriteLine(br.ReadString());
}
Console.ReadLine();
```

## 以编程方式监视文件

现在你已经更好地掌握了各种读写器的使用，接下来你将了解FileSystemWatcher类的作用。当你希望以编程方式监视（或“监控”）系统中的文件时，这个类型会非常有用。具体来说，你可以指示FileSystemWatcher类型监视文件，以检查System.IO.NotifyFilters枚举指定的任何操作。

```
public enum NotifyFilters
{
    Attributes, CreationTime,
    DirectoryName, FileName,
    LastAccess, LastWrite,
    Security, Size
}
```

要开始使用FileSystemWatcher类型，你需要设置Path属性来指定包含要监视文件的目录名称（和位置），同时还要设置Filter属性，该属性定义要监视文件的扩展名。

此时，你可以选择处理Changed、Created和Deleted事件，这些事件都与FileSystemEventHandler委托配合使用。该委托可以调用任何符合以下模式的方法：

```
// The FileSystemEventHandler delegate must point
// to methods matching the following signature.
void MyNotificationHandler(object source, FileSystemEventArgs e)
```

你还可以使用RenamedEventHandler委托类型处理Renamed事件，该委托可以调用符合以下签名的方法：

```
// The RenamedEventHandler delegate must point
// to methods matching the following signature.
void MyRenamedHandler(object source, RenamedEventArgs e)
```

虽然你可以使用传统的委托/事件语法来处理每个事件，但我们将使用新的lambda表达式语法。

接下来，让我们看看监视文件的过程。下面的控制台应用程序项目（名为MyDirectoryWatcher，并为System.IO使用了using）监视`bin\debug\net5.0`目录中的`*.txt`文件，并在文件被创建、删除、修改或重命名时打印消息：

```
using System;
using System.IO;

Console.WriteLine("**** The Amazing File Watcher App ****\n");
// Establish the path to the directory to watch.
FileSystemWatcher watcher = new FileSystemWatcher();
try
{
    watcher.Path = @".";
}
catch(ArgumentException ex)
{
    Console.WriteLine(ex.Message);
    return;
}
// Set up the things to be on the lookout for.
watcher.NotifyFilter = NotifyFilters.LastAccess
  | NotifyFilters.LastWrite
  | NotifyFilters.FileName
  | NotifyFilters.DirectoryName;

// Only watch text files.
watcher.Filter = "*.txt";

// Add event handlers.
// Specify what is done when a file is changed, created, or deleted.
watcher.Changed += (s, e) =>
    Console.WriteLine($"File: {e.FullPath} {e.ChangeType}!");
watcher.Created += (s, e) =>
    Console.WriteLine($"File: {e.FullPath} {e.ChangeType}!");
watcher.Deleted += (s, e) =>
    Console.WriteLine($"File: {e.FullPath} {e.ChangeType}!");
// Specify what is done when a file is renamed.
watcher.Renamed += (s, e) =>
    Console.WriteLine($"File: {e.OldFullPath} renamed to {e.FullPath}");
// Begin watching the directory.
watcher.EnableRaisingEvents = true;

// Wait for the user to quit the program.
Console.WriteLine(@"Press 'q' to quit app.");
// Raise some events.
using (var sw = File.CreateText("Test.txt"))
{
    sw.Write("This is some text");
}
File.Move("Test.txt","Test2.txt");
File.Delete("Test2.txt");

while(Console.Read()!='q');
```

当你运行这个程序时，最后几行将创建、更改、重命名然后删除一个文本文件，并在此过程中引发事件。你也可以导航到`bin\debug\net5.0`目录，摆弄一下`*.txt`扩展名的文件，并引发额外的事件。

```
**** The Amazing File Watcher App ****
Press 'q' to quit app.
File: .\Test.txt Created!
File: .\Test.txt Changed!
File: .\Test.txt renamed to .\Test2.txt
File: .\Test2.txt Deleted!
```

这就结束了本章关于.NET Core平台中基本I/O操作的内容。虽然你肯定会在许多应用程序中使用这些技术，但你也可能会发现对象序列化服务可以极大地简化如何持久化大量数据。

## 理解对象序列化

术语*序列化*描述了将对象的状态持久化（并可能传输）到流（例如文件流或内存流）中的过程。持久化的数据序列包含了你稍后重建（或*反序列化*）对象的公共状态所需的所有必要信息。使用这项技术可以轻松地保存大量数据。在许多情况下，使用序列化服务保存应用程序数据，比使用System.IO命名空间中的读写器所需的代码更少。

例如，假设你想创建一个基于GUI的桌面应用程序，为最终用户提供保存其偏好设置（例如窗口颜色和字体大小）的途径。为此，你可以定义一个名为UserPrefs的类，它封装了大约20个字段数据。现在，如果你使用System.IO.BinaryWriter类型，你需要手动保存UserPrefs对象的每个字段。同样，如果你要从文件将数据加载回内存，你需要使用System.IO.BinaryReader，并（再次）手动读取每个值以重新配置一个新的UserPrefs对象。

这一切都是可行的，但你可以通过使用可扩展标记语言（XML）或JavaScript对象表示法（JSON）序列化来节省大量时间。这两种格式都由名称-值对组成，允许在单个文本块中表示对象的公共状态，该文本块可跨平台和编程语言使用。这样做意味着你只需几行代码就可以持久化对象的整个公共状态。

注意 本书之前版本中介绍的BinaryFormatter类型存在高风险安全隐患，你应立即停止使用它（http://aka.ms/binaryformatter）。更安全的替代方案包括使用BinaryReaders/BinaryWriters来处理XML/JSON。

.NET Core对象序列化使持久化对象变得容易；然而，幕后使用的过程相当复杂。例如，当一个对象被持久化到流中时，所有关联的公共数据（例如基类数据和包含的对象）也会自动序列化。因此，如果你尝试持久化一个派生类，继承链上的所有公共数据也会一并处理。正如你将看到的，你会使用对象图来表示一组相互关联的对象。

最后，请理解你可以将对象图持久化到任何System.IO.Stream派生类型中。重要的是数据序列能正确表示图中的对象状态。

### 对象图的作用

如前所述，.NET运行时将考虑所有相关对象，以确保在序列化对象时正确持久化公共数据。这组相关对象被称为对象图。对象图提供了一种简单的方式来记录一组项如何相互引用。对象图并不表示OOP中的is-a或has-a关系。相反，你可以将对象图中的箭头理解为“需要”或“依赖于”。

对象图中的每个对象都被分配一个唯一的数值。请记住，分配给对象图中成员的数值是任意的，对外部世界没有实际意义。一旦你为所有对象分配了数值，对象图就可以记录每个对象的依赖集。

例如，假设你创建了一组模拟某些汽车（当然）的类。你有一个名为Car的基类，它有一个Radio。另一个名为JamesBondCar的类扩展了Car基类型。图20-1展示了建模这些关系的一个可能的对象图。

