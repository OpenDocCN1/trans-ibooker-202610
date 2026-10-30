
Double-click the MainWindow.xaml file in Solution Explorer, and replace the XAML with the following:

```
<Window x:Class="DataParallelismWithForEach.MainWindow"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"

    xmlns:local="clr-namespace:DataParallelismWithForEach"
    mc:Ignorable="d"
    Title="Fun with TPL" Height="400" Width="800">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
            <RowDefinition Height="Auto"/>
        </Grid.RowDefinitions>
        <Label Grid.Row="0" Grid.Column="0">
            Feel free to type here while the images are processed...
        </Label>
        <TextBox Grid.Row="1" Grid.Column="0" Margin="10,10,10,10"/>
        <Grid Grid.Row="2" Grid.Column="0">
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="Auto"/>
                <ColumnDefinition Width="*"/>
                <ColumnDefinition Width="Auto"/>
            </Grid.ColumnDefinitions>
            <Button Name="cmdCancel" Grid.Row="0" Grid.Column="0" Margin="10,10,0,10"
                Click="cmdCancel_Click">
                Cancel
            </Button>
            <Button Name="cmdProcess" Grid.Row="0" Grid.Column="2" Margin="0,10,10,10"
                Click="cmdProcess_Click">
                Click to Flip Your Images!
            </Button>
        </Grid>
    </Grid>
</Window>
```

Again, do not worry about what the markup means or how it works; you will spend plenty of time with WPF shortly. The GUI of the application consists of a multiline TextBox and a single Button (named cmdProcess). The purpose of the text area is to allow you to enter data while the work is being performed in the background, thus illustrating the nonblocking nature of the parallel task.

For this example, an additional NuGet package (System.Drawing.Common) is required. To add it to your project, enter the following line (all on one line) in the command line (in the same directory as your solution file) or Package Manager Console in Visual Studio:

```
dotnet add DataParallelismWithForEach package System.Drawing.Common
```

Open the MainWindow.xaml.cs file (double-click it in Visual Studio—you might have to expand the node by MainWindow.xaml), and add the following using statements to the top of the file:

```
// Be sure you have these namespaces! (System.Threading.Tasks should already be there from the default template)
using System;
using System.Drawing;
using System.Threading.Tasks;
using System.Threading;
using System.Windows;
using System.IO;
```

Note You should update the string passed into the following Directory.GetFiles() method call to point to a path on your computer that has some image files (such as a personal folder of family pictures). I have included some sample images (that ship with the Windows operating system) in the Solution directory for your convenience.

```
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
    }

    private void cmdCancel_Click(object sender, EventArgs e)
    {
        // This will be updated shortly
    }

    private void cmdProcess_Click(object sender, EventArgs e)
    {
        ProcessFiles();
        this.Title = "Processing Complete";
    }

    private void ProcessFiles()
    {
        // Load up all *.jpg files, and make a new folder for the
        // modified data.
        //Get the directory path where the file is executing
        //For VS 2019 debugging, the current directory will be <projectdirectory>\bin\debug\net5.0-windows
        //For VS Code or "dotnet run", the current directory will be <projectdirectory>
        var basePath = Directory.GetCurrentDirectory();
        var pictureDirectory =
            Path.Combine(basePath, "TestPictures");
        var outputDirectory =
            Path.Combine(basePath, "ModifiedPictures");
```

```
//Clear out any existing files
if (Directory.Exists(outputDirectory))
{
    Directory.Delete(outputDirectory, true);
}
Directory.CreateDirectory(outputDirectory);
string[] files = Directory.GetFiles(pictureDirectory,
    "*.jpg", SearchOption.AllDirectories);

// Process the image data in a blocking manner.
foreach (string currentFile in files)
{
    string filename =
        System.IO.Path.GetFileName(currentFile);
    // Print out the ID of the thread processing the current image.
    this.Title = $"Processing {filename} on thread {Thread.CurrentThread.ManagedThreadId}";
    using (Bitmap bitmap = new Bitmap(currentFile))
    {
        bitmap.RotateFlip(RotateFlipType.Rotate180FlipNone);
        bitmap.Save(System.IO.Path.Combine(
            outputDirectory, filename));
    }
}
```

> Note If you receive an error that Path is an ambiguous reference between System.IO.Path and System.Windows.Shapes.Path, either remove the using for System.Windows.Shapes or add System.IO to Path: System.IO.Path.Combine(...).

Notice that the ProcessFiles() method will rotate each *.jpg file under the specified directory. Currently, all the work is happening on the primary thread of the executable. Therefore, if the button is clicked, the program will appear to hang. Furthermore, the caption of the window will also report that the same primary thread is processing the file, as we have only a single thread of execution.

To process the files on as many CPUs as possible, you can rewrite the current foreach loop to use Parallel.ForEach(). Recall that this method has been overloaded numerous times; however, in the simplest form, you must specify the IEnumerable<T>-compatible object that contains the items to process (that would be the files string array) and an Action<T> delegate that points to the method that will perform the work.

Here is the relevant update, using the C# lambda operator in place of a literal Action<T> delegate object. Notice that you are currently commenting out the line of code that displayed the ID of the thread executing the current image file. See the next section to find out the reason why.

```
// Process the image data in a parallel manner!
Parallel.ForEach(files, currentFile =>
{
    string filename = Path.GetFileName(currentFile);
```

```
// This code statement is now a problem! See next section.
// this.Title = $"Processing {filename} on thread
//     {Thread.CurrentThread.ManagedThreadId}"
// Thread.CurrentThread.ManagedThreadId);

using (Bitmap bitmap = new Bitmap(currentFile))
{
    bitmap.RotateFlip(RotateFlipType.Rotate180FlipNone);
    bitmap.Save(Path.Combine(outputDirectory, filename));
}
);
```

## Accessing UI Elements on Secondary Threads

You will notice that I have commented out the previous line of code that updated the caption of the main window with the ID of the currently executing thread. As noted previously, GUI controls have “thread affinity” with the thread that created them. If secondary threads attempt to access a control they did not directly create, you are bound to run into runtime errors when debugging your software. On the flip side, if you were to run the application (via Ctrl+F5), you might not ever find any problems whatsoever with the original code.

Note Let me reiterate the previous point: when you debug a multithreaded application, you can sometimes catch errors that arise when a secondary thread is “touching” a control created on the primary thread. However, oftentimes when you run the application, the application could appear to run correctly (or it might error straightaway). Until you take precautions (examined next), your application has the potential of raising a runtime error under such circumstances.

One approach that you can use to allow these secondary threads to access the controls in a thread-safe manner is yet another delegate-centric technique, specifically an anonymous delegate. The Control parent class in WPF defines a Dispatcher object, which manages the work items for a thread. This object has a method named Invoke(), which takes a System.Delegate as input. You can call this method when you are in a coding context involving secondary threads to provide a thread-safe manner to update the UI of the given control. Now, while you could write all the required delegate code directly, most developers use expression syntax as a simple alternative. Here is the relevant update to the content with the previously commented-out code statement:

```
// Eek! This will not work anymore!
//this.Title = $"Processing {filename} on thread {Thread.CurrentThread.ManagedThreadId}";

// Invoke on the Form object, to allow secondary threads to access controls
// in a thread-safe manner.
Dispatcher?.Invoke(() =>
{
    this.Title = $"Processing {filename}";
});
using (Bitmap bitmap = new Bitmap(currentFile))
{
```
