
Add the following NuGet package to your WinForms project:

```
Microsoft.Windows.SDK.Contracts
```

Install the XamlHost NuGet package into the WinForms app:

```
Install-Package Microsoft.Toolkit.Forms.UI.XamlHost
```

Now we can replace our existing TreeView with the UWP one.

You'll notice that I've fully qualified all the XAML controls. Since we're dealing with two disparate frameworks, this kind of change makes it very easy to get confused and mix up which control you're dealing with.

In the following code samples, I've included class-level variables with the code samples for clarity. I, personally, would suggest that these actually go at the top of your class file. Of course, it makes no functional difference.

The first thing that we need to consider is XamlHost.

## WindowsXamlHost

Let's create our TreeView; we'll do this in the code-behind for ImportBooks.cs. We're going to add some code to the constructor, which will now look as follows:

```
private readonly Microsoft.Toolkit.Forms.UI.XamlHost.WindowsXamlHost
__windowsXamlHostTreeView;

public ImportBooks()
{
	InitializeComponent();
	_jsonPath = Path.Combine(Application.StartupPath, "bookData.txt");
	spaces = spaces.ReadFromDataStore(_jsonPath);

	var windowsXamlHostTreeView = new WindowsXamlHost();
	windowsXamlHostTreeView.InitialTypeName =
		"Windows.UI.Xaml.Controls.TreeView";
	windowsXamlHostTreeView.AutoSizeMode =
		System.Windows.Forms.AutoSizeMode.GrowOnly;
	windowsXamlHostTreeView.Location = new System.Drawing.Point(12, 60);
	windowsXamlHostTreeView.Name = "tvFoundBooks";
	windowsXamlHostTreeView.Size = new System.Drawing.Size(513, 350);
	windowsXamlHostTreeView.TabIndex = 8;
	windowsXamlHostTreeView.Dock = System.Windows.Forms.DockStyle.None;
	windowsXamlHostTreeView.ChildChanged +=
	windowsXamlHostTreeView_ChildChanged;

	this.Controls.Add(windowsXamlHostTreeView);
}
```

Let's quickly review what we've done here (it's actually not that much). Firstly, we've created a new WindowsXamlHost object. This is the basis for XAML Islands; it acts as a wrapper around your UWP control, so it will work in a WinForms context.

Although this chapter discusses WinForms, the same is true for WPF and, while the exact syntax may differ slightly, the basic principle is the same.

The things to notice on this code sample are as follows:

- We're setting the name to tvFoundBooks, which is the same name as our WinForms app had.
- We're listening to the ChildChanged event: this is so that we can set some specifics on the control itself (we'll come back to this shortly).
- The InitialTypeName is how XAML Islands knows which UWP control to invoke.
- We're adding the host control to the current form (we also set the location).

## ItemTemplate

Now that we've set up the host control, we can have a look at the ChildChanged event that we mentioned; this is where we set up the UWP control (rather than the host control):

```
private Windows.UI.Xaml.Controls.TreeView? _tvFoundBooks = null;

private void windowsXamlHostTreeView_ChildChanged(object? sender, EventArgs e)
{
    if (sender == null) return;

    var host = (WindowsXamlHost)sender;
    _tvFoundBooks = (Windows.UI.Xaml.Controls.TreeView)host.Child;
    _tvFoundBooks.ItemInvoked += _tvFoundBooks_ItemInvoked;
    _tvFoundBooks.ItemsSource = DataSource;

    const string Xaml = "<DataTemplate
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"><TreeViewItem ItemsSource="{Binding Children}" Content="{Binding Name}"/></DataTemplate>";
    var xaml = XamlReader.Load(Xaml);
    _tvFoundBooks.ItemTemplate = xaml as Windows.UI.Xaml.DataTemplate;
}
```

Don't worry so much about why _tvFoundBooks is a class-level variable, we'll come back to that shortly. In the preceding code sample, we have a gated check to ensure that sender is not null, and then we're forcing it to a WindowsXamlHost type. Once we have this type, we can get whatever is inside the host by calling the .Child property.

As before, we're listening to the ItemInvoked event (again, we'll come back to this shortly). The first really new thing here is that we're setting the ItemsSource, and the ItemTemplate. We'll come back to ItemsSource, but the template is worth exploring. Unlike WinForms, UWP uses XAML to define how its controls look. This means that you have control over exactly what goes into the TreeView; for example, each node could have an image, or text, or both. However, if you don't specify ItemTemplate, then the rendering engine doesn't know what to display, or how.

The preceding XAML is probably the simplest one that will display anything. You'll notice there are a few binding statements; they are binding to properties relative to the ItemsSource. Let's have a look at exactly what it is we're binding to.

## TreeView Item model and ItemsSource

In order to bind something to a control in UWP, you need something. Essentially, what that means is that we need a model.

> A model, in .NET terms, is simply a class that holds data.

We're going to create a new class, and we'll call it Item:

```
public class Item
 {
     public string Name { get; set; }
     public ObservableCollection<Item> Children { get; set; } = new
     ObservableCollection<Item>();
     public ItemType ItemType { get; set; }
     public string FullName { get; set; }

 public override string ToString()
 {
           return Name;
       }
   }
```

> I would always recommend that models are held in their own file and sit in a folder called Models, but there's no technical reason why you couldn't add this class to the end of ImportBooks.cs.

Most of this class should be self-explanatory; we're holding the `Name` and `FullName` (that is, the name and path) of the file. The `ObservableCollection` is a special type of `Collection` that allows the UI framework to be notified when it changes.

> For the code that we're writing here, we could get away with this simply being a `List`; however, `ObservableCollection` is good practice when dealing with desktop XAML frameworks such as UWP, and this will make extensibility easier.

Finally, we're holding the type of the item, which is a new enumerated type:

```
public enum ItemType
{
    Docx,
    Docxx,
    Pdfx,
    Epubx,
    Folder
}
```

Back in ImportBooks.cs, we're going to set up our ItemsSource. The first step is to add a class-level variable called DataSource:

```
public ObservableCollection<Models.Item> DataSource { get; set; }
```

Our next change is in the btnSelectSourceFolder_Click event handler:

```
private void btnSelectSourceFolder_Click(object sender, EventArgs e)
{
    try
    {
        FolderBrowserDialog fbd = new FolderBrowserDialog();
        fbd.Description = "Select the location of your eBooks and documents";
        DialogResult dlgResult = fbd.ShowDialog();
        if (dlgResult == DialogResult.OK)
        {
            UpdateBookList(fbd.SelectedPath);
        }
    }
    catch (Exception ex)
    {
        MessageBox.Show(ex.Message);
    }
```

As you can see, the new method is hugely simplified compared to the previous version; we've extracted all the real logic into a new method, so let's see that next:

```
private void UpdateBookList(string path)
{
    DirectoryInfo di = new DirectoryInfo(path);
    var bookList = new List<Models.Item>();
    var rootItem = new Models.Item()
    {
        Name = di.Name
    };
    rootItem.ItemType = Models.ItemType.Folder;
    PopulateBookList(di.FullName, rootItem);
    bookList.Add(rootItem);
    DataSource = new ObservableCollection<Models.Item>(bookList);
    _tvFoundBooks.ItemsSource = DataSource.OrderBy(a => a.Name);
}
```

Here, we're setting up the root item of our TreeView; however, you'll notice that the only reference that we actually have to the TreeView is at the end, where we refresh ItemsSource.PopulateBookList is our next port of call. As before, this method is essentially in two parts; let's see the first part:

```
public void PopulateBookList(string paramDir, Models.Item rootItem)
{
    if (rootItem == null) throw new ArgumentNullException();
    rootItem.FullName = paramDir;
    rootItem.ItemType = Models.ItemType.Folder;
    DirectoryInfo dir = new DirectoryInfo(paramDir);
    foreach (DirectoryInfo dirInfo in dir.GetDirectories())
    {
```
