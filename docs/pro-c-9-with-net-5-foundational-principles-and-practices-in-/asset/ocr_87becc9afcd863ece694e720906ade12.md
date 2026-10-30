new WPF application project named WpfRoutedEvents. Now, update the XAML description of the initial window by adding the following Button control, which defines some complex content:

```
<Window ...
    <Grid>
        <Button Name="btnClickMe" Height="75" Width = "250"
            Click ="btnClickMe_Clicked">
            <StackPanel Orientation ="Horizontal">
                <Label Height="50" FontSize ="20">
                    Fancy Button!</Label>
                <Canvas Height ="50" Width ="100" >
                    <Ellipse Name = "outerEllipse" Fill ="Green"
                        Height ="25" Width ="50" Cursor="Hand"
                        Canvas.Left="25" Canvas.Top="12"/>
                    <Ellipse Name = "innerEllipse" Fill ="Yellow"
                        Height = "15" Width ="36"
                        Canvas.Top="17" Canvas.Left="32"/>
                </Canvas>
            </StackPanel>
        </Button>
    </Grid>
</Window>
```

Notice in the Button's opening definition that you have handled the Click event by specifying the name of a method to be called when the event is raised. The Click event works with the RoutedEventHandler delegate, which expects an event handler that takes an object as the first parameter and a System.Windows. RoutedEventArgs as the second. Implement this handler as so:

```
public void btnClickMe_Clicked(object sender, RoutedEventArgs e)
{
    // Do something when button is clicked.
    MessageBox.Show("Clicked the button");
}
```

If you run your application, you will see this message box display, regardless of which part of the button's content you click (the green Ellipse, the yellow Ellipse, the Label, or the Button's surface). This is a good thing. Imagine how tedious WPF event handling would be if you were forced to handle a Click event for every one of these subelements. Not only would the creation of separate event handlers for each aspect of the Button be labor intensive, you would end up with some mighty nasty code to maintain down the road.

Thankfully, WPF routed events take care of ensuring that your single Click event handler will be called regardless of which part of the button is clicked automatically. Simply put, the routed events model automatically propagates an event up (or down) a tree of objects, looking for an appropriate handler.

Specifically speaking, a routed event can make use of three routing strategies. If an event is moving from the point of origin up to other defining scopes within the object tree, the event is said to be a bubbling event. Conversely, if an event is moving from the outermost element (e.g., a Window) down to the point of origin, the event is said to be a tunneling event. Finally, if an event is raised and handled only by the originating element (which is what could be described as a normal CLR event), it is said to be a direct event.