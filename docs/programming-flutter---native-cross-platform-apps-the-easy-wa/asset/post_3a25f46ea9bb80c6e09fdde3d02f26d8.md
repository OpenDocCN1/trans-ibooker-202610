
## Defining Widgets in Flutter

```
class MyApp extends StatelessWidget
```

In Flutter every UI element or collection of UI elements is a widget, which is an immutable object. This means we can’t change a widget’s member variables: if there are any, they have to be declared as final or constant. You can find more information on Dart variables in Variables and Conditions.

There are two kinds of widgets:

- Stateless widgets (subclasses of StatelessWidget), which get rendered when the app starts and when a parent widget gets re-rendered, like MyApp.
- Stateful widgets (subclasses of StatefulWidget), which you can re-render at any time by calling the setState() function, like this app’s MyHomePage, which is covered in The Home Page Widget.

The app widget is the collection of every other widget, so it is a widget. In this case it is a StatelessWidget.

## Working with Stateless Widgets

firstapp_starting/lib/main.dart

```
@override
Widget build(BuildContext context) {
  return MaterialApp(
    title: 'Flutter Demo',
    theme: ThemeData(
      primarySwatch: Colors.blue,
    ),
    home: MyHomePage(title: 'Flutter Demo Home Page'),
  );
}
```

This is the build method, which gets called when the widget needs to be rendered and returns a Widget object, which is every widget’s superclass.

In this case it gets called just once: when the app starts, given that MyApp doesn’t have a parent widget (it is every other widget’s parent widget).

In MyApp’s build method it is specified that the MyApp widget (the app itself) is a MaterialApp, which means that our app will use Material Design features that are part of the standard library.

The code sets, using the MaterialApp constructor, an app title, a theme with a primary swatch (a range of similar colors, as specified in Google’s Material Design guidelines), and a home page, which will be of class MyHomePage and have the title “Flutter Demo Home Page”.

## The Home Page Widget

This defines the aforementioned MyHomePage class, which is a StatefulWidget (a widget that has a state, unlike MyApp) with a title member and a state, which is _MyHomePageState:

firstapp_starting/lib/main.dart

```
class MyHomePage extends StatefulWidget {
  MyHomePage({Key key, this.title}) : super(key: key);
  final String title;

  @override
  _MyHomePageState createState() => _MyHomePageState();
}
```

In Dart, the underscore (_) in _MyHomePageState has an effect akin to the one private has in languages like Java and is explained in The underscore.

## The Advantage of Stateful Widgets

Stateful widgets have the ability to be re-rendered programmatically, allowing the developer to build an app that reacts to changes in the data it is showing or representing.

Because of this, they are more complex than stateless widgets: a stateful widget has a state, which is represented by a State object, which contains the widget’s logic and mutable data. As I mentioned in the previous section, widgets themselves are immutable, so they need a different object (the State) to hold the mutable data.

They get re-rendered when you call the setState() function, which notifies the framework that the state of the widget has changed and it needs to be re-rendered (by calling its build method again) to show the changes to the user.

## The MyHomePage Widget

The line:

```
MyHomePage({Key key, this.title}) : super(key: key);
```

defines MyHomePage’s arguments (as explained in Constructors). Since they are wrapped in braces, they are named arguments, which is why, when calling MyHomePage’s constructor, the syntax was:

```
MyHomePage(title: 'Flutter Demo Home Page')
```

The unused key parameter is used by the framework to identify which widgets to re-render. You can provide it and it can be useful, but it’s optional, and we won’t need it for this app.

We’ll talk about keys in the next chapter in The Key.

You can find more information on Dart class and constructor definitions in Classes.

Inside _MyHomePageState is where the app’s logic is defined:

firstapp_starting/lib/main.dart

```
class _MyHomePageState extends State<MyHomePage> {
  int _counter = 0;

  void _incrementCounter() {
    setState(() {
      _counter++;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text(widget.title),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            Text(
              'You have pushed the button this many times:',
            ),
          ],
        ),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: _incrementCounter,
        tooltip: 'Increment',
        child: Icon(Icons.add),
      ),
    );
  }
}
```

## Use of a Widget to Provide the Basic App Structure: The Scaffold

This time the widget we return is a Scaffold, the basic Material Design visual layout structure; in other words, it is a generic container for all of our app content.

A Scaffold has an AppBar, which (using a Text widget) displays the title which was set earlier in MyHomePage’s constructor.

## Build the App’s Body

The body of our Scaffold is wrapped inside a Center widget, which centers our content in the middle of the page.

The Center’s child (the content which it centers) is a Column, which displays its children in a vertical stack.

The first Text widget simply displays a String, whereas the second displays the counter with a built-in theme called display1.

## Add the Floating Action Button

Not centered (but inside the Scaffold, outside the body, in its own floatingActionButton attribute) is the FloatingActionButton which, when pressed, calls the _incrementCounter function we saw previously.

Quite easy to understand are the tooltip code and the Icon widget (again, everything is a widget here) inside of it (its “child”) which is a simple plus (or “add”, as it is called) sign.

Every time we click the FAB the counter is incremented and the state changes, the setState() call triggers the build method again and the UI gets reloaded with the updated counter.

## WHAT WE’VE SEEN AND WHERE WE’RE GOING

This is a very simple app: it’s just a MaterialApp that contains a Scaffold, with a simple FAB in the lower right and, in the middle, a Column with two Text widgets showing the number of times the FAB has been clicked.

This is very simple, but the only way to really understand how something is done is by doing it, so the next section is going to focus on adding new features to the existing code.

## Give the App Your Personal Touch

Now let’s modify what we’ve seen. We’ll change the app color to be green, add a red reset button to reset the counter to 0, make the app display the string “None” instead of the counter when the counter is at 0 and change the appBar’s title text. In doing this, we’ll get a feel for how to make simple changes, and also an introduction to more complex Flutter syntax.

The result will look like this:

![](img/eb93f09b5f47b8f6c9bead1b9047c506_45_0.png)

## CHANGE THE APP’S PRIMARY COLOR

We’ll start editing our app with the simplest change: changing the color to green. This is as simple as changing the following few lines in MyApp’s build method:

firstapp_starting/lib/main.dart

```
theme: ThemeData(
  primarySwatch: Colors.blue,
),
```

Go ahead and change Colors.blue to Colors.green so that those lines instead read as follows:

firstapp/lib/main.dart

```
theme: ThemeData(
  primarySwatch: Colors.green,
),
```

## MAKE THE COUNTER DISPLAY “NONE” INSTEAD OF 0

Now, let’s change the counter text in the middle so that it displays “None” instead of 0.

To do this, we’ll add a _displayedString variable to _MyHomePageState, below the counter’s declaration.

Locate the following line:

```
int _counter = 0;
```

and, below it, add:

```
String _displayedString;
```

After we’ve done that, we need to set this variable to “None” if the counter is 0 and to a string representation of the counter if that isn’t the case. We’re going to need a conditional.

## Use Conditionals in Flutter

To do this every time the counter is increased (which we do using a call to setState that triggers a re-render that calls the build method), this needs to be part of _MyHomePageState’s build method. So, just above:

```
return new Scaffold(
```

add:

firstapp/lib/main.dart

```
if (_counter == 0) {
  _displayedString = "None";
} else {
  _displayedString = _counter.toString();
}
```
