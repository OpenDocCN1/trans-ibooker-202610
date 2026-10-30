```
<GridSplitter Grid.Column ="0" Width ="5" Background ="Gray" />
<StackPanel Grid.Column="0" VerticalAlignment ="Stretch" >
    <Label Name="lblSpellingInstructions" FontSize="14" Margin="10,10,0,0">
        Spelling Hints
    </Label>

    <Expander Name="expanderSpelling" Header ="Try these!"
        Margin="10,10,10,10">
        <!-- This will be filled programmatically -->
        <Label Name ="lblSpellingHints" FontSize ="12"/>
    </Expander>
</StackPanel>

<!-- This will be the area to type within -->
<TextBox  Grid.Column ="1"
    SpellCheck.IsEnabled ="True"
    AcceptsReturn ="True"
    Name ="txtData" FontSize ="14"
    BorderBrush ="Blue"
    VerticalScrollBarVisibility="Auto"
    HorizontalScrollBarVisibility="Auto">
</TextBox>
</Grid>
```

# Implementing the MouseEnter/MouseLeave Event Handlers

At this point, the UI of your window is complete. The only remaining tasks are to provide an implementation for the remaining event handlers. Begin by updating your C# code file so that each of the MouseEnter, MouseLeave, and MouseExit handlers sets the text pane of the status bar with a fitting message to help the end user, like so:

```
public partial class MainWindow : System.Windows.Window
{
    ...
    protected void MouseEnterExitArea(object sender, RoutedEventArgs args)
    {
        statBarText.Text = "Exit the Application";
    }
    protected void MouseEnterToolsHintsArea(object sender, RoutedEventArgs args)
    {
        statBarText.Text = "Show Spelling Suggestions";
    }
    protected void MouseLeaveArea(object sender, RoutedEventArgs args)
    {
        statBarText.Text = "Ready";
    }
}
```

At this point, you can run your application. You should see your status bar change its text based on which menu item/toolbar button you hover your mouse over.