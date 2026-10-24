```
ListView.builder(
  itemCount: strings.length,
  itemBuilder: (BuildContext context, int i) =>
    Text(strings[i]),
)
```

There is also ListView.separated, which allows you to add a separator widget, in this case yet another Text for simplicity:

```
ListView.separated(
  itemCount: 500,
  itemBuilder: (context, i) =>
    Text("$i", style: Theme.of(context).textTheme.display1.),
  separatorBuilder: (context, i) =>
    i%3 == 0 ? Divider() : Padding(padding: EdgeInsets.all(0.5),),
)
```

This adds a divider widget[21] once every three numbers, starting from the space between the first and second widget, like in the following screenshot:

![](bbox=[0.15, 0.43, 0.29, 0.63])

This is especially useful when you want do display lists of items separated into categories.

Other uses for ListView in general are explained in the Row’s usage section in Where You’ll Use the Row Together with Other Widgets.

shrinkWrap, when set to false (default), will make the list take up as much space as is available on the main axis. If, instead, you set it to true the grid’s size will adapt to the size that is needed to display its children.

# Customizing the Divider

The Divider can be customized to fit your app’s overall look, feel and needs: