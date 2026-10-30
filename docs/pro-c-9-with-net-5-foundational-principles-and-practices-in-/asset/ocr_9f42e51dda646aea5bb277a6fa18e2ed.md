> Note The Properties window provides a Search text area at the top. Type in the name of a property you would like to set to quickly find the item in question.

After you have finished tinkering with the Brushes editor, check out the generated markup. It might look something like this:

```
<Button Content="Button" HorizontalAlignment="Left" Margin="10,10,0,0" VerticalAlignment="Top" Width="75">
    <Button.Background>
        <LinearGradientBrush EndPoint="0.5,1" StartPoint="0.5,0">
            <GradientStop Color="Black" Offset="0"/>
            <GradientStop Color="#FFE90E0E" Offset="1"/>
            <GradientStop Color="#FF1F4CE3"/>
        </LinearGradientBrush>
    </Button.Background>
</Button>
```

# Handling Events Using the Properties Window

If you want to handle events for a given control, you can also make use of the Properties window, but this time you need to click the Events button at the upper right of the Properties window (look for the lightning bolt icon). Ensure that the button is selected on your designer and locate the Click event. Once you do, double-click directly on the Click event entry. This will cause Visual Studio to automatically build an event handler that takes the following general form:

*NameOfControl_NameOfEvent*

Since you did not rename your button, the Properties window shows that it generated an event handler named Button_Click (see Figure 24-7).