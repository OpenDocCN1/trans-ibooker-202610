![](bbox=[0.185, 0.132, 0.898, 0.283])

Figure 10.25 Full width select with no arrow

# 10.4.2 Using linear-gradients to create an arrow

Our next order of business will be to create a new arrow whose positioning we can control. Remember at the beginning of the chapter, we used a gradient to create a circle? We are going to use this trick again to create our arrow which we will add to our dropdown using the background-image property.. Instead of a radial-gradient however, we will use a linear-gradient.

Much like radial-gradient, linear-gradient will take a series of colors and percentages for when they should start and stop, but instead of the colors being displayed in a circular fashion around a center point, they will be placed in a linear fashion towards a side or corner. It can also take an angle to dictate the direction of the lines of color. Table 10.3 shows the angle and their keyword equivalents when dictating the direction of the lines of color.

Table 10.3 Keyword and Angle Equivalent

| Keyword | Angle in degrees |
|---|---|
| to top | 0 |
| to bottom | 180 |
| to left | 270 |
| to right | 90 |

Same as with the radial-gradient, if we stop and stop a color at the same percentage, we will get a sharp line rather than a fade from one color to another.

The background-image property allows us to add multiple images to our background. We are going to exploit this functionality. We will need three overlapping gradients to create our arrow which will all be assigned to our background-image property.