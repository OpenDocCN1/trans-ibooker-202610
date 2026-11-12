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

# WindowsXamlHost

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
```