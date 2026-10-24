![](bbox=[0.148, 0.091, 0.325, 0.343])

# Adding a Link to the HomeScreen’s App Bar

Now we need the user to be able to reach this page from the rest of the app. We’ll make this possible by adding an AppBar action to the HomeScreen. This will be very similar to previous app bar actions we added earlier: when the user taps on the icon we show the user the StarredPage on top of the current view:

```
```dart
// networking/xkcd_app/lib/main.dart

IconButton(
  icon: Icon(Icons.star),
  tooltip: "Browse Starred Comics",
  onPressed: () {
    Navigator.push(
      context,
      MaterialPageRoute(
        builder: (BuildContext context) =>
          StarredPage()
      ),
    );
  },
),
```
```

This is what the HomeScreen’s app bar will look like:

![](bbox=[0.148, 0.746, 0.335, 0.769])