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