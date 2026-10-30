In the BtnChangeColor_Click() event handler, use the SelectedItem property of the ComboBox to locate the selected record from the cars list, and change the color to Pink. The code is listed here:

```
private void BtnChangeColor_OnClick(object sender, RoutedEventArgs e)
{
    _cars.First(x => x.Id == ((Car)cboCars.SelectedItem)?.Id).Color = "Pink";
}
```

Run the app, select a vehicle, and click the Change Color button. Nothing changes visibly. Select the other vehicle and go back to your originally selected vehicle. Now you will see the updated value. This is not a good experience for the user!

Now add an event handler to the btnAddCar button, like this:

```
<Button x:Name="btnAddCar" Content="Add Car" Margin="5,0,5,0" Padding="4, 2" Click="BtnAddCar_OnClick" />
```

In the BtnAddCar_Click event handler, add a new record to the Car list.

```
private void BtnAddCar_Click(object sender, RoutedEventArgs e)
{
    var maxCount = _cars?.Max(x => x.Id) ?? 0;
    _cars?.Add(new Car { Id=++maxCount,Color="Yellow",Make="VW",PetName="Birdie"});
}
```

Run the app, click the Add Car button, and examine the contents of the ComboBox. Even though you know there are three cars in the list, only two are displayed! To correct both of these problems, you will convert the Car class to an observable model and use an observable collection to hold all of the Car instances.

# Observable Models

The problem of data changing on a property of your model and not being displayed in the UI is resolved by implementing the INotifyPropertyChanged interface on your Car model class. The INotifyPropertyChanged interface contains a single event: PropertyChangedEvent. The XAML binding engine listens for this event for each bound property on classes that implement the INotifyPropertyChanged interface. The interface is shown here:

```
public interface INotifyPropertyChanged
{
    event PropertyChangedEventHandler PropertyChanged;
}
```

Add the following using statements to the Car.cs class:

```
using System.ComponentModel;
using System.Runtime.CompilerServices;
```