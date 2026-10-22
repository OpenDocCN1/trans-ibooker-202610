After adding the delay, we can see (Figure 3.8) that we have achieved our staggered effect. However, the elements are growing and shrinking from the top rather than the center.

![](bbox=[0.18, 0.19, 0.9, 0.46])

Figure 3.9 Animated rectangles with height change emanating from the top

To dictate where we want the element to grow and shrink from we need to tell the browser where on the rectangle the animation should originate from. To tackle this we will now look at the transform-origin property.

# 3.4 Transform origin

The transform-origin property sets the position the animation should start from (the point of origin). If the transform is happening in three-dimensions (3D) then the value can be up to three values (x, y, and z); when it’s two-dimensions (2D) then it is up to 2 values (x and y).

The first value is the horizontal position or the X axis, where the second value is the vertical or the Y axis and then when working in 3D the third value would be the forwards and backwards or the Z axis.

There are 3 ways to declare the value of the transform-origin:

+   1. Length
+   2. Percentage
+   3. Keywords
+      a) Top
+      b) Right
+      c) Bottom
+      d) Left
+      e) Center