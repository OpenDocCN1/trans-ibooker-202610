![](bbox=[0.15, 0.09, 0.34, 0.31])

# HERO ANIMATIONS COMBINED WITH CUSTOMPAINT

The combination of these canvas painting abilities with hero animation could make for interesting user interaction animation in Flutter apps: let’s imagine having two widgets similar to the one we’ve seen in the previous section, but in different colors and that serve as links to different views or different versions of the same view.

Let’s start by defining the painters: a red, green and white one just like the previous one:

```
class GreenWhiteRedPainter extends CustomPainter {
  @override
  void paint(Canvas canvas, Size size) {
    var greenPaint = Paint();
    greenPaint.style = PaintingStyle.fill;
    greenPaint.color = Colors.green;
    var redPaint = Paint();
    redPaint.style = PaintingStyle.fill;
    redPaint.color = Colors.red;
    var whitePaint = Paint();
    whitePaint.style = PaintingStyle.fill;
    whitePaint.color = Colors.white;
    canvas.drawColor(Colors.blue, BlendMode.color);
    canvas.drawOval(
      Rect.fromPoints(
        size.topLeft(Offset(-40, -100)),
        size.bottomRight(Offset(15, 10))
      ),
      greenPaint
    );
    canvas.drawOval(
      Rect.fromPoints(
        size.topLeft(Offset(-15, -10)),
        size.bottomRight(Offset(40, 100))
      ),
      redPaint
    );
  }
}
```