![](bbox=[0.146, 0.094, 0.353, 0.261])

![](bbox=[0.393, 0.094, 0.602, 0.261])

![](bbox=[0.648, 0.094, 0.855, 0.261])

![](bbox=[0.146, 0.352, 0.353, 0.519])

![](bbox=[0.393, 0.352, 0.602, 0.519])

![](bbox=[0.648, 0.352, 0.855, 0.519])

Figure 7-1. Light background colors look best with dark font, and dark background colors look best with light font

In computer science one way to represent a color is with RGB values, or the “red”, “green”, and “blue” values. Each of these values are between 0 and 255 and express how these three colors are mixed to create the desired color. For example, if we express the RGB as (red, green, blue) then dark orange would have an RGB of (255, 140, 0) and pink would be (255, 192, 203). Black would be (0, 0, 0) and white would be (255, 255, 255).

From a machine learning and regression perspective, we have three numeric input variables red, green, and blue to capture a given background color. We need to fit a function to these input variables and output whether a light (1) or dark (0) font should be used for that background color.