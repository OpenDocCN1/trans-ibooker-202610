```
home: MyHomePage(title: 'An app that can count to', );
```

Now we need to edit _MyHomePageState’s build method again, since that’s what gets called every time the app’s state changes, and that’s where the variable part of the string will have to be generated.

To do what I just described, locate the following lines:

firstapp_starting/lib/main.dart

```
appBar: AppBar(
  title: Text(widget.title),
),
```

and change:

```
widget.title
```

to:

```
'${widget.title} $_counter'
```

so that those lines now instead read as follows:

firstapp/lib/main.dart

```
appBar: AppBar(
  title: Text('${widget.title} $_counter'),
),
```

As you can see, we’ve done the opposite of what we did to the counter, and we had to use braces for widget.title because dot notation was needed to access a member variable.

# ADD A RESET BUTTON

The last thing we need to do to get to the app in the screenshots shown earlier is to add the reset button.

Before we add the button itself, let’s define a simple method that changes the state and resets the counter to 0.

# Use setState() to Reset the Counter