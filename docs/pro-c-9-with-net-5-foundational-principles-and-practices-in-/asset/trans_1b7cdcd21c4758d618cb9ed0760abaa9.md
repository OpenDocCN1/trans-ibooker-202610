

## Summary

由于 Windows Presentation Foundation 是一个图形密集型的 GUI API，因此我们拥有多种渲染图形输出的方式也就不足为奇了。本章首先[content]

## Summary

由于 Windows Presentation Foundation 是一个图形密集型的 GUI API，因此我们拥有多种渲染图形输出的方式也就不足为奇了。本章首先考察了 WPF 应用程序可以采用的三种方式（形状、绘图和视觉对象）来实现此目的，并讨论了诸如画刷、画笔和变换等各种渲染原语。

请记住，当你需要构建交互式 2D 渲染时，形状会使过程变得非常简单。然而，静态的非交互式渲染可以通过使用绘图和几何图形以更优化的方式进行渲染，而视觉层（仅在代码中可访问）则为你提供最大程度的控制和性能。

## WPF Resources, Animations, Styles, and Templates

本章将介绍三个重要且相互关联的话题，从而加深你对 Windows Presentation Foundation（WPF）API 的理解。首先要学习的是逻辑资源的作用。正如你将看到的，逻辑资源（也称为对象资源）系统是一种在 WPF 应用程序中命名和引用常用对象的方式。虽然逻辑资源通常在 XAML 中编写，但也可以在过程式代码中定义。

接下来，你将学习如何定义、执行和控制动画序列。尽管你可能认为如此，WPF 动画并不局限于视频游戏或多媒体应用程序。在 WPF API 下，动画可以像让按钮在获得焦点时看起来发光，或扩展 DataGrid 中选中行的大小一样微妙。理解动画是构建自定义控件模板的关键方面（正如本章后面将看到的那样）。

你随后将探索 WPF 样式和模板的作用。 much like a web page that uses CSS or the ASP.NET theme engine, a WPF application can define a common look and feel for a set of controls. 你可以在标记中定义这些样式并将其存储为对象资源以供以后使用，也可以动态地在运行时应用它们。最后一个示例将教你如何构建自定义控件模板。

## Understanding the WPF Resource System

你的第一个任务是检查嵌入和访问应用程序资源的话题。WPF 支持两种类型的资源。第一种是二进制资源，此类别通常包括大多数程序员在传统意义上认为的资源（嵌入式图像文件或声音剪辑、应用程序使用的图标等）。

第二种类型称为对象资源或逻辑资源，它代表一个命名的 .NET 对象，可以在整个应用程序中打包和重用。虽然任何 .NET 对象都可以打包为对象资源，但逻辑资源在处理任何类型的图形数据时特别有帮助，因为你可以定义常用的图形原语（画刷、画笔、动画等）并在需要时引用它们。

## Working with Binary Resources

在进入对象资源的话题之前，让我们快速了解一下如何将诸如图标或图像文件（例如公司标志或动画图像）等二进制资源打包到你的应用程序中。如果你想跟随操作，创建一个名为 BinaryResourcesApp 的新 WPF 应用程序。更新初始窗口的标记以处理 Window Loaded 事件并使用 DockPanel 作为布局根，如下所示：

```
<Window x:Class="BinaryResourcesApp.MainWindow"
    <!-- Omitted for brevity -->
    Title="Fun with Binary Resources" Height="500" Width="649" Loaded="MainWindow_OnLoaded">
    <DockPanel LastChildFill="True">
    </DockPanel>
</Window>
```

现在，假设你的应用程序需要根据用户输入在窗口的一部分中显示三个图像文件之一。WPF Image 控件不仅可以显示典型的图像文件（*.bmp、*.gif、*.ico、*.jpg、*.png、*.wdp 或 *.tiff），还可以显示 DrawingImage 中的数据（如你在第 26 章中看到的那样）。你可能会为窗口构建一个 UI，其中包含一个 DockPanel，其中包含一个简单的工具栏，带有 Next 和 Previous 按钮。在此工具栏下方，你可以放置一个 Image 控件，其 Source 属性目前没有设置值，如下所示：

```
<DockPanel LastChildFill="True">
    <ToolBar Height="60" Name="picturePickerToolbar" DockPanel.Dock="Top">
        <Button x:Name="btnPreviousImage" Height="40" Width="100" BorderBrush="Black" Margin="5" Content="Previous" Click="btnPreviousImage_Click"/>
        <Button x:Name="btnNextImage" Height="40" Width="100" BorderBrush="Black" Margin="5" Content="Next" Click="btnNextImage_Click"/>
    </ToolBar>
    <!-- We will fill this Image in code. -->
    <Border BorderThickness="2" BorderBrush="Green">
        <Image x:Name="imageHolder" Stretch="Fill" />
    </Border>
</DockPanel>
```

接下来，添加以下空事件处理程序：

```
private void MainWindow_OnLoaded(
    object sender, RoutedEventArgs e)
{
}
private void btnPreviousImage_Click(
    object sender, RoutedEventArgs e)
{
}
private void btnNextImage_Click(
    object sender, RoutedEventArgs e)
{
}
```

窗口加载时，图像将被添加到集合中，Next 和 Previous 按钮将循环浏览这些图像。现在应用程序框架已经就位，让我们来检查实现此功能的各种选项。

## Including Loose Resource Files in a Project

一个选项是以应用程序安装路径子目录中的一组松散文件形式分发你的图像文件。首先在你的项目中添加一个新文件夹（命名为 Images）。通过右键单击并选择 Add ► Existing Item 将一些图像添加到此文件夹中。确保在 Add Existing Item 对话框中更改文件筛选器为 *. *，以便显示图像文件。你可以添加自己的图像文件或使用可下载代码中的三个名为 Deer.jpg、Dogs.jpg 和 Welcome.jpg 的图像文件。

## Configuring the Loose Resources

为了在项目构建时将 \Images 文件夹的内容复制到 \bin\Debug 文件夹，首先在 Solution Explorer 中选择所有图像。现在，在仍选择这些图像的情况下，右键单击并选择 Properties 以打开 Properties 窗口。将 Build Action 属性设置为 Content，并将 Copy to Output Directory 属性设置为 Copy always（参见图 27-1）。

![](img/587021f4d5ba039be70a11107130377b_1137_0.png)

图 27-1. 配置图像数据以复制到你的输出目录

> Note 你也可以选择 Copy if Newer，如果你在构建包含大量内容的较大项目，这将为你节省时间。对于本示例，Copy always 即可。

如果构建你的项目，你现在可以单击 Solution Explorer 的 Show All Files 按钮，并在 \bin\Debug 目录下查看复制的 Image 文件夹（你可能需要单击 Refresh 按钮）。

## Programmatically Loading an Image

WPF 提供了一个名为 BitmapImage 的类，它是 System.Windows.Media.Imaging 命名空间的一部分。此类允许你从由 System.Uri 对象表示位置的图像文件加载数据。添加一个 List<BitmapImage> 来保存图像，以及一个 int 来存储当前显示图像的索引。

```
// A List of BitmapImage files.
List<BitmapImage> _images=new List<BitmapImage>();
// Current position in the list.
private int _currImage=0;
```

在窗口的 Loaded 事件中，填充图像列表，然后将 Image 控件源设置为列表中的第一张图像。

```
private void MainWindow_OnLoaded(
    object sender, RoutedEventArgs e)
{
    try
    {
        string path=Environment.CurrentDirectory;
        // Load these images from disk when the window loads.
        _images.Add(new BitmapImage(new Uri($@"{path}\Images\Deer.jpg")));
        _images.Add(new BitmapImage(new Uri($@"{path}\Images\Dogs.jpg")));
        _images.Add(new BitmapImage(new Uri($@"{path}\Images\Welcome.jpg")));
        // Show first image in the list.
        imageHolder.Source=_images[_currImage];
    }
    catch (Exception ex)
    {
        MessageBox.Show(ex.Message);
    }
}
```

接下来，实现上一个和下一个处理程序以循环浏览图像。如果用户到达列表末尾，则让他们回到开头，反之亦然。

```
private void btnPreviousImage_Click(
    object sender, RoutedEventArgs e)
{
    if (--_currImage < 0)
    {
        _currImage=_images.Count - 1;
    }
    imageHolder.Source=_images[_currImage];
}
private void btnNextImage_Click(
    object sender, RoutedEventArgs e)
{
    if (++_currImage >= _currImage >= _images.Count)
```

