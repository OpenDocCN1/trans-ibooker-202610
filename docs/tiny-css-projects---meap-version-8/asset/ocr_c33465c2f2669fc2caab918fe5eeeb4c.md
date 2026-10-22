Notice that both our viewbox and viewport heights are equal to 300. This is how we zoom. If the viewbox figures are less than the viewport then you are effectively zooming out to the frame, the graphic will be smaller. If the viewbox figures are more than the viewport then you are zooming in. Since we have equal viewport and viewbox heights, we are not zooming.

Now that we have defined the space we will be working in, we can start adding shapes to the loader.

# 3.1.4 Shapes in SVG

There are a few standard SVG shapes/elements:

+   - `rect` (Rectangle )
+   - `circle`
+   - `ellipse`
+   - `line`
+   - `polyline`
+   - `pologyon`

If we want to create an irregular shape, we can also use path. But we will not need it for this loader. Most often, they are what we see when we look at the XML behind logos, icons and complex animation graphics. For our project, we will use the basic rectangle shape to create the wave.

To define our rectangles which will create the bars in our loader, we will use the `<rect>` element and add 4 properties: `height`, `width`, `x`, and `y`. The x and y attributes will determine the position of the top left corner of the rectangle relative to the top left hand corner of the SVG.

We will want to create 11 rectangles (seen in Listing 3.4) that have a width of 60, a height of 300 and then we will use the x attribute to move the rectangles across the graphic. We will start at 0 and increase the value by the width of our bar (60) plus an additional gap of 5. Each rectangle’s x value will be 65 more than the previous. Our 11th rectangle should have an x value of 650.

```
Listing 3.4 11 rectangles
<svg viewBox="0 0 710 300" width="100%" height="300">
  <rect width="60" height="300" x="0" />
  <rect width="60" height="300" x="65" />
  <rect width="60" height="300" x="130" />
  <rect width="60" height="300" x="195" />
  <rect width="60" height="300" x="260" />
  <rect width="60" height="300" x="325" />
  <rect width="60" height="300" x="390" />
  <rect width="60" height="300" x="455" />
  <rect width="60" height="300" x="520" />
  <rect width="60" height="300" x="585" />
  <rect width="60" height="300" x="650" />
</svg>
```

We now have our rectangles positioned inside of our viewport and they are being correctly resized as we increase and decrease the window size by our viewBox. Figure 3.6 shows our