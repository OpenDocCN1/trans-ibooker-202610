![](bbox=[0.148, 0.091, 0.325, 0.229])

Obviously you can change the radius to your liking by using the `radius` argument.

# ## Use a CircleAvatar in a ListTile

Integrating it into a `ListTile` is as simple as using the `CircleAvatar` as the `leading` widget, like in the following example:

```
```dart
ListTile(
  leading: CircleAvatar(
    backgroundImage: NetworkImage("https://pragprog.com/favicon.png"),
  ),
  title: Text("Example"),
),
```
```

which produces:

![](bbox=[0.148, 0.535, 0.312, 0.58])

# ## Use the Expandable ListTile: The ExpansionTile

The `ExpansionTile` is an expandable ListTile: it is just like a `ListTile` when it first gets rendered but you can, without leaving the current view, make an `ExpansionTile` taller and show more widgets, like more text or additional buttons and features.

You use the `ExpansionTile` like this:

```
```dart
ExpansionTile(
  leading: Image(leftImage), // OPTIONAL
  title: Text(title),

  // ExpansionTile-specific arguments
  initiallyExpanded: true, // OPTIONAL default: false
  children: <Widget>[
    Row(
      children: [
        Text("Left on first row"),
        Text("Right on first row"),
```
```