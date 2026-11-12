The first place we need to call this method is obviously from within itself, as this is a recursive method. The second place we need to call it is from the btnSelectSourceFolder button click event (again, as before, select the click property and double-click):

```
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
```

This is all quite straightforward code. Select the folder to recurse and populate the TreeView control with all the files found that match the file extension contained in our AllowedExtensions property. We also need to look at the code when someone selects a book in the tvFoundBooks TreeView control. When a book is selected, we need to read the properties of the selected file and return those properties to the file details section:

```
```csharp
private void tvFoundBooks_AfterSelect(object sender, TreeViewEventArgs e)
{
    DocumentEngine engine = new DocumentEngine();
    string path = e.Node.Tag?.ToString() ?? "";
    if (File.Exists(path))
    {
        var (dateCreated, dateLastAccessed, fileName, fileExtension,
        fileLength, hasError) = engine.GetFileProperties(e.Node.Tag.ToString()));
}
```
```