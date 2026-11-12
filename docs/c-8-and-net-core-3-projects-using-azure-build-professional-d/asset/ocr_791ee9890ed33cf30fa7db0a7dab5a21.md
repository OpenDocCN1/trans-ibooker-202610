The PopulateStorageSpacesList() method is using a local function, essentially allowing us to declare a piece of functionality that is accessible only from within its parent.

Let's add the call to this new method to the ImportBooks_Load method:

```
private async void ImportBooks_Load(object sender, EventArgs e)
{
    _spaces = await _spaces.ReadFromDataStore(_jsonPath);
    PopulateStorageSpacesList();
    if (dlVirtualStorageSpaces.Items.Count == 0)
    {
        dlVirtualStorageSpaces.Items.Add("<create new storage space > ");
    }
    lblEbookCount.Text = "";
}
```

We now need to add the logic for changing the selected storage space. The SelectedIndexChanged() event of the dlVirtualStorageSpaces control is modified as follows:

```
private void dlVirtualStorageSpaces_SelectedIndexChanged(object sender,
EventArgs e)
{
    int selectedValue =
    dlVirtualStorageSpaces.SelectedValue.ToString().ToInt();
    if ((selectedValue == (int)_storageSpaceSelection.New) // -9999
    {
        txtNewStorageSpaceName.Visible = true;
        lblStorageSpaceDescription.Visible = true;
        txtStorageSpaceDescription.ReadOnly = false;
        btnSaveNewStorageSpace.Visible = true;
        btnCancelNewStorageSpace.Visible = true;
        dlVirtualStorageSpaces.Enabled = false;
        btnAddNewStorageSpace.Enabled = false;
        lblEbookCount.Text = "";
    }
    else if (selectedValue != (int)_storageSpaceSelection.NoSelection)
    {
        // Find the contents of the selected storage space
        int contentCount = (from c in _spaces
        where c.ID == selectedValue
        select c).Count();
        if ((contentCount > 0)
        {
            StorageSpace selectedSpace = (from c in _spaces
```