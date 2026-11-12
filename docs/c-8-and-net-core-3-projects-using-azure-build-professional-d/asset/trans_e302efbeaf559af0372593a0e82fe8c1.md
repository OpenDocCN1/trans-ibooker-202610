

dlVirtualStorageSpaces.Enabled = true;
btnAddNewStorageSpace.Enabled = true;
}

private void btnAddNewStorageSpace_Click(object sender, EventArgs e)
{
    txtNewStorageSpaceName.Visible = true;
    lblStorageSpaceDescription.Visible = true;
    txtStorageSpaceDescription.ReadOnly = false;
    btnSaveNewStorageSpace.Visible = true;
    btnCancelNewStorageSpaceSave.Visible = true;
    dlVirtualStorageSpaces.Enabled = false;
    btnAddNewStorageSpace.Enabled = false;
}

现在只需在启动窗体 Form1.cs 中完成代码即可。

## 创建主 eBookManager 窗体

首先将 Form1.cs 重命名为 eBookManager.cs。这是应用程序的启动窗体，将列出之前保存的所有现有存储空间：

![](img/762492a8287f369cc1ac4621a212f193_50_0.png)

按以下方式设计 eBookManager 窗体：

- 用于现有存储空间的 ListView 控件
- 用于选中存储空间中所包含电子书的 ListView
- 用于打开电子书文件位置的按钮
- 用于导航到 ImportBooks.cs 窗体的菜单控件
- 用于显示所选电子书信息的各种只读字段：

![](img/762492a8287f369cc1ac4621a212f193_51_0.png)

> 同样，由于 WinForms 设计器的特性，您可以直接从仓库复制并粘贴设计器代码。

本节将需要以下 using 语句：

```
using eBookManager.Engine;
using eBookManager.Helper;
using System;
using System.Collections.Generic;
using System.IO;
using System.Windows.Forms;
using System.Linq;
using System.Diagnostics;
```

如前所示，您可以选择省略此步骤，并在每次无法识别特定方法或命名空间时按 Ctrl + . 。

请注意，您无法通过此方式包含带有扩展方法的库，因此需要手动包含 eBookManager.Helper。

现在，我们按照以下步骤开始设计 eBookManager 窗体：

- 1. 构造函数和加载方法与 ImportBooks.cs 窗体中的类似。它们会读取任何可用的存储空间，并使用之前保存的存储空间填充存储空间列表视图控件：

```
private string _jsonPath;
private List<StorageSpace> _spaces;

public eBookManager()
{
    InitializeComponent();
    _jsonPath = Path.Combine(Application.StartupPath,
        "bookData.txt");
}

private async void eBookManager_Load(object sender, EventArgs e)
{
    _spaces = await _spaces.ReadFromDataStore(_jsonPath);

    // imageList1
    this.imageList1.Images.Add("storage_space_cloud.png",
        Image.FromFile("img/storage_space_cloud.png"));
    this.imageList1.Images.Add("eBook.png",
        Image.FromFile("img/eBook.png"));
    this.imageList1.Images.Add("no_eBook.png",
        Image.FromFile("img/no_eBook.png"));
    this.imageList1.TransparentColor =
        System.Drawing.Color.Transparent;

    // btnReadEbook
    this.btnReadEbook.Image = Image.FromFile("img/ReadEbook.png");
    this.btnReadEbook.Location = new System.Drawing.Point(103,
        227);
    this.btnReadEbook.Name = "btnReadEbook";
    this.btnReadEbook.Size = new System.Drawing.Size(36, 40);
    this.btnReadEbook.TabIndex = 32;
    this.toolTip1.SetToolTip(this.btnReadEbook, "Click here to open the eBook file location");
    this.btnReadEbook.UseVisualStyleBackColor = true;
    this.btnReadEbook.Click += new System.EventHandler(this.btnReadEbook_Click);
// EbookManager Icon
    this.Icon = new System.Drawing.Icon("ico/mainForm.ico");
    PopulateStorageSpaceList();
}
```

```
private void PopulateStorageSpaceList()
{
    lstStorageSpaces.Clear();
    if (!(_spaces == null))
    {
        foreach (StorageSpace space in _spaces)
        {
            ListViewItem lvItem = new ListViewItem(space.Name, 0);
            lvItem.Tag = space.BookList;
            lvItem.Name = space.ID.ToString();
            lstStorageSpaces.Items.Add(lvItem);
        }
    }
}
```

- 2. 如果用户单击某个存储空间，我们需要能够读取该选中空间中包含的图书：

```
private void lstStorageSpaces_MouseClick(object sender,
    MouseEventArgs e)
{
    ListViewItem selectedStorageSpace =
    lstStorageSpaces.SelectedItems[0];
    int spaceID = selectedStorageSpace.Name.ToInt();
    txtStorageSpaceDescription.Text = (from d in _spaces
    where d.ID == spaceID
    select d.Description).First();
    List<Document> ebookList =
    (List<Document>) selectedStorageSpace.Tag;
    PopulateContainedEbooks(ebookList);
}
```

- 3. 现在需要创建一个方法，用填充选中存储空间中包含的图书来填充 `lstBooks` 列表视图：

```
private void PopulateContainedEbooks(List<Document> ebookList)
{
    lstBooks.Clear();
    ClearSelectedBook();
    if (ebookList != null)
    {
        foreach (Document eBook in ebookList)
        {
            ListViewItem book = new ListViewItem(eBook.Title, 1);
            book.Tag = eBook;
            lstBooks.Items.Add(book);
        }
    }
    else
    {
        ListViewItem book = new ListViewItem("This storage space contains no eBooks", 2);
        book.Tag = "";
        lstBooks.Items.Add(book);
    }
}
```

- 4. 当更改选中的存储空间时，我们还需要清除所选图书的详细信息。我在文件和图书详细信息周围创建了两个组控件。此代码仅遍历所有子控件；如果子控件是文本框，则将其清除：

```
private void ClearSelectedBook()
{
    foreach (Control ctrl in gbBookDetails.Controls)
    {
        if (ctrl is TextBox)
            ctrl.Text = "";
    }
    foreach (Control ctrl in gbFileDetails.Controls)
    {
        if (ctrl is TextBox)
            ctrl.Text = "";
    }
    dtLastAccessed.Value = DateTime.Now;
    dtCreated.Value = DateTime.Now;
    dtDatePublished.Value = DateTime.Now;
}
```

- 5. 添加到窗体的 MenuStrip 在 ImportEbooks 菜单项上有一个单击事件。它只需打开 ImportBooks 窗体：

```csharp
private async void mnuImportEbooks_Click(object sender, EventArgs e)
{
    ImportBooks import = new ImportBooks();
    import.ShowDialog();
    _spaces = await _spaces.ReadFromDataStore(_jsonPath);
    PopulateStorageSpaceList();
}
```

- 6. 以下方法封装了选择特定电子书并在 eBookManager 窗体上填充文件和电子书详细信息的逻辑：

```csharp
private void lstBooks_MouseClick(object sender, MouseEventArgs e)
{
    ListViewItem selectedBook = lstBooks.SelectedItems[0];
    if (!String.IsNullOrEmpty(selectedBook.Tag.ToString()))
    {
        Document ebook = (Document)selectedBook.Tag;
        txtFileName.Text = ebook.FileName;
        txtExtension.Text = ebook.Extension;
        dtLastAccessed.Value = ebook.LastAccessed;
        dtCreated.Value = ebook.Created;
        txtFilePath.Text = ebook.FilePath;
        txtFileSize.Text = ebook.FileSize;
        txtTitle.Text = ebook.Title;
        txtAuthor.Text = ebook.Author;
        txtPublisher.Text = ebook.Publisher;
        txtPrice.Text = ebook.Price;
        txtISBN.Text = ebook.ISBN;
        dtDatePublished.Value = ebook.PublishDate;
        txtCategory.Text = ebook.Category;
    }
}
```

- 7. 最后，当所选图书是您希望阅读的图书时，单击“读取电子书”按钮以打开所选电子书的文件位置：

```csharp
private void btnReadEbook_Click(object sender, EventArgs e)
{
    string filePath = txtFilePath.Text;
    FileInfo fi = new FileInfo(filePath);
    if (fi.Exists)
    {
        Process.Start("explorer.exe",
        Path.GetDirectoryName(filePath));
    }
}
```

这完成了 eBookManager 应用程序中包含的代码逻辑。

> 您可以进一步修改代码，为所选电子书打开所需的应用程序，而不仅仅是打开文件位置。换句话说，如果您单击 PDF 文档，应用程序可以启动带有已加载文档的 PDF 阅读器。最后，请注意此版本的应用程序尚未实现分类功能。

是时候启动应用程序并进行测试了。

## 运行 eBookManager 应用程序

要运行应用程序，请执行以下步骤：

- 1. 当应用程序首次启动时，将没有可用的虚拟存储空间。要创建一个，我们需要导入一些图书。单击“导入电子书”菜单项：

![](img/762492a8287f369cc1ac4621a212f193_56_0.png)

