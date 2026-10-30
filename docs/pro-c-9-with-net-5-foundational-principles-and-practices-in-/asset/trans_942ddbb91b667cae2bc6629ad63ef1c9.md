

```csharp
bitmap.RotateFlip(RotateFlipType.Rotate180FlipNone);
bitmap.Save(Path.Combine(outputDirectory, filename));
}
```

现在，如果你运行程序，TPL 确实会将工作负载分发到线程池中的多个线程，使用尽可能多的 CPU。然而，由于标题始终从主线程更新，标题更新代码不再显示当前线程，并且在所有图像处理完成之前，如果你在文本框中输入内容，你将看不到任何内容！原因在于主 UI 线程仍然被阻塞，等待所有其他线程完成其工作。

## Task 类

Task 类允许你轻松地在一个次要线程上调用一个方法，并且可以用作使用异步委托的简单替代方案。按如下方式更新你的 Button 控件的 Click 处理程序：

```csharp
private void cmdProcess_Click(object sender, EventArgs e)
{
    // Start a new "task" to process the files.
    Task.Factory.StartNew(() => ProcessFiles());
    // Can also be written this way
    // Task.Factory.StartNew(ProcessFiles);
}
```

Task 的 Factory 属性返回一个 TaskFactory 对象。当你调用其 StartNew() 方法时，你传入一个 Action<T> 委托（在这里，使用合适的 lambda 表达式隐藏），该委托指向要在异步方式下调用的方法。通过这个小的更新，你现在会发现窗口的标题将显示线程池中哪个线程正在处理给定的文件，更好的是，文本区域能够接收输入，因为 UI 线程不再被阻塞。

## 处理取消请求

对当前示例的一个改进是提供一种让用户通过第二个恰当地命名的 Cancel 按钮停止图像数据处理的方法。值得庆幸的是，Parallel.For() 和 Parallel.ForEach() 方法都支持使用取消令牌进行取消。当你在 Parallel 上调用方法时，你可以传入一个 ParallelOptions 对象，而该对象反过来包含一个 CancellationTokenSource 对象。

首先，在你的 Form 派生类中定义以下新的私有成员变量，类型为 CancellationTokenSource，命名为 _cancelToken：

```csharp
public partial class MainWindow : Window
{
    // New Window-level variable.
    private CancellationTokenSource _cancelToken = new CancellationTokenSource();
    ...
}
```

将 Cancel 按钮 Click 事件更新为以下代码：

```csharp
private void cmdCancel_Click(object sender, EventArgs e)
{
    // This will be used to tell all the worker threads to stop!
    _cancelToken.Cancel();
}
```

现在，真正的修改需要在 ProcessFiles() 方法内进行。考虑最终的实现：

```csharp
private void ProcessFiles()
{
    // Use ParallelOptions instance to store the CancellationToken.
    ParallelOptions parOpts = new ParallelOptions();
    parOpts.CancellationToken = _cancelToken.Token;
    parOpts.MaxDegreeOfParallelism = System.Environment.ProcessorCount;

    // Load up all *.jpg files, and make a new folder for the modified data.
    string[] files = Directory.GetFiles(@".\TestPictures", "*.jpg", SearchOption.AllDirectories);
    string outputDirectory = @".\ModifiedPictures";
    Directory.CreateDirectory(outputDirectory);

    try
    {
        // Process the image data in a parallel manner!
        Parallel.ForEach(files, parOpts, currentFile =>
        {
            parOpts.CancellationToken.ThrowIfCancellationRequested();

            string filename = Path.GetFileName(currentFile);
            Dispatcher?.Invoke(() =>
            {
                this.Title = $"Processing {filename} on thread {Thread.CurrentThread.ManagedThreadId}";
            });
            using (Bitmap bitmap = new Bitmap(currentFile))
            {
                bitmap.RotateFlip(RotateFlipType.Rotate180FlipNone);
                bitmap.Save(Path.Combine(outputDirectory, filename));
            }
        });
        Dispatcher?.Invoke(() => this.Title = "Done!");
    }
    catch (OperationCanceledException ex)
    {
        Dispatcher?.Invoke(() => this.Title = ex.Message);
    }
}
```

注意你开始方法时通过配置一个 ParallelOptions 对象，将 CancellationToken 属性设置为使用 CancellationTokenSource 令牌。还要注意当你调用 Parallel.ForEach() 方法时，你将 ParallelOptions 对象作为第二个参数传入。

在循环逻辑的范围内，你对令牌调用 ThrowIfCancellationRequested()，这将确保如果用户点击 Cancel 按钮，所有线程将停止，并且你将通过运行时异常收到通知。当你捕获 OperationCanceledException 错误时，你将把主窗口的文本设置为错误消息。

## 使用 Parallel 类进行任务并行

除了数据并行之外，TPL 还可以使用 Parallel.Invoke() 方法轻松启动任意数量的异步任务。这种方法比使用 System.Threading 中的成员更直接；然而，如果你需要更多地控制任务的执行方式，你可以放弃使用 Parallel.Invoke() 并直接使用 Task 类，就像你在前一个示例中所做的那样。

为了说明任务并行，创建一个名为 MyEBookReader 的新控制台应用程序，并确保在 Program.cs 顶部导入 System.Threading、System.Text、System.Threading.Tasks、System.Linq 和 System.Net 命名空间（此示例是对 .NET Core 文档中有用的示例的修改）。在这里，你将从 Project Gutenberg（www.gutenberg.org）获取一本公开可用的电子书，然后并行执行一组耗时的任务。

书籍在 GetBook() 方法中下载，显示如下：

```csharp
using System;
using System.Linq;
using System.Threading;
using System.Threading.Tasks;
using System.Net;
using System.Text;

string _theEBook = "";
GetBook();
Console.WriteLine("Downloading book...");
Console.ReadLine();

void GetBook()
{
    WebClient wc = new WebClient();
    wc.DownloadStringCompleted += (s, eArgs) =>
    {
        _theEBook = eArgs.Result;
        Console.WriteLine("Download complete.");
        GetStats();
    };

    // The Project Gutenberg EBook of A Tale of Two Cities, by Charles Dickens
    // You might have to run it twice if you’ve never visited the site before, since the first
    // time you visit there is a message box that pops up, and breaks this code.
    wc.DownloadStringAsync(new Uri("http://www.gutenberg.org/files/98/98-8.txt"));
}
```

WebClient 类是 System.Net 的成员。此类提供用于向 URI 标识的资源发送数据和从该资源接收数据的方法。事实证明，其中许多方法都有异步版本，例如 DownloadStringAsync()。此方法将自动从 .NET Core Runtime 线程池启动一个新线程。当 WebClient 完成获取数据后，它将触发 DownloadStringCompleted 事件，你在这里使用 C# lambda 表达式进行处理。如果调用此方法的同步版本（DownloadString()），“Downloading” 消息要到下载完成才会被显示。

接下来，GetStats() 方法被实现为提取 _theEBook 变量中包含的各个单词，然后将字符串数组传递给几个辅助函数进行处理，如下所示：

```csharp
void GetStats()
{
    // Get the words from the ebook.
    string[] words = _theEBook.Split(new char[]
    { ' ', '\u000A', ',', '.', ';', ':', '-', '?', '/' },
        StringSplitOptions.RemoveEmptyEntries);

    // Now, find the ten most common words.
    string[] tenMostCommon = FindTenMostCommon(words);

    // Get the longest word.
    string longestWord = FindLongestWord(words);

    // Now that all tasks are complete, build a string to show all stats.
    StringBuilder bookStats = new StringBuilder("Ten Most Common Words are:\n");
    foreach (string s in tenMostCommon)
    {
        bookStats.AppendLine(s);
    }

    bookStats.AppendFormat("Longest word is: {0}", longestWord);
    bookStats.AppendLine();
    Console.WriteLine(bookStats.ToString(), "Book info");
}
```

FindTenMostCommon() 方法使用 LINQ 查询来获取字符串数组中最常出现的字符串对象列表，而 FindLongestWord() 定位，嗯，最长的单词。

```csharp
string[] FindTenMostCommon(string[] words)
{
    var frequencyOrder = from word in words
                          where word.Length > 6
                          group word by word into g
                          orderby g.Count() descending
                          select g.Key;
    string[] commonWords = (frequencyOrder.Take(10)).ToArray();
    return commonWords;
}

string FindLongestWord(string[] words)
{
    return (from w in words orderby w.Length descending select w).FirstOrDefault();
}
```

