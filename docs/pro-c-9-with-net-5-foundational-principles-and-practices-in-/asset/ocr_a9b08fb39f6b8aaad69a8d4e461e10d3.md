# Adding Bindings and Data

The next step is to add the binding statements for the controls. Remember that data-binding statements revolve around a data context, and this can be set on the control itself or on a parent control. Here, you are going to set the context on the DetailsGrid, so each control contained will inherit that data context. Set the DataContext to the SelectedItem property of the ComboBox. Update the Grid that holds the detail controls to the following:

```
<Grid Grid.Row="1" Name="DetailsGrid"
    DataContext="{Binding ElementName=cboCars, Path=SelectedItem}">
```

The text boxes in the DetailsGrid will show the individual properties of the select car. Add the appropriate text attributes and related bindings to the TextBox controls, like so:

```
<TextBox Grid.Column="1" Grid.Row="0" Text="{Binding Path=Id}" />
<TextBox Grid.Column="1" Grid.Row="1" Text="{Binding Path=Make}" />
<TextBox Grid.Column="1" Grid.Row="2" Text="{Binding Path=Color}" />
<TextBox Grid.Column="1" Grid.Row="3" Text="{Binding Path=PetName}" />
```

Finally, add data to the ComboBox. In MainWindow.xaml.cs, create a new list of Car records, and set the ItemsSource for the ComboBox to the list. Also, add the using statement for the Notifications.Models namespace.

```
using WpfNotifications.Models;
//omitted for brevity
public partial class MainWindow : Window
{
    readonly IList<Car> _cars = new List<Car>();
    public MainWindow()
    {
        InitializeComponent();
        _cars.Add(new Car {Id = 1, Color = "Blue", Make = "Chevy", PetName = "Kit"});
        _cars.Add(new Car {Id = 2, Color = "Red", Make = "Ford", PetName = "Red Rider"});
        cboCars.ItemsSource = _cars;
    }
}
```

Run the app. You'll see that the vehicle selector has two cars to choose from. Choose one of them, and the text boxes will be automatically populated with the vehicle detail. Change the color of one of the vehicles, select the other vehicle, and then go back to the vehicle you edited. You will see the new color is indeed still attached to the vehicle. This isn't anything remarkable; you've seen the power of XAML data binding in previous examples.

# Programmatically Changing the Vehicle Data

While the previous example works as expected, if the data is changed programmatically, the UI will not reflect the changes unless you program the app to refresh the data. To demonstrate this, add an event handler for the btnChangeColor Button, like so:

```
<Button x:Name="btnChangeColor" Content="Change Color" Margin="5,0,5,0" Padding="4, 2" Click="BtnChangeColor_OnClick"/>
```