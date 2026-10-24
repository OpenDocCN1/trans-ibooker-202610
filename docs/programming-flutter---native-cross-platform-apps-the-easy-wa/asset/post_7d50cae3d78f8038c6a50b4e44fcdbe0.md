
```
```dart
);
}

class ExpandedRow extends StatelessWidget {

  ExpandedRow({this.children, this.crossAxisAlignment});

  final List<Widget> children;
  final CrossAxisAlignment crossAxisAlignment;

  @override
  Widget build(BuildContext context) =>
      Expanded(
        flex:1,
        child: Row(
          children: children,
          crossAxisAlignment: crossAxisAlignment,
        ),
      );
}
```
```

After defining them, simply replace all of the operator button's `Row` widgets with `ExpandedRow` widgets and all of the operator and number `FlatButton`s and the calculator's layout should be complete.

## The Finished Layout

If you have done everything correctly, you probably have ended up with the following code in main.dart:

```
layout/calculator_part2/lib/main.dart

import "package:flutter/material.dart";

class ExpandedButton extends StatelessWidget {

ExpandedButton({this.onPressed, this.child, this.color});

final Widget child;
final VoidCallback onPressed;
final Color color;

@override
Widget build(BuildContext context) =>
Expanded(
flex:1,
child: FlatButton(
onPressed: onPressed,
child: child,
color: color,
),
);
}

class ExpandedRow extends StatelessWidget {

ExpandedRow({this.children, this.crossAxisAlignment});

final List<Widget> children;
final CrossAxisAlignment crossAxisAlignment;

@override
Widget build(BuildContext context) =>
Expanded(
flex:1,
child: Row(
children: children,
crossAxisAlignment: crossAxisAlignment,
),
);
}

void main() => runApp(new MyApp());

class MyApp extends StatelessWidget {
@override
Widget build(BuildContext context) {
return MaterialApp(
title: "Flutter Calculator",
theme: ThemeData(
primarySwatch: Colors.blue,
backgroundColor: Colors.black26
),
```

```
home: CalculatorHomePage(title: "Flutter Calculator"),
);
}
}

class CalculatorHomePage extends StatefulWidget {
  CalculatorHomePage({Key key, this.title}) : super(key: key);
  final String title;

  @override
  _CalculatorHomePageState createState() => _CalculatorHomePageState();
}

class _CalculatorHomePageState extends State<CalculatorHomePage> {
  String _str = "0";

  void add(String a) {
  }

  void deleteAll() {
  }

  void deleteOne() {
  }

  void getResult() {
  }

  @override
  Widget build(BuildContext context) {
    return Center(child: Scaffold(
      appBar: AppBar(
        title: Text(widget.title),
      ),
      body: Column(
        crossAxisAlignment: CrossAxisAlignment.stretch,
        // mainAxisAlignment: MainAxisAlignment.spaceAround,
        // mainAxisSize: MainAxisSize.max,
        children: <Widget>[
          Expanded(
            flex: 2,
            child: Card(
              color: Colors.lightGreen[50],
              child: Padding(
                padding: EdgeInsets.all(15.0),
                child: Text(
                  _str,
                  textScaleFactor: 2.0,
                ),
              ),
            ),
          ),
          Expanded(
            flex: 1,
            child: Row(
              crossAxisAlignment: CrossAxisAlignment.stretch,
              children: <Widget>[
                Expanded(
                  flex: 3,
                  child: FlatButton(
                    child: Text(
                      'C',
                      style: TextStyle(color: Colors.white),
                    ),
```

```
onPressed: (){deleteAll();},
color: Colors.black54,
),
),
Expanded(
flex: 1,
child: FlatButton(
child: Text(
' <-',
style: TextStyle(color: Colors.white)
),
onPressed: (){deleteOne();},
color: Colors.black87,
),
),
],
),
Expanded(
flex: 4,
child: Row(
crossAxisAlignment: CrossAxisAlignment.stretch,
children: <Widget>[
Expanded(
flex: 3,
child: Column(
crossAxisAlignment: CrossAxisAlignment.stretch,
children: <Widget>[
ExpandedRow(
crossAxisAlignment: CrossAxisAlignment.stretch,
children: <Widget>[
ExpandedButton(
child: Text(
'7',
style: TextStyle(color: Colors.white),
),
onPressed: () {add('7');},
color: Colors.blueAccent,
),
ExpandedButton(
child: Text(
'8',
style: TextStyle(color: Colors.white),
),
onPressed: () {add('8');},
color: Colors.blueAccent,
),
ExpandedButton(
child: Text(
'9',
style: TextStyle(color: Colors.white),
),
onPressed: () {add('9');},
color: Colors.blueAccent,
),
],
),
ExpandedRow(
crossAxisAlignment: CrossAxisAlignment.stretch,
children:<Widget>[
ExpandedButton(
child: Text(
'4',
style: TextStyle(color: Colors.white),
),
onPressed: () {add('4');},
```

```
color: Colors.blueAccent,
),
ExpandedButton(
  child: Text(
    '5',
    style: TextStyle(color: Colors.white),
  ),
  onPressed: () {add('5');},
  color: Colors.blueAccent,
),
ExpandedButton(
  child: Text(
    '6',
    style: TextStyle(color: Colors.white),
  ),
  onPressed: () {add('6');},
  color: Colors.blueAccent,
),
],
),
ExpandedRow(
  crossAxisAlignment: CrossAxisAlignment.stretch,
  children: <Widget>[
    ExpandedButton(
      child: Text(
        '1',
        style: TextStyle(color: Colors.white),
      ),
      onPressed: () {add('1');},
      color: Colors.blueAccent,
    ),
    ExpandedButton(
      child: Text(
        '2',
        style: TextStyle(color: Colors.white),
      ),
      onPressed: () {add('2');},
      color: Colors.blueAccent,
    ),
    ExpandedButton(
      child: Text(
        '3',
        style: TextStyle(color: Colors.white)
      ),
      onPressed: () {add('3');},
      color: Colors.blueAccent,
    ),
  ],
),
ExpandedRow(
  crossAxisAlignment: CrossAxisAlignment.stretch,
  children: <Widget>[
    ExpandedButton(
      child: Text(
        '0',
        style: TextStyle(color: Colors.white),
      ),
      onPressed: () {add('0');},
      color: Colors.blueAccent,
    ),
    ExpandedButton(
      child: Text(
        ',',
        style: TextStyle(color: Colors.white),
      ),
      onPressed: () {add(',');},
```

```
color: Colors.blueAccent,
),
ExpandedButton(child: Text('='), onPressed: () {
getResult();
},
color: Colors.blue[50]),
],
),
],
),
Expanded(
flex: 1,
child: Column(
crossAxisAlignment: CrossAxisAlignment.stretch,
children: <Widget>[
ExpandedButton(
child: Image.asset(
"icons/divide.png",
width: 10.0,
height: 10.0,
),
onPressed: () {add('÷');},
color: Colors.blue[50],
),
ExpandedButton(
child: Text('x'),
onPressed: () {add('x');},
color: Colors.blue[50],
),
ExpandedButton(
child: Text('-'),
onPressed: () {add('-');},
color: Colors.blue[50],
),
ExpandedButton(
child: Text('+'),
onPressed: () {add('+');},
color: Colors.blue[50],
),
],
),
],
),
],
);
}
}
```

Running the app produces the following visual result:

## Implement the Calculations

Now it’s time to make this really nice-looking calculator work. The way our calculator logic is going to work is not how expressions are usually parsed, but this way will show you how to parse and manipulate strings in Dart, and it will also show you how to do that using regular expressions. It’s not the simplest solution and it’s surely not the one with the lowest computational complexity, but it’s a good example and it is supposed to make you more confident when writing Dart code that operates on strings.

The [Calculation](Calculation) class needs to handle a few basic operations:

+   1. Add a digit or operator.
+   2. Display the current string containing all of the digits and operators that have been added.
+   3. Compute the result of the current expression.
+   4. Delete a digit or operator.
+   5. Delete the entire expression.

Being able to display the current string can also be used for debugging insertion and deletion as it displays the current state of the calculation.

The [Calculation](Calculation) class needs to be able to take inputs one character at a time and to compute the result of an expression considering mathematical operator precedence rules.

To do that, we will implement the class in the following way: we will have a [List](List) of [String](String)s, each of which will be either an operator or a number to make parsing and computing results simpler.

We will start implementing it by creating a file called `calculator.dart` inside the `lib` directory.

At this point we can start defining our [Calculation](Calculation) and that data it needs inside `calculator.dart`:

```
layout/calculator/lib/calculator.dart
```
