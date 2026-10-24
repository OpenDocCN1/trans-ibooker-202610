# The Key

The Key is used to uniquely identify a widgeet and it is used by the framework to update the elements that actually need to be updated.

You usually won't be using keys, since the framework will take care of them by itself, but you can use them to have greater control of widget state and updates.

In addition to that, there are widgets that explicitly ask for a key, like the `Dismissibles` we encountered in *Dismissible ListTitles*.

# MANY KEYS FOR THE SAME KEYHOLE

There are many kinds of `Key`s, and some are actually more useful than the plain `Key` constructor, which takes as a named argument a `String` that must be unique within the widget's parent.

```
ExampleWidget(
  key: Key("example")
)
```

`GlobalKey`s also exist and, because of their global scope, they have to be absolutely unique.

`GlobalKey`s allow for widgets to be moved to be a child of a completely different parent while preserving its state.

There also are `LocalKey`s, which have the same scope as the normal `Key`s, but have a few different constructors:

+   - The `ValueKey`, which takes a value of any type, not just a `String`.
+   - The `UniqueKey`, which takes no value at all and is always unique (what we used in *Dismissible ListTitles*).

# USING KEYS

Most of the time, you don't need to use `Key`s; they only become necessary when widgets require you to set a `Key`, as happened when we talked about the `Dismissible` in *Dismissible ListTitles*.