```
private void btnShowVisualTree_Click(
    object sender, RoutedEventArgs e)
{ 
}
```

If you run your application and click this first button, you will see a tree print in the text area, which is just about an exact replica of the original XAML (see Figure 27-10).

![](bbox=[0.1, 0.24, 0.73, 0.52])

Figure 27-10. Viewing a logical tree at runtime

# Programmatically Inspecting a Visual Tree

A Window's visual tree can also be inspected at runtime using the VisualTreeHelper class of System.Windows.Media. Here is a Click implementation of the second Button control (btnShowVisualTree), which performs similar recursive logic to build a textual representation of the visual tree:

```
using System.Windows.Media;

private void btnShowVisualTree_Click(object sender, RoutedEventArgs e)
{
    _dataToShow="";
    BuildVisualTree(0, this);
    txtDisplayArea.Text=_dataToShow;
}
void BuildVisualTree(int depth, DependencyObject obj)
{
    // Add the type name to the dataToShow member variable.
    _dataToShow +=new string(' ', depth) + obj.GetType().Name + "\n";
    // Make a recursive call for each visual child.
    for (int i=0; i < VisualTreeHelper.GetChildrenCount(obj); i++)
```