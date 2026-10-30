The Button still needs to send in a Car object as the parameter for the CanExecute() and Execute() methods. This is assigned through the CommandParameter property. You set this to the SelectedItem of the cboCars ComboBox, as follows:

```
CommandParameter="{Binding ElementName=cboCars, Path=SelectedItem}"
```

The complete markup for the button is shown here:

```
<Button x:Name="btnChangeColor" Content="Change Color" Margin="5,0,5,0"
    Padding="4, 2" Command="{Binding Path=ChangeColorCmd,
        RelativeSource={RelativeSource Mode=FindAncestor, AncestorType={x:Type Window}}}"
    CommandParameter="{Binding ElementName=cboCars, Path=SelectedItem}"/>
```

# Testing the Application

Run the application. You will see that the Change Color command is not enabled, as shown in Figure 28-8, since there isn’t a vehicle selected.

![](bbox=[0.07, 0.38, 0.86, 0.595])

Figure 28-8. A window with nothing selected

Now, select a vehicle; the button will become enabled, and clicking it will change the color, as expected!

# Creating the CommandBase Class

If you continued with this pattern for AddCarCommand.cs, there would be code that would be repeated between the classes. This is a good sign that a base class can help. Create a new class in the Cmds folder named CommandBase.cs and add a using for the System.Windows.Input namespace. Set the class to public and implement the ICommand interface. Change the class and the Execute() and CanExecute() methods to abstract. Finally, add in the updated CanExecuteChanged event from the ChangeColorCommand class. The full implementation is listed here: