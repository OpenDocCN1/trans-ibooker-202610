The next step is to go to the eBookManager screen.

# The eBookManager screen

In the eBookManager.Designer.cs file, remove the imageList1 section:

```
this.lstStorageSpaces.TabIndex = 1;
this.lstStorageSpaces.UseCompatibleStateImageBehavior = false;
this.lstStorageSpaces.View = System.Windows.Forms.View.Tile;
this.lstStorageSpaces.MouseClick += new System.Windows.Forms.MouseEve
//
// imagelist1
//
this.imagelist1.ImageStream = ((System.Windows.Forms.ImageListStreame
this.imagelist1.TransparentColor = System.Drawing.Color.Transparent;
this.imagelist1.Images.SetKeyName(0, "storage_space_cloud.png");
this.imagelist1.Images.SetKeyName(1, "eBook.png");
this.imagelist1.Images.SetKeyName(2, "no_book.png");
//
// groupBox1
//
this.groupBox1.Anchor = ((System.Windows.Forms.AnchorStyles)((System
| System.Windows.Forms.AnchorStyles.Left)));
```

Also remove the btnReadEbook section:

```
this.txtStorageSpaceDescription.TabIndex = 6;
//
// btnReadEbook
//
this.btnReadEbook.Image = global::eBookManager.Properties.Resources.R
this.btnReadEbook.Location = new System.Drawing.Point(103, 227);
this.btnReadEbook.Name = "btnReadEbook";
this.btnReadEbook.Size = new System.Drawing.Size(36, 40);
this.btnReadEbook.TabIndex = 32;
this.toolTip1.SetToolTip(this.btnReadEbook, "Click here to open the e
this.btnReadEbook.UseVisualStyleBackColor = true;
this.btnReadEbook.Click += new System.EventHandler(this.btnReadEbook_
//
// toolTip1
//
this.toolTip1.ToolTipIcon = System.Windows.Forms.ToolTipIcon.Info;
```