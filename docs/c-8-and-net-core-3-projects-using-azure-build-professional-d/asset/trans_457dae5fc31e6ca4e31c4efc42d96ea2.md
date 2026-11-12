

> 与任何其他类一样，你应该考虑你的扩展方法类是否变得过大，或成为不相关功能的倾倒之地，或包含可能更适合提取到独立类中的功能。

## DocumentEngine 类

此类的目的仅仅是为文档提供支持代码。在 eBookManager 应用程序中，我将使用一个名为 GetFileProperties() 的单一方法，它会（你猜对了）返回所选文件的属性。此类也只包含这个单一方法。随着应用程序为你的特定目的进行修改，你可以修改此类并添加特定于文档的其他方法。

在 DocumentEngine 类中，添加以下代码：

```csharp
public (DateTime dateCreated, DateTime dateLastAccessed, string fileName,
string fileExtension, long fileLength, bool error) GetFileProperties(string filePath)
{
    var returnTuple = (created: DateTime.MinValue,
    lastDateAccessed: DateTime.MinValue, name: "", ext: "",
    fileSize: 0L, error: false);
    try
    {
        FileInfo fi = new FileInfo(filePath);
        fi.Refresh();
        returnTuple = (fi.CreationTime, fi.LastAccessTime, fi.Name,
        fi.Extension, fi.Length, false);
    }
    catch
    {
        returnTuple.error = true;
    }
    return returnTuple;
}
```

GetFileProperties() 方法以 (DateTime dateCreated, DateTime dateLastAccessed, string fileName, string fileExtension, long fileLength, bool error) 的形式返回一个元组，并允许我们轻松检查调用代码返回的值。

在获取特定文件的属性之前，元组通过以下方式进行初始化：

```
var returnTuple = (created: DateTime.MinValue, lastDateAccessed:
DateTime.MinValue, name: "", ext: "", fileSize: 0L, error: false);
```

如果有异常，我可以返回默认值。使用 FileInfo 类读取文件属性足够简单。然后我可以通过以下方式将文件属性赋值给元组：

```
returnTuple = (fi.CreationTime, fi.LastAccessTime, fi.Name, fi.Extension,
fi.Length, false);
```

然后将元组返回到调用代码中，按需使用。我们将在下一节查看调用代码。

## ImportBooks 窗体

ImportBooks 窗体正如其名称所示。它允许我们创建虚拟存储空间并将书籍导入这些空间。窗体设计如下：

![](img/762492a8287f369cc1ac4621a212f193_38_0.png)

TreeView 控件以 `tv` 作为前缀，按钮以 `btn`，组合框以 `dl`，文本框以 `txt`，日期时间选择器以 `dt`。

虽然这种前缀做法今天不再广泛使用，但它曾是 WinForms 开发者的常见做法。其背后的原因是 WinForms 从来不太适合业务层与表示层的分离（曾有尝试纠正这一问题，尤其是 MVP 模式），这意味着从代码隐藏文件直接引用控件是一种常见做法，因此表明你正在处理的控件类型是有意义的。

当此窗体加载时，如果已定义任何存储空间，它们将在 `dlVirtualStorageSpaces` 组合框中列出。单击 **选择源文件夹** 按钮将允许我们选择要在其中查找电子书的源文件夹。

如果存储空间不存在，我们可以单击 `btnAddNewStorageSpace` 按钮添加新的虚拟存储空间。这将允许我们为新存储空间添加名称和描述，并单击 `btnSaveNewStorageSpace` 按钮。从 `tvFoundBooks` TreeView 选择电子书将填充窗体右侧的 **文件详细信息** 控件组。然后你可以添加其他 **图书详细信息** 并单击 `btnAddBookToStorageSpace` 按钮将书籍添加到我们的空间中。

你可以通过简单地按下 `F7`，或在“解决方案资源管理器”中右键单击并选择 **查看代码** 来访问 Windows 窗体的代码隐藏文件。

以下步骤描述了对 `ImportBooks` 代码隐藏文件所做的更改：

- 你需要确保将以下命名空间添加到你的类中（这些应该替换这里的任何现有命名空间）：

```
using eBookManager.Engine;
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Windows.Forms;
using static eBookManager.Helper.ExtensionMethods;
using static System.Math;
```

- 接下来，让我们从最合理的地方开始：`ImportBooks()` 构造函数和类级变量。在构造函数之上添加以下声明：

```csharp
private string _jsonPath;
private List<StorageSpace> _spaces;
private enum _storageSpaceSelection { New = -9999, NoSelection = -1 }
```

枚举器的有用性将在代码后面变得明显。`_jsonPath` 变量将包含用于存储我们电子书信息的文件的路径。

> 有些人，包括我自己，喜欢给私有类级变量加下划线前缀（如本示例所示）。这是个人偏好；但是，Visual Studio 中有一些设置可以帮助自动生成此类变量，如果你告诉它你的偏好的话。

- 按如下方式修改构造函数：

```csharp
public ImportBooks()
{
    InitializeComponent();

    _jsonPath = Path.Combine(Application.StartupPath,
    "bookData.txt");
}
```

`_jsonPath` 在应用程序的执行文件夹中初始化，文件被硬编码为 bookData.txt。如果你选择改进此项目，可以提供设置屏幕来配置这些设置。

- 因为我们希望在窗体加载时加载一些数据，所以我们将附加 Form_Load 事件。在 WinForms 中创建事件处理程序的简便方法是在窗体设计器中选择事件，然后在要处理的事件旁双击：

![](img/762492a8287f369cc1ac4621a212f193_41_0.png)

新事件应从数据存储异步加载以下代码：

```
private async void ImportBooks_Load(object sender, EventArgs e)
{
    _spaces = await _spaces.ReadFromDataStore(_jsonPath);
}
```

- 接下来，我们需要添加另外两个枚举器，它们定义了我们应用程序将能够保存的文件扩展名：

```
private HashSet<string> AllowedExtensions => new
HashSet<string>(StringComparer.InvariantCultureIgnoreCase)
{ ".doc", ".docx", ".pdf", ".epub", ".lit" };
```

```
private enum Extension { doc = 0, docx = 1, pdf = 2, epub = 3, lit = 4 }
```

当我们查看 PopulateBookList() 方法时，可以看到 AllowedExtensions 属性的实现。

## 填充 TreeView 控件

PopulateBookList() 方法所做的一切就是用选定源位置找到的文件和文件夹填充 TreeView 控件。请参阅 ImportBooks 代码隐藏文件中的以下代码：

```csharp
public void PopulateBookList(string paramDir, TreeNode paramNode)
{
	DirectoryInfo dir = new DirectoryInfo(paramDir);
	foreach (DirectoryInfo dirInfo in dir.GetDirectories())
	{
		TreeNode node = new TreeNode(dirInfo.Name);
		node.ImageIndex = 4;
		node.SelectedImageIndex = 5;
		if (paramNode != null)
			paramNode.Nodes.Add(node);
		else
			tvFoundBooks.Nodes.Add(node);
		PopulateBookList(dirInfo.FullName, node);
	}
	foreach (FileInfo fleInfo in dir.GetFiles()
	.Where(x => AllowedExtensions.Contains(x.Extension)).ToList())
	{
		TreeNode node = new TreeNode(fleInfo.Name);
		node.Tag = fleInfo.FullName;
		int iconIndex = Enum.Parse(typeof(Extension),
		fleInfo.Extension.TrimStart('.'), true).GetHashCode();
		node.ImageIndex = iconIndex;
		node.SelectedImageIndex = iconIndex;
		if (paramNode != null)
			paramNode.Nodes.Add(node);
		else
			tvFoundBooks.Nodes.Add(node);
	}
}
```

我们需要调用此方法的第一个位置显然是在其内部，因为这是一个递归方法。第二个位置是从 btnSelectSourceFolder 按钮单击事件中调用它（同样，如前所述，选择 click 属性并双击）：

```csharp
private void btnSelectSourceFolder_Click(object sender, EventArgs e)
{
    try
    {
        FolderBrowserDialog fbd = new FolderBrowserDialog();
        fbd.Description = "Select the location of your eBooks and documents";
        DialogResult dlgResult = fbd.ShowDialog();
        if (dlgResult == DialogResult.OK)
        {
            tvFoundBooks.Nodes.Clear();
            string path = fbd.SelectedPath;
            DirectoryInfo di = new DirectoryInfo(path);
            TreeNode root = new TreeNode(di.Name);
            root.ImageIndex = 4;
            root.SelectedImageIndex = 5;
            tvFoundBooks.Nodes.Add(root);
            PopulateBookList(di.FullName, root);
            tvFoundBooks.Sort();
            root.Expand();
        }
    }
    catch (Exception ex)
    {
        MessageBox.Show(ex.Message);
    }
}
```

这全都是相当直接的代码。选择要递归的文件夹，并用所有与我们 AllowedExtensions 属性中包含的文件扩展名匹配的找到的文件填充 TreeView 控件。当有人在 tvFoundBooks TreeView 控件中选择一本书时，我们也需要查看代码。选择一本书时，我们需要读取所选文件的属性，并将这些属性返回到文件详细信息部分：

```csharp
private void tvFoundBooks_AfterSelect(object sender, TreeViewEventArgs e)
{
    DocumentEngine engine = new DocumentEngine();
    string path = e.Node.Tag?.ToString() ?? "";
    if (File.Exists(path))
    {
        var (dateCreated, dateLastAccessed, fileName, fileExtension,
        fileLength, hasError) = engine.GetFileProperties(e.Node.Tag.ToString());
    }
}
```

