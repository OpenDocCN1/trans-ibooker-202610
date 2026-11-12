
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

All that remains now is for us to complete the code in the Form1.cs form, which is the startup form.

## Creating the main eBookManager form

Start off by renaming Form1.cs to eBookManager.cs. This is the startup form for the application, and it will list all existing storage spaces that were previously saved:

![](img/762492a8287f369cc1ac4621a212f193_50_0.png)

Design your eBookManager form as follows:

- A ListView control for existing storage spaces
- A ListView for eBooks contained in the selected storage space
- A button that opens the file location of the eBook
- A menu control to navigate to the ImportBooks.cs form
- Various read-only fields to display the selected eBook information:

![](img/762492a8287f369cc1ac4621a212f193_51_0.png)

> Again, due to the nature of the WinForms designer, you may choose to simply copy and paste the designer code from the repository.

The following using statements will be needed in this section:

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

As demonstrated earlier, you may choose to omit this and then press Ctrl + . each time a particular method or namespace isn't recognized.

Bear in mind that you won't be able to use this to include libraries with extension methods, so you'll need to include eBookManager.Helper manually.

Let's now start designing our eBookManager form with the help of the following steps:

- 1. The constructor and load methods are quite similar to those in the ImportBooks.cs form. They read any available storage spaces and populate the storage spaces list view control with the previously saved storage spaces:

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

- 2. If the user clicks on a storage space, we need to be able to read the books contained in that selected space:

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

- 3. We now need to create the method that will populate the `lstBooks` list view with the books contained in the selected storage space:

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

- 4. We also need to clear the selected book's details when the selected storage space is changed. I have created two group controls around the file and book details. This code just loops through all the child controls; if the child control is a textbox, it clears it:

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

- 5. The MenuStrip that was added to the form has a click event on the ImportEbooks menu item. It simply opens up the ImportBooks form:

```csharp
private async void mnuImportEbooks_Click(object sender, EventArgs e)
{
    ImportBooks import = new ImportBooks();
    import.ShowDialog();
    _spaces = await _spaces.ReadFromDataStore(_jsonPath);
    PopulateStorageSpaceList();
}
```

- 6. The following method wraps up the logic to select a specific eBook and populate the file and eBook details on the eBookManager form:

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

- 7. Lastly, when the book selected is the one you wish to read, click on the Read eBook button to open the file location of the selected eBook:

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

This completes the code logic contained in the eBookManager application.

> You can further modify the code to open the required application for the selected eBook instead of just the file location. In other words, if you click on a PDF document, the application can launch a PDF reader with the document loaded. Lastly, note that classification has not been implemented in this version of the application.

It is time to fire up the application and test it out.

## Running the eBookManager application

To run the application, please perform the following steps:

- 1. When the application starts for the first time, there will be no virtual storage spaces available. To create one, we will need to import some books. Click on the Import eBooks menu item:

![](img/762492a8287f369cc1ac4621a212f193_56_0.png)
