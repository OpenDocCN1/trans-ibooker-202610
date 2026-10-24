
- The `currentIndex`, the value to which you assign will have to be changed manually when the user taps on a navigation item.
- The `onTap` callback, which takes an integer indicating which option has been tapped by the user.

A practical example of a full app home page class could be the following:

```dart
String _str;
int index = 0;

@override
Widget build(BuildContext context) {
  switch(index) {
    case 0:
      _str = "Home";
      break;
    case 1:
      _str = "Page 2";
      break;
    case 2:
      _str = "Page 3";
  }
  return Scaffold(
    appBar: AppBar(
      title: Text("Bottom Navigation Bar Example"),
      backgroundColor: Colors.red,
    ),
    body: Center(
      child: Text(
        "$_str",
        style: Theme.of(context).textTheme.display1
      ),
    ),
    bottomNavigationBar: BottomNavigationBar(
      currentIndex: index,
      items: [
        BottomNavigationBarItem(
          icon: Icon(Icons.home),
          title: Text("Home"),
        ),
        BottomNavigationBarItem(
          icon: Icon(Icons.looks_two),
          title: Text("Page 2"),
        ),
        BottomNavigationBarItem(
          icon: Icon(Icons.format_list_numbered_rtl),
          title: Text("Page 3"),
        ),
      ],
      onTap: (page) {
        setState() {
          switch(page) {
            case 0:
              index = 0;
              break;
            case 1:
              index = 1;
              break;
            case 2:
              index = 2;
          }
        }
      },
    ),
  );
}
```

which looks like the **screenshot** and produces the desired behavior when the user taps on a navigation option.

![](img/eb93f09b5f47b8f6c9bead1b9047c506_215_0.png)

# PageView

The PageView is comparable to a horizontal ListView, but that only scrolls page by page.

You can use it as a part of the overall layout, but also as the entire body of the app’s Scaffold, like in the following example:

```dart
Scaffold(
  appBar: AppBar(
    title: Text("PageView Example"),
    backgroundColor: Colors.teal,
  ),
  body: PageView(
    children: <Widget>[
      Center(
        child: Text(
          "Swipe left",
          style: Theme.of(context).textTheme.display1
        ),
      ),
      Center(
        child: Padding(
          padding: EdgeInsets.all(10.0),
          child: Text(
            "Swipe left again, or swipe right",
            style: Theme.of(context).textTheme.display1
          ),
        ),
      ),
      Center(
        child: Text(
          "Swipe right",
          style: Theme.of(context).textTheme.display1
        ),
      ),
    ],
  ),
)
```

which produces a simple view with an AppBar and, in the middle of the screen, a Text widget.

There is no other on-screen indication that the view has other pages, but swiping left moves the view to the next page and swiping right moves the view to the previous page.

In addition to the children, the PageView has a controller attribute, which can be set to a PageController.

The PageController has a useful option: the `initialPage`, which is an integer specifying the page (starting from 0) at which the view starts when the app loads.

# TOP NAVIGATION WITH A TABBAR AND TABBARVIEW

Significantly more useful and common is the TabBarView which, paired with a TabBar below the AppBar, provides the same swiping experience of a PageView combined with a nice menu at the top to select which page to switch to and to highlight the page the user is currently at.

The TabBarView and the TabBar are linked by a TabController.

An elegant way to create one is by wrapping the entire view in a DefaultTabController, and setting the entire Scaffold as its child, the number of tabs as its length, and (optionally) an initialIndex (starting from 0, which is the default).

The TabBar can be set as the AppBar's bottom option and it has a tab option where you'll add the Tab widgets, made up of a text and an optional icon, which will be the menu items which will make up the TabBar.

The TabBarView will instead be set as the Scaffold’s body and you will add the pages to be displayed to its children option.

For example, this:

```dart
DefaultTabController(
  length: 3,
  // OPTIONAL initialIndex: 0,
  child: Scaffold(
    appBar: AppBar(
      title: Text("TabBarView Example"),
      bottom: TabBar(
        tabs: <Widget>[
          Tab(
            icon: Icon(Icons.looks_one),
            text: "First Page",
          ),
          Tab(
            icon: Icon(Icons.looks_two),
            text: "Second Page",
          ),
          Tab(
            icon: Icon(Icons.looks_3),
            text: "Third Page",
          ),
        ],
      ),
    ),
    body: TabBarView(
      children: <Widget>[
        Center(
          child: Text("This is the first page")
        ),
        Center(
          child: Text("This is the second page")
        ),
        Center(
          child: Text("This is the third page")
        ),
      ],
    )
  )
)
```

which produces the result as shown in the screenshot.

![](img/eb93f09b5f47b8f6c9bead1b9047c506_218_0.png)

# InheritedWidgets

The DefaultTabController is a special kind of widget: an InheritedWidget, which is used to share state information between Flutter widgets, by storing them in a widget that sits at the top of the widget tree and allowing direct access to its fields and methods by its children, so that they don’t need to be passed down the tree using constructor arguments.

They are usually accessed using the Widget.of(context).fieldName syntax that we used previously to get theming data from the Theme and to show snackbars in the Scaffold. That is because both the Theme and the Scaffold are actually InheritedWidgets.

An InheritedWidget is defined in the following way:

```dart
class Example extends InheritedWidget {
  Example({child, this.field}) : super(child: child);
  final String field;

  @override
  bool updateShouldNotify(Example old) => true;
}
```

updateShouldNotify is used to determine whether the children widgets have to be rebuilt when the InheritedWidget gets rebuilt by the framework.

That will usually be true, but there you can also set it to a set of conditions to determine whether, for example, there are changes to the widget’s fields and only rebuild the children in that case.

In its current state, Example.of(context).fieldName from a child of Example wouldn’t work since there is actually no of method.

Even though it is very common, it is not actually built into the framework: by default you access parent InheritedWidget using context.inheritFromWidgetOfExactType(Example).

In fact, a very common static method (explained on Static Members) defined inside InheritedWidget is the of method which, in the case of an InheritedWidget called Example, can be defined in the following way:

```dart
static Example of(BuildContext context) =>
  context.inheritFromWidgetOfExactType(Example);
```

so that our entire Example class definition becomes:

```dart
class Example extends InheritedWidget {
  Example({child, this.field}) : super(child: child);
  final String field;

  @override
  bool updateShouldNotify(Example old) => old.field != field;

  static Example of(BuildContext context) =>
    context.inheritFromWidgetOfExactType(Example);
}
```

You can use it inside an app like this:

```dart
Example(
  field: "An example of a string",
  child: MyCustomExternalWidget(),
)
```

And, at any point inside the definition of MyCustomExternalWidget, which takes no arguments, you could access that field you set using:

```dart
Example.of(context).field
```

which returns the exact String that was set earlier.

# Build the App’s Basic UI

We’ll build an app that is going to display a ListView of ListTiles. There will be a tile for each comic and tapping on a tile should display the details of that comic.

The home is going to look like the screenshot.

![](img/eb93f09b5f47b8f6c9bead1b9047c506_221_0.png)

Let’s start by defining the classes we need for this app:

- A class for the home screen.
- A custom ComicTile class to make creating ListTiles for the comics easier.
- A class for the comic page.

For a change, we won’t be using an app class; the main method will pass the needed data directly to the HomeScreen class we’re going to create:

```dart
networking_starting/xkcd_app/lib/main.dart

void main() async =>
  runApp(
    new MaterialApp(
      home: HomeScreen(
        title: 'XKCD app',
        latestComic: await getLatestComicNumber(),
      )
    );
```
