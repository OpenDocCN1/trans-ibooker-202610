
- 6. 威尔士语的 ISO 文化代码是什么？
- 7. 本地化、全球化和国际化之间有什么区别？
- 8. 在正则表达式中，$ 表示什么？
- 9. 在正则表达式中，如何表示数字？
- 10. 为什么不应该使用电子邮件地址的官方标准来创建正则表达式以验证用户的电子邮件地址？

## 练习 8.2 – 练习正则表达式

在 Chapter08 解决方案/工作区中，创建一个名为 Exercise02 的控制台应用程序，该应用程序提示用户输入正则表达式，然后提示用户输入一些输入并将两者进行匹配比较，直到用户按下 Esc 为止，如以下输出所示：

```
The default regular expression checks for at least one digit.
Enter a regular expression (or press ENTER to use the default): ^[a-z]+$
Enter some input: apples
apples matches ^[a-z]+$? True
Press ESC to end or any key to try again.
Enter a regular expression (or press ENTER to use the default): ^[a-z]+$
Enter some input: abc123xyz
abc123xyz matches ^[a-z]+$? False
Press ESC to end or any key to try again.
```

## 练习 8.3 – 练习编写扩展方法

在 Chapter08 解决方案/工作区中，创建一个名为 Exercise03 的类库，定义扩展方法以扩展 BigInteger 和 int 等数字类型的方法名为 ToWords，该方法返回描述数字的字符串；例如，18,000,000 为 eighteen million，而 18,456,002,032,011,000,007 为 eighteen quintillion, four hundred and fifty-six quadrillion, two trillion, thirty-two billion, eleven million, and seven。

你可以在以下链接阅读更多关于大数字名称的信息：https://en.wikipedia.org/wiki/Names_of_large_numbers

## 练习 8.4 – 探索主题

使用以下页面上的链接了解本章涵盖主题的更多详细信息：

https://github.com/markjprice/cs10dotnet6/blob/main/book-links.md#chapter-8---working-with-common-net-types

## 摘要

在本章中，你探索了一些用于存储和操作数字、日期和时间以及包括正则表达式在内的文本的类型选择，以及用于存储多项的集合；使用索引、范围和跨度；使用了某些网络资源；反思了代码和属性；使用 Microsoft 推荐的第三方库操作图像；并学习了如何使你的代码国际化。

在下一章中，我们将管理文件和流，编码和解码文本，并执行序列化。

## 09

## 使用文件、流和序列化

本章内容是关于向文件和流读写、文本编码和序列化。

我们将涵盖以下主题：

- 管理文件系统
- 使用流进行读写
- 编码和解码文本
- 序列化对象图
- 控制 JSON 处理

## 管理文件系统

你的应用程序通常需要在不同环境中对文件和目录执行输入和输出操作。System 和 System.IO 命名空间包含用于此目的的类。

## 处理跨平台环境和文件系统

让我们探索如何处理跨平台环境，例如 Windows、Linux 或 macOS 之间的差异。Windows、macOS 和 Linux 的路径不同，因此我们将首先探索 .NET 如何处理这个问题：

- 1. 使用你喜欢的代码编辑器创建一个名为 Chapter09 的新解决方案/工作区。

- 2. 添加控制台应用程序项目，如以下列表中定义：
  - 1. 项目模板：**Console Application**/console
  - 2. 工作区/解决方案文件和文件夹：Chapter09
  - 3. 项目文件和文件夹：WorkingWithFileSystems

- 3. 在 Program.cs 中，添加语句以静态导入 System.Console、System.IO.Directory、System.Environment 和 System.IO.Path 类型，如以下代码所示：

```
using static System.Console;
using static System.IO.Directory;
using static System.IO.Path;
using static System.Environment;
```

- 4. 在 Program.cs 中，创建一个静态 OutputFileSystemInfo 方法，并在其中编写语句执行以下操作：
  - 输出路径和目录分隔字符。
  - 输出当前目录的路径。
  - 输出系统文件、临时文件和文档的一些特殊路径。

```
static void OutputFileSystemInfo()
{
    WriteLine("{0,-33} {1}", arg0: "Path.PathSeparator", arg1: PathSeparator);
    WriteLine("{0,-33} {1}", arg0: "Path.DirectorySeparatorChar", arg1: DirectorySeparatorChar);
    WriteLine("{0,-33} {1}", arg0: "Directory.GetCurrentDirectory()", arg1: GetCurrentDirectory());
    WriteLine("{0,-33} {1}", arg0: "Environment.CurrentDirectory", arg1: CurrentDirectory);
    WriteLine("{0,-33} {1}", arg0: "Environment.SystemDirectory", arg1: SystemDirectory);
    WriteLine("{0,-33} {1}", arg0: "Path.GetTempPath()", arg1: GetTempPath());

    WriteLine("GetFolderPath(SpecialFolder)");
    WriteLine("{0,-33} {1}", arg0: "SpecialFolder.System)", arg1: GetFolderPath(SpecialFolder.System));
    WriteLine("{0,-33} {1}", arg0: "SpecialFolder.ApplicationData)", arg1: GetFolderPath(SpecialFolder.ApplicationData));
    WriteLine("{0,-33} {1}", arg0: "SpecialFolder.MyDocuments)", arg1: GetFolderPath(SpecialFolder.MyDocuments));
    WriteLine("{0,-33} {1}", arg0: "SpecialFolder.Personal)", arg1: GetFolderPath(SpecialFolder.Personal));
}
```

Environment 类型还有许多我们在此代码中未使用的其他有用成员，包括 GetEnvironmentVariables 方法以及 OSVersion 和 ProcessorCount 属性。

- 5. 在 Program.cs 中，在函数上方调用 OutputFileSystemInfo 方法，如以下代码所示：

```
OutputFileSystemInfo();
```

- 6. 运行代码并查看结果，如图 9.1 所示：

![](img/c63b00dca7192cc58cfb9e1f1e64f483_401_0.png)

图 9.1：在 Windows 上运行你的应用程序以显示文件系统信息

使用 Visual Studio Code 通过 dotnet run 运行控制台应用程序时，CurrentDirectory 将是项目文件夹，而不是 bin 内部的文件夹。

良好实践：Windows 使用反斜杠 \ 作为目录分隔字符。macOS 和 Linux 使用正斜杠 / 作为目录分隔字符。在组合路径时，不要在代码中假设使用什么字符。

## 管理驱动器

要管理驱动器，请使用 DriveInfo 类型，它有一个静态方法返回连接到计算机的所有驱动器的信息。每个驱动器都有一个驱动器类型。

让我们探索驱动器：

- 1. 创建一个 WorkWithDrives 方法，并编写语句获取所有驱动器并输出其名称、类型、大小、可用空闲空间和格式，但仅当驱动器已准备就绪时，如以下代码所示：

```
static void WorkWithDrives()
{
WriteLine("{0,-30} | {1,-10} | {2,-7} | {3,18} | {4,18}",
    "NAME", "TYPE", "FORMAT", "SIZE (BYTES)", "FREE SPACE");

foreach (DriveInfo drive in DriveInfo.GetDrives())
{
    if (drive.IsReady)
    {
        WriteLine(
            "{0,-30} | {1,-10} | {2,-7} | {3,18:N0} | {4,18:N0}",
            drive.Name, drive.DriveType, drive.DriveFormat,
            drive.TotalSize, drive.AvailableFreeSpace);
    }
    else
    {
        WriteLine("{0,-30} | {1,-10}", drive.Name, drive.DriveType);
    }
}
```

**良好实践：** 在读取 TotalSize 等属性之前检查驱动器是否已准备就绪，否则在可移动驱动器上会看到异常抛出。

- 2. 在 Program.cs 中，注释掉之前的方法调用并添加对 WorkWithDrives 的调用，如以下代码高亮所示：

```
// OutputFileSystemInfo();
WorkWithDrives();
```

- 3. 运行代码并查看结果，如图 9.2 所示：

![](img/c63b00dca7192cc58cfb9e1f1e64f483_402_0.png)

图 9.2：在 Windows 上显示驱动器信息

## 管理目录

要管理目录，请使用 Directory、Path 和 Environment 静态类。这些类型包含许多用于操作文件系统的成员。
