Inside _MyHomePageState’s definition, below _incrementCounter()’s definition, add the following code:

```
firstapp/lib/main.dart

void _resetCounter() {
  setState() {
    _counter = 0;
  };
}
```

This is very similar to _incrementCounter() but, instead of incrementing the counter, we set it to 0.

# Make the Reset Button

Now that we’ve defined a method that resets the counter and triggers a re-render, we need to create a button to call that function. So before we do that, we need to know how we do that with Flutter.

# Buttons in Flutter: The FlatButton and the IconButton

If we want to quickly create a button in Flutter we can choose between a FlatButton or an IconButton.

The FlatButton is ideal when you want a button that displays text: it has a child attribute that is usually a Text widget, but you have the option to set it to anything.

The reason why the FlatButton is used mostly for text buttons is that using an IconButton exists specifically for the creation of icon-based buttons, by allowing the developer to specify an Icon object as its icon attribute, with the button only consisting of the icon and a small amount of padding.

One of the attributes that show the difference in focus is that the FlatButton’s color attribute controls the color of the button itself (the background to its content), whereas the IconButton’s color attribute control’s the icon’s color.

A RaisedButton also exists, but it is just a slightly different looking FlatButton.

# Implement the Reset Button