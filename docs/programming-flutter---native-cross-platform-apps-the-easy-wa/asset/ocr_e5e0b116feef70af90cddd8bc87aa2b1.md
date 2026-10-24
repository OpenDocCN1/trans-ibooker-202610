```
```dart
var list = numbers.map((el) => "#$el").toList();
```
```

# THE UNDERSCORE

You might have seen in many code examples that we have prefixed some variable or class names with an underscore (_). It is used to make them *library private*.

For example, when we write:

```
```dart
class MyWidget extends StatefulWidget {
  // (...)
}

class _MyWidgetState extends State<MyWidget> {
  // (...)
}
```
```

We are making the MyWidgetState class *library private*, meaning it is only accessible from within the current package: if you are working on a library and you want to make a class or top-level function inaccessible to users of that library, you us the underscore this way.

Using the underscore for class members is used to make it inaccessible from outside its class definition, like in the following example:

```
```dart
class CounterClass {
  int _counter = 0;

  void _setCounterTo(int n) {
    _counter = n;
  }

  int getCounter() => _counter;

  void increaseCounter() => _setCounterTo(_counter+1);
}
```
```

which is obviously a lot more complicated than it needs to be (_setCounterTo adds unnecessary complication) in order to show how different kinds of methods may be chosen to be set as private: if we are making a counter class, we want to be able to get the current value of the counter, but we don’t need to change it to any value other than it incremented by 1, so we only give access to a method that does exactly that and another that returns the value of the counter.