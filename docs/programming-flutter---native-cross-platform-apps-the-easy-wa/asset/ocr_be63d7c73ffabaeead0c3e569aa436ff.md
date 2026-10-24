# What You Need to Build the UI:
Navigation and the InheritedWidget

Before we actually build the UI for this app, we’re going to discuss two important things: how to navigate between multiple views in Flutter and how to use InheritedWidgetS. This will be a general introduction to the options Flutter offers for navigation; we’ll only use some of these in the app. If you’d prefer to jump ahead to more advanced navigation, take a look at Build the App’s Basic UI and then come back here when you want to learn about how to implement navigation in Flutter apps.

# NAVIGATION IN FLUTTER

Flutter offers multiple options for navigating between views:

+   - push/pop navigation, useful when there is a home page and many pages accessible from it, potentially using a Drawer menu.
+   - Page-by-page navigation using the PageView and PageController, useful when there are a limited number of pages that can be considered on the same level.

# PUSH/POP NAVIGATION USING THE NAVIGATOR

Making an app that takes advantage of the Navigator for push/pop navigation is really simple.

# Define the Pages

The first thing to do is to define the pages, which will be really simple:

```
class NewPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text("New Page")),
      body: Center(
        child: FlatButton(
          color: Colors.black12,
          onPressed: () {},
          child: Text("Go back to the home page"),
        )
      ),
    );
  }
}
```