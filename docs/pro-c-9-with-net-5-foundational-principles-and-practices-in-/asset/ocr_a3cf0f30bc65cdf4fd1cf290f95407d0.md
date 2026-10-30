```
{
    _execute = execute
    ?? throw new ArgumentNullException(nameof(execute));
    _canExecute = canExecute;
}
```

Finally, implement the CanExecute() and Execute() overrides. CanExecute() returns true if the Func is null; or, if it is not null, it executes and returns true. Execute() executes the Action parameter.

```
public override bool CanExecute(object parameter)
    => _canExecute == null || _canExecute((T)parameter);
public override void Execute(object parameter)
{ _execute((T)parameter); }
```

# Updating MainWindow.xaml.cs

When you use RelayCommands, all of the methods for the delegates need to be specified when a new command is constructed. This doesn’t mean that the code needs to live in the code-behind (as is shown here); it just has to be accessible from the code-behind. It could live in another class (or even another assembly), providing the code encapsulation benefits of creating a custom command class.

Add a new private variable of type RelayCommand<Car> and a public property named DeleteCarCmd, as shown here:

```
private RelayCommand<Car> _deleteCarCommand = null;
public RelayCommand<Car> DeleteCarCmd
    => _deleteCarCommand ??=
        new RelayCommand<Car>(DeleteCar,CanDeleteCar);
```

The DeleteCar() and CanDeleteCar() methods must be created as well, as follows:

```
private bool CanDeleteCar(Car car) => car != null;
private void DeleteCar(Car car)
{
    _cars.Remove(car);
}
```

Notice the strong typing in the methods—this is one of the benefits of using RelayCommand<T>.

# Adding and Implementing the Delete Car Button

The final step to tie it all together is to add the button and assign the Command and CommandParameter bindings. Add the following markup:

```
<Button x:Name="btnDeleteCar" Content="Delete Car" Margin="5,0,5,0" Padding="4, 2"
    Command="{Binding Path=DeleteCarCmd,
        RelativeSource={RelativeSource Mode=FindAncestor, AncestorType={x:Type Window}}}"
    CommandParameter="{Binding ElementName=cboCars, Path=SelectedItem}"/>
```

Now when you run the application, you can test that the Delete Car button is enabled only if a car is selected in the drop-down and that clicking the button does indeed delete the car from the Car list.