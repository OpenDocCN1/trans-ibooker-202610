```
],
),
),
```

```
floatingActionButton: FloatingActionButton(
  onPressed: _incrementCounter,
  tooltip: 'Increment',
  child: Icon(Icons.add),
);
}
```

# Use of a Widget to Provide the Basic App Structure: The Scaffold

This time the widget we return is a Scaffold, the basic Material Design visual layout structure; in other words, it is a generic container for all of our app content.

A Scaffold has an AppBar, which (using a Text widget) displays the title which was set earlier in MyHomePage’s constructor.

# Build the App’s Body

The body of our Scaffold is wrapped inside a Center widget, which centers our content in the middle of the page.

The Center’s child (the content which it centers) is a Column, which displays its children in a vertical stack.

The first Text widget simply displays a String, whereas the second displays the counter with a built-in theme called display1.

# Add the Floating Action Button

Not centered (but inside the Scaffold, outside the body, in its own floatingActionButton attribute) is the FloatingActionButton which, when pressed, calls the _incrementCounter function we saw previously.

Quite easy to understand are the tooltip code and the Icon widget (again, everything is a widget here) inside of it (its “child”) which is a simple plus (or “add”, as it is called) sign.