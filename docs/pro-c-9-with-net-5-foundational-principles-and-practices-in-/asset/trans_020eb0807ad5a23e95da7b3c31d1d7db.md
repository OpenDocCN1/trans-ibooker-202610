
双击解决方案资源管理器中的 MainWindow.xaml 文件，并将 XAML 替换为以下内容：

```
<Window x:Class="DataParallelismWithForEach.MainWindow"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"

    xmlns:local="clr-namespace:DataParallelismWithForEach"
    mc:Ignorable="d"
    Title="Fun with TPL" Height="400" Width="800">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
            <RowDefinition Height="Auto"/>
        </Grid.RowDefinitions>
        <Label Grid.Row="0" Grid.Column="0">
            Feel free to type here while the images are processed...
        </Label>
        <TextBox Grid.Row="1" Grid.Column="0" Margin="10,10,10,10"/>
        <Grid Grid.Row="2" Grid.Column="0">
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="Auto"/>
                <ColumnDefinition Width="*"/>
                <ColumnDefinition Width="Auto"/>
            </Grid.ColumnDefinitions>
            <Button Name="cmdCancel" Grid.Row="0" Grid.Column="0" Margin="10,10,0,10"
                Click="cmdCancel_Click">
                Cancel
            </Button>
            <Button Name="cmdProcess" Grid.Row="0" Grid.Column="2" Margin="0,10,10,10"
                Click="cmdProcess_Click">
                Click to Flip Your Images!
            </Button>
        </Grid>
    </Grid>
</Window>
```

同样，不用纠结这些标记的含义或工作原理；你马上就要花大量时间学习 WPF。该应用程序的 GUI 由一个多行 TextBox 和一个按钮（名为 cmdProcess）组成。文本区域的作用是让你在后台执行工作时输入数据，从而展示并行任务的非阻塞特性。

对于此示例，需要一个额外的 NuGet 包（System.Drawing.Common）。要将其添加到项目中，请在命令行（与你的解决方案文件位于同一目录）或 Visual Studio 的包管理器控制台中输入以下一行（全部在同一行）：

```
dotnet add DataParallelismWithForEach package System.Drawing.Common
```

打开 MainWindow.xaml.cs 文件（在 Visual Studio 中双击它——你可能需要展开 MainWindow.xaml 旁边的节点），然后在文件顶部添加以下 using 语句：

```
// Be sure you have these namespaces! (System.Threading.Tasks should already be there from the default template)
using System;
using System.Drawing;
using System.Threading.Tasks;
using System.Threading;
using System.Windows;
using System.IO;
```

注意：你应该更新传入以下 Directory.GetFiles() 方法调用的字符串，使其指向你计算机上包含一些图像文件的路径（例如存放家庭照片的个人文件夹）。为了方便你，我已将一些示例图像（随 Windows 操作系统附带）包含在解决方案目录中。

```
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
    }

    private void cmdCancel_Click(object sender, EventArgs e)
    {
        // This will be updated shortly
    }

    private void cmdProcess_Click(object sender, EventArgs e)
    {
        ProcessFiles();
        this.Title = "Processing Complete";
    }

    private void ProcessFiles()
    {
        // Load up all *.jpg files, and make a new folder for the
        // modified data.
        //Get the directory path where the file is executing
        //For VS 2019 debugging, the current directory will be <projectdirectory>\bin\debug\net5.0-windows
        //For VS Code or "dotnet run", the current directory will be <projectdirectory>
        var basePath = Directory.GetCurrentDirectory();
        var pictureDirectory =
            Path.Combine(basePath, "TestPictures");
        var outputDirectory =
            Path.Combine(basePath, "ModifiedPictures");
```

```
//Clear out any existing files
if (Directory.Exists(outputDirectory))
{
    Directory.Delete(outputDirectory, true);
}
Directory.CreateDirectory(outputDirectory);
string[] files = Directory.GetFiles(pictureDirectory,
    "*.jpg", SearchOption.AllDirectories);

// Process the image data in a blocking manner.
foreach (string currentFile in files)
{
    string filename =
        System.IO.Path.GetFileName(currentFile);
    // Print out the ID of the thread processing the current image.
    this.Title = $"Processing {filename} on thread {Thread.CurrentThread.ManagedThreadId}";
    using (Bitmap bitmap = new Bitmap(currentFile))
    {
        bitmap.RotateFlip(RotateFlipType.Rotate180FlipNone);
        bitmap.Save(System.IO.Path.Combine(
            outputDirectory, filename));
    }
}
```

> 注意：如果你收到错误，指出 Path 在 System.IO.Path 和 System.Windows.Shapes.Path 之间存在歧义引用，请移除 System.Windows.Shapes 的 using，或者为 Path 添加 System.IO：System.IO.Path.Combine(...)。

注意，ProcessFiles() 方法将旋转指定目录下的每个 *.jpg 文件。目前，所有工作都在可执行文件的主线程上进行。因此，如果单击按钮，程序将看起来像挂起一样。此外，窗口的标题也会显示同一个主线程正在处理文件，因为我们只有一个执行线程。

为了尽可能利用多 CPU 处理文件，你可以将当前的 foreach 循环重写为使用 Parallel.ForEach()。回想一下，这个方法已被多次重载；然而，在最简单的形式中，你必须指定一个与 IEnumerable<T> 兼容的对象，该对象包含要处理的项（即 files 字符串数组），以及一个 Action<T> 委托，该委托指向将执行工作的那个方法。

下面是相关更新，使用 C# lambda 运算符代替字面量的 Action<T> 委托对象。注意，你当前已经注释掉了显示执行当前图像文件的线程 ID 的那行代码。请参阅下一部分以了解原因。

```
// Process the image data in a parallel manner!
Parallel.ForEach(files, currentFile =>
{
    string filename = Path.GetFileName(currentFile);
```

```
// This code statement is now a problem! See next section.
// this.Title = $"Processing {filename} on thread
//     {Thread.CurrentThread.ManagedThreadId}"
// Thread.CurrentThread.ManagedThreadId);

using (Bitmap bitmap = new Bitmap(currentFile))
{
    bitmap.RotateFlip(RotateFlipType.Rotate180FlipNone);
    bitmap.Save(Path.Combine(outputDirectory, filename));
}
);
```

## 在辅助线程上访问 UI 元素

你会注意到，我已注释掉了之前更新主窗口标题以显示当前执行线程 ID 的那行代码。如前所述，GUI 控件与其创建线程具有“线程关联性”。如果辅助线程尝试访问它们未直接创建的控件，那么在调试软件时，你必然会遇到运行时错误。另一方面，如果你运行该应用程序（通过 Ctrl+F5），你可能根本不会发现原始代码有任何问题。

> 注意：让我重申前面的观点：当你调试多线程应用程序时，有时可以捕获到当辅助线程“触碰”在主线程上创建的控件时出现的错误。但是，很多时候当你运行应用程序时，它可能看起来运行正常（或者可能立即出错）。在你采取预防措施（将在下一节讨论）之前，你的应用程序在这种情况下有可能引发运行时错误。

你可以使用的一种方法，让这些辅助线程以线程安全的方式访问控件，是另一种以委托为中心的技术，具体来说是匿名委托。WPF 中的 Control 父类定义了一个 Dispatcher 对象，该对象管理线程的工作项。该对象有一个名为 Invoke() 的方法，它接受一个 System.Delegate 作为输入。当你处于涉及辅助线程的编码环境中时，可以调用此方法，以线程安全的方式更新给定控件的 UI。现在，虽然你可以直接编写所有必需的委托代码，但大多数开发人员使用表达式语法作为简单替代。以下是针对先前注释掉的代码语句的相关更新：

```
// Eek! This will not work anymore!
//this.Title = $"Processing {filename} on thread {Thread.CurrentThread.ManagedThreadId}";

// Invoke on the Form object, to allow secondary threads to access controls
// in a thread-safe manner.
Dispatcher?.Invoke(() =>
{
    this.Title = $"Processing {filename}";
});
using (Bitmap bitmap = new Bitmap(currentFile))
{
```
