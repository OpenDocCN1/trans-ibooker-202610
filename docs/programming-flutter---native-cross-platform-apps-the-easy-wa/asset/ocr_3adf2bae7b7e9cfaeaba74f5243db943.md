```
```dart
canvas.drawCircle(size.center(Offset(0, 0)), 40.0, whitePaint);

}

@override
bool shouldRepaint(CustomPainter old) {
  return false;
}
}
```
```

and a blue and white one:

```
```dart
class BlueWhiteBluePainter extends CustomPainter {
  @override
  void paint(Canvas canvas, Size size) {
    var bluePaint = Paint();
    bluePaint.style = PaintingStyle.fill;
    bluePaint.color = Colors.blue;
    var whitePaint = Paint();
    whitePaint.style = PaintingStyle.fill;
    whitePaint.color = Colors.white;
    canvas.drawColor(Colors.blue, BlendMode.color);
    canvas.drawOval(
      Rect.fromPoints(
        size.topLeft(Offset(-40, -100)),
        size.bottomRight(Offset(15, 10))
      ),
      bluePaint
    );
    canvas.drawOval(
      Rect.fromPoints(
        size.topLeft(Offset(-15, -10)),
        size.bottomRight(Offset(40, 100))
      ),
      bluePaint
    );
    canvas.drawCircle(size.center(Offset(0, 0)), 40.0, whitePaint);
  }

  @override
  bool shouldRepaint(CustomPainter old) {
    return false;
  }
}
```
```

We’ll use them in an app for two different widgets in a Row that will each pass themselves to a second view:

```
```dart
class FirstPage extends StatelessWidget {

  final widgets = [
    Hero(
      tag: "Blue White Red",
      child: CustomPaint(
        painter: BlueWhiteBluePainter(),
        child: Text("1"),
      ),
    ),
    Hero(
      tag: "Green White Red",
```
```