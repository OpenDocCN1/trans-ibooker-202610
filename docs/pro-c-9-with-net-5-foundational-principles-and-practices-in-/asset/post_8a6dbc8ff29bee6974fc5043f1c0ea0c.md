
this.MyInkCanvas.EditingMode = (sender as RadioButton)?.Content.ToString() switch
{
    // These strings must be the same as the Content values for each
    // RadioButton.
    "Ink Mode!" => InkCanvasEditingMode.Ink,
    "Erase Mode!" => InkCanvasEditingMode.EraseByStroke,
    "Select Mode!" => InkCanvasEditingMode.Select,
    _ => this.MyInkCanvas.EditingMode
};
}

Also, set the mode to Ink by default in the window's constructor. And while you are at it, set a default selection for the ComboBox (more details on this control in the next section), as follows:

```
public MainWindow()
{
    this.InitializeComponent();

    // Be in Ink mode by default.
    this.MyInkCanvas.EditingMode = InkCanvasEditingMode.Ink;
    this.inkRadio.IsChecked = true;
    this.comboColors.SelectedIndex = 0;
}
```

Now run your program again by pressing F5. Enter Ink mode and draw some data. Next, enter Erase mode and remove the previous mouse stroke you entered (you'll notice the mouse icon automatically looks like an eraser). Finally, enter Select mode and select some strokes by using the mouse as a lasso.

After you circle the item, you can move it around the canvas and resize its dimensions. Figure 25-20 shows your edit modes at work.

![](img/587021f4d5ba039be70a11107130377b_1077_0.png)

Figure 25-20. The InkCanvas in action, with edit modes!

## The ComboBox Control

After you populate a ComboBox control (or a ListBox), you have three ways to determine the selected item. First, if you want to find the numerical index of the item selected, you can use the SelectedIndex property (which is zero based; a value of -1 represents no selection). Second, if you want to obtain the object within the list that has been selected, the SelectedItem property fits the bill. Third, the SelectedValue allows you to obtain the value of the selected object (typically obtained using a call to ToString()).

You need to add the last bit of code for this tab to change the color of the strokes entered on the InkCanvas. The DefaultDrawingAttributes property of InkCanvas returns a DrawingAttributes object that allows you to configure numerous aspects of the pen nib, including its size and color (among other settings). Update your C# code with this implementation of the ColorChanged() method:

```
private void ColorChanged(object sender, SelectionChangedEventArgs e)
{
    // Get the selected value in the combo box.
    string colorToUse =
        (this.comboColors.SelectedItem as ComboBoxItem)?.Content.ToString();

    // Change the color used to render the strokes.
    this.MyInkCanvas.DefaultDrawingAttributes.Color =
        (Color)ColorConverter.ConvertFromString(colorToUse);
}
```

Now recall that the ComboBox has a collection of ComboBoxItems. If you view the generated XAML, you’ll see the following definition:

```
<ComboBox x:Name="comboColors" Width="100" SelectionChanged="ColorChanged">
    <ComboBoxItem Content="Red"/>
    <ComboBoxItem Content="Green"/>
    <ComboBoxItem Content="Blue"/>
</ComboBox>
```

When you call SelectedItem, you grab the selected ComboBoxItem, which is stored as a general Object. After you cast the Object as a ComboBoxItem, you pluck out the value of the Content, which will be the string Red, Green, or Blue. This string is then converted to a Color object using the handy ColorConverter utility class. Now run your program again. You should be able to change between colors as you render your image.

Note that the ComboBox and ListBox controls can contain complex content as well, rather than a list of text data. You can get a sense of some of the things that are possible by opening the XAML editor for your window and changing the definition of your ComboBox so it contains a set of StackPanel elements, each of which contains an Ellipse and a Label (notice that the Width of the ComboBox is 175).

```
<ComboBox x:Name="comboColors" Width="175" Margin="10,0,0,0" SelectionChanged="ColorChanged">
    <StackPanel Orientation ="Horizontal" Tag="Red">
        <Ellipse Fill ="Red" Height ="50" Width ="50"/>
        <Label FontSize ="20" HorizontalAlignment="Center"
            VerticalAlignment="Center" Content="Red"/>
    </StackPanel>

<StackPanel Orientation ="Horizontal" Tag="Green">
    <Ellipse Fill ="Green" Height ="50" Width ="50"/>
    <Label FontSize ="20" HorizontalAlignment="Center"
        VerticalAlignment="Center" Content="Green"/>
</StackPanel>

<StackPanel Orientation ="Horizontal" Tag="Blue">
    <Ellipse Fill ="Blue" Height ="50" Width ="50"/>
    <Label FontSize ="20" HorizontalAlignment="Center"
        VerticalAlignment="Center" Content="Blue"/>
</StackPanel>
</ComboBox>
```

Notice that each StackPanel assigns a value to its Tag property, which is a simple, fast, and convenient way to discover which stack of items has been selected by the user (there are better ways to do this, but this will do for now). With this adjustment, you need to change the implementation of your ColorChanged() method, like this:

```
private void ColorChanged(object sender, SelectionChangedEventArgs e)
{
    // Get the Tag of the selected StackPanel.
    string colorToUse = (this.comboColors.SelectedItem
        as StackPanel).Tag.ToString();
    ...
}
```

Now run your program again and take note of your unique ComboBox (see Figure 25-21).

![](img/587021f4d5ba039be70a11107130377b_1079_0.png)

Figure 25-21. A custom ComboBox, thanks to the WPF content model

## Saving, Loading, and Clearing InkCanvas Data

The last part of this tab will enable you to save and load your canvas data, as well as clear it of all content by adding event handlers for the buttons in the toolbar. Update the XAML for the buttons by adding markup for the click events, as follows:

```
<Button Grid.Column="0" x:Name="btnSave" Margin="10,10" Width="70" Content="Save Data" Click="SaveData"/>
<Button Grid.Column="1" x:Name="btnLoad" Margin="10,10" Width="70" Content="Load Data" Click="LoadData"/>
<Button Grid.Column="2" x:Name="btnClear" Margin="10,10" Width="70" Content="Clear" Click="Clear"/>
```

Next, import the System.IO and System.Windows.Ink namespaces to your code file. Implement the handlers, like this:

```
private void SaveData(object sender, RoutedEventArgs e)
{
    // Save all data on the InkCanvas to a local file.
    using (FileStream fs = new FileStream("StrokeData.bin", FileMode.Create))
    this.MyInkCanvas.Strokes.Save(fs);
    fs.Close();
    MessageBox.Show("Image Saved","Saved");
}

private void LoadData(object sender, RoutedEventArgs e)
{
    // Fill StrokeCollection from file.
    using(FileStream fs = new FileStream("StrokeData.bin", FileMode.Open, FileAccess.Read))
    StrokeCollection strokes = new StrokeCollection(fs);
    this.MyInkCanvas.Strokes = strokes;
}

private void Clear(object sender, RoutedEventArgs e)
{
    // Clear all strokes.
    this.MyInkCanvas.Strokes.Clear();
}
```

You should now be able to save your data to a file, load it from the file, and clear the InkCanvas of all data. That wraps up the first tab of the TabControl, as well as your examination of the WPF digital Ink API. To be sure, there is more to say about this technology; however, you should be in a good position to dig into the topic further if that interests you. Next, you will learn how to use WPF data binding.

## Introducing the WPF Data-Binding Model

Controls are often the target of various data-binding operations. Simply put, data binding is the act of connecting control properties to data values that might change over the course of your application’s lifetime. Doing so lets a user interface element display the state of a variable in your code. For example, you might use data binding to accomplish the following:

- Check a CheckBox control based on a Boolean property of a given object.
- Display data in DataGrid objects from a relational database table.
- Connect a Label to an integer that represents the number of files in a folder.

When you use the intrinsic WPF data-binding engine, you must be aware of the distinction between the source and the destination of the binding operation. As you might expect, the source of a data-binding operation is the data itself (e.g., a Boolean property or relational data), while the destination (target) is the UI control property that uses the data content (e.g., a property on a CheckBox or TextBox control).

In addition to binding to traditional data, WPF enables element binding, as stated in the previous examples. This means you can bind (for example) the visibility of a property based on the checked property of a check box. You could certainly do this in WinForms, but it had to be done through code. The WPF framework provides a rich data-binding ecosystem that can be almost entirely handled in markup. This also enables you to ensure that the source and destination stay in sync if either of their values changes.

## Building the Data Binding Tab

Using the Document Outline editor, change the Grid of your second tab to a StackPanel. Now, use the Toolbox and Properties editor of Visual Studio to build the following initial layout:

```
<TabItem x:Name="tabDataBinding" Header="Data Binding">
    <StackPanel Width="250">
        <Label Content="Move the scroll bar to see the current value"/>

        <!-- The scrollbar's value is the source of this data bind. -->
        <ScrollBar x:Name="mySB" Orientation="Horizontal" Height="30"
            Minimum = "1" Maximum = "100" LargeChange="1" SmallChange="1"/>

        <!-- The label's content will be bound to the scroll bar! -->
        <Label x:Name="labelSBThumb" Height="30" BorderBrush="Blue"
            BorderThickness="2" Content = "0"/>
    </StackPanel>
</TabItem>
```

Notice that the ScrollBar object (named mySB here) has been configured with a range between 1 and 100. The goal is to ensure that, as you reposition the thumb of the scrollbar (or click the left or right arrow), the Label will automatically update with the current value. Currently, the Content property of the Label control is set to the value "0"; however, you will change this via a data-binding operation.
