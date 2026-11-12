```
```csharp
engine.GetFileProperties(selectedItem.FullName.ToString());

if (!hasError)
{
    txtFileName.Text = fileName;
    txtExtension.Text = fileExtention;
    dtCreated.Value = dateCreated;
    dtLastAccessed.Value = dateLastAccessed;
    txtFilePath.Text = selectedItem.FullName.ToString();
    txtFileSize.Text = $"{Round(fileLength.ToMegabytes(),
    2).ToString()} MB";
}
}
```
```

Again, this is very marginally changed; instead of storing the full filename (with the path) in the node `tag` property, we're now just referencing the underlying model, so it's much clearer. Our next step is to remove the existing WinForms TreeView control.

# # Removing the existing TreeView

The following code should be removed from ImportBooks.Designer.cs:

```
```csharp
//
// tvFoundBooks
//
this.tvFoundBooks.Location = new System.Drawing.Point(12, 41);
this.tvFoundBooks.Name = "tvFoundBooks";
this.tvFoundBooks.Size = new System.Drawing.Size(513, 246);
this.tvFoundBooks.TabIndex = 8;
this.tvFoundBooks.AfterSelect += new
System.Windows.Forms.TreeViewEventHandler(this.tvFoundBooks_AfterSelect);
```
```

This will remove the control itself. Later, we'll need to remove the following code that adds the TreeView to the controls collection:

```
```csharp
this.Controls.Add(this.tvFoundBooks);
```
```

That's it. If you now run the project, you'll see a UWP TreeView control right in the middle of a WinForms application.