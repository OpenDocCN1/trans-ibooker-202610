![](bbox=[0.21, 0.19, 0.31, 0.28])

![](bbox=[0.43, 0.17, 0.68, 0.29])

![](bbox=[0.77, 0.14, 0.88, 0.32])

Square
Shape-outside: circle(50%)

landscape

portrait

Back to our image, it is square, so we use a shape-outside property with a value of circle(50%) to our image. Listing 7.6 shows our CSS rule.

Our image is square, so it has an aspect ratio of 1 (width / height = 175 ÷ 175 = 1).

ASPECT RATIO: The aspect ratio of an image is the proportional relationship of the image's height and width calculated by dividing the width by the height.

Adding the aspect ratio is not strictly necessary to create our shape but will help reduce layout shifts on load.

LAYOUT SHIFTS: When an element is added to the page, or its size is changed, everything after the element moves to make room for the element or fill in the void left behind. The movement of elements on the page is referred to as a layout shift.

When the image has a set height and width or has a defined aspect ratio, the browser can save room for the image while it is being loaded and therefore reduce this movement. It is therefore good practice to define aspect ratios and or a height and width for our images.

# Listing 7.6 shape-outside

```
img.compass {
  aspect-ratio: 1; #A
  float: right;  #B
  shape-outside: circle(50%); #C
}
```

#A Aspect ratio
#B Float the image to the right
#C Add our circle with a value of 50%

Figure 7.8 displays our output. The text wraps around the image and follows the curve, but our image is not clipped in any way.