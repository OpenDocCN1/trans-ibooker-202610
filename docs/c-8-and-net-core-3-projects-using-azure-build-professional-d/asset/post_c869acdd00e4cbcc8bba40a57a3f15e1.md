
> As with any other class, you should consider whether your extension method class is getting too large, or becoming a dumping ground for unrelated functionality, or functionality that may be better extracted into a self-contained class.

## The DocumentEngine class

The purpose of this class is merely to provide supporting code to a document. In the eBookManager application, I am going to use a single method called GetFileProperties() that will (you guessed it) return the properties of a selected file. This class also only contains this single method. As the application is modified for your specific purposes, you can modify this class and add additional methods that are specific to documents.

Inside the DocumentEngine class, add the following code:

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

The GetFileProperties() method returns a tuple as (DateTime dateCreated,
DateTime dateLastAccessed, string fileName, string fileExtension, long
fileLength, bool error) and allows us to inspect the values returned from the calling
code easily.

Before getting the properties of the specific file, the tuple is initialized by doing the following:

```
var returnTuple = (created: DateTime.MinValue, lastDateAccessed:
DateTime.MinValue, name: "", ext: "", fileSize: 0L, error: false);
```

If there is an exception, I can return default values. Reading the file properties is simple enough using the FileInfo class. I can then assign the file properties to the tuple by doing this:

```
returnTuple = (fi.CreationTime, fi.LastAccessTime, fi.Name, fi.Extension,
fi.Length, false);
```

The tuple is then returned to the calling code, where it will be used as required. We will have a look at the calling code next.

## The ImportBooks form

The ImportBooks form does exactly what the name suggests. It allows us to create virtual storage spaces and to import books into those spaces. The form design is as follows:

![](img/762492a8287f369cc1ac4621a212f193_38_0.png)

TreeView controls are prefixed with `tv`, buttons with `btn`, combo boxes with `dl`, textboxes with `txt`, and date time pickers with `dt`.

Although this kind of prefixing isn't widely used today, this used to be a common practice for WinForms developers. The reason behind it is that WinForms never really lent itself very well to a separation of business and presentation layers (there have been attempts to rectify this, notably with the MVP pattern), meaning that referencing controls directly from code-behind was a common practice and, as such, it made sense to indicate the type of control you were dealing with.

When this form loads, if any storage spaces have been defined then they will be listed in the `dlVirtualStorageSpaces` combo box. Clicking on the **Select source folder** button will allow us to select a source folder in which to look for eBooks.

If a storage space does not exist, we can add a new virtual storage space by clicking the `btnAddNewStorageSpace` button. This will allow us to add a name and description for the new storage space and click on the `btnSaveNewStorageSpace` button. Selecting an eBook from the `tvFoundBooks` TreeView will populate the **File details** group of controls to the right of the form. You can then add additional **Book details** and click on the `btnAddBookToStorageSpace` button to add the book to our space.

You can access the code-behind of a Windows Form by simply pressing `F7`, or right-clicking in Solution Explorer and selecting **View Code**.

The following steps describe changes to be made to the `ImportBooks` code-behind:

- You need to ensure that the following namespaces are added to your class (these should replace any existing namespaces here):

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

- Next, let's start at the most logical place: the `ImportBooks()` constructor and the class-level variables. Add the following declarations above the constructor:

```csharp
private string _jsonPath;
private List<StorageSpace> _spaces;
private enum _storageSpaceSelection { New = -9999, NoSelection = -1 }
```

The usefulness of the enumerator will become evident later on in the code. The `_jsonPath` variable will contain the path to the file used to store our eBook information.

> Some people, including myself, like to prefix private class-level variables with an underscore (as in this example). This is a personal preference; however, there are settings in Visual Studio that will aid in the auto-generation of such variables if you tell it what your preference is.

- Modify the constructor as follows:

```csharp
public ImportBooks()
{
    InitializeComponent();

    _jsonPath = Path.Combine(Application.StartupPath,
    "bookData.txt");
}
```

_jsonPath is initialized in the executing folder for the application and the file is hardcoded to bookData.txt. You could provide a settings screen to configure these settings if you chose to improve this project.

- Because we want to load some data when the form loads, we’ll attach the Form_Load event. An easy way to create an event handler in WinForms is to select the event in the form designer and simply double-click next to the event that you wish to handle:

![](img/762492a8287f369cc1ac4621a212f193_41_0.png)

The new event should load the following code from the data store asynchronously:

```
private async void ImportBooks_Load(object sender, EventArgs e)
{
    _spaces = await _spaces.ReadFromDataStore(_jsonPath);
}
```

- Next, we need to add another two enumerators that define the file extensions that we will be able to save in our application:

```
private HashSet<string> AllowedExtensions => new
HashSet<string>(StringComparer.InvariantCultureIgnoreCase)
{ ".doc", ".docx", ".pdf", ".epub", ".lit" };
```

```
private enum Extension { doc = 0, docx = 1, pdf = 2, epub = 3, lit = 4 }
```

We can see the implementation of the AllowedExtensions property when we look at the
PopulateBookList() method.

## Populating the TreeView control

All that the PopulateBookList() method does is populate the TreeView control with
files and folders found at the selected source location. Consider the following code in the
ImportBooks code-behind:

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

The first place we need to call this method is obviously from within itself, as this is a recursive method. The second place we need to call it is from the btnSelectSourceFolder button click event (again, as before, select the click property and double-click):

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

This is all quite straightforward code. Select the folder to recurse and populate the TreeView control with all the files found that match the file extension contained in our AllowedExtensions property. We also need to look at the code when someone selects a book in the tvFoundBooks TreeView control. When a book is selected, we need to read the properties of the selected file and return those properties to the file details section:

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
