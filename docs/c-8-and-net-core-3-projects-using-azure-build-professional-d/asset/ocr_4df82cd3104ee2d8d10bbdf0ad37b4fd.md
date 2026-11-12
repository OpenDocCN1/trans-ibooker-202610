And finally, remove the this.Icon assignment in the eBookManager section:

```
//
// eBookManager
//
this.AutoScaleDimensions = new System.Drawing.SizeF(6F, 13F);
this.AutoScaleMode = System.Windows.Forms.AutoScaleMode.Font;
this.ClientSize = new System.Drawing.Size(1113, 643);
this.Controls.Add(this.groupBox3);
this.Controls.Add(this.groupBox2);
this.Controls.Add(this.groupBox1);
this.Controls.Add(this.menuStrip1);
this.Icon = ((System.Drawing.Icon)(resources.GetObject("$this.Icon")))
this.MainMenuStrip = this.menuStrip1;
this.Name = "eBookManager";
this.Text = " eBook Manager";
this.Load += new System.EventHandler(this.Form1_Load);
```

We'll move the code that has been removed into the Form_Load event of eBookManager.cs:

```
private void Form1_Load(object sender, EventArgs e)
{
    System.ComponentModel.ComponentResourceManager resources = new
    System.ComponentModel.ComponentResourceManager(typeof(eBookManager));

    this.components = new System.ComponentModel.Container();

    // imageList1
    //this.imageList1.ImageStream =
    ((System.Windows.Forms.ImageListStreamer)(resources.GetObject("imageList1.ImageStream")));
    this.imageList1.Images.Add("storage_space_cloud.png",
    Image.FromFile("img/storage_space_cloud.png"));
    this.imageList1.Images.Add("eBook.png",
    Image.FromFile("img/eBook.png"));
    this.imageList1.Images.Add("no_eBook.png",
    Image.FromFile("img/no_eBook.png"));
    this.imageList1.TransparentColor = System.Drawing.Color.Transparent;

    // btnReadEbook
    this.btnReadEbook.Image = Image.FromFile("img/ReadEbook.png");
    this.btnReadEbook.Location = new System.Drawing.Point(103, 227);
    this.btnReadEbook.Name = "btnReadEbook";
    this.btnReadEbook.Size = new System.Drawing.Size(36, 40);
    this.btnReadEbook.TabIndex = 32;
    this.toolTip1.SetToolTip(this.btnReadEbook, "Click here to open the
```