```
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
```

All that remains now is for us to complete the code in the Form1.cs form, which is the startup form.

# Creating the main eBookManager form

Start off by renaming Form1.cs to eBookManager.cs. This is the startup form for the application, and it will list all existing storage spaces that were previously saved:

![](bbox=[0.262, 0.515, 0.735, 0.8])