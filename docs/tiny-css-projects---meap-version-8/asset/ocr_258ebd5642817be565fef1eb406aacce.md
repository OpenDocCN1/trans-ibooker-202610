For our project we will specify that we want 2 columns and then for the sizing of each column we will use the minmax function. Our column definition will therefore looks as follows:

```
grid-template-columns: repeat(2, minmax(auto, 2fr)) 250px;
```

This declaration will produce 3 columns, 2 of equal width using the fraction unit and one of 250 pixels. Let’s dissect this declaration a bit further. Notice that inside the repeat function, we use another function minmax().

# 2.2.2 Minmax

The minmax(min, max) function is made up of two arguments, the minimum and maximum range the grid track should be. The W3C specification states that the minmax function "defines a size range greater than or equal to min and less than or equal to max"¹.

# Valid arguments

To make the function valid the min value (the first argument) needs to be smaller than the max, otherwise max gets ignored by the browser and the function will rely just on the min value.

For our project we set the minimum value to auto and the max to 2 fractions. Let’s look at what auto means.

# 2.2.3 The auto keyword

The auto keyword can be used for either the minimum or maximum value within the function.

When the keyword auto is used for the maximum value then the minimum dimension the container will take is equal to the max-content keyword. To make layouts that include fixed headers and footers, we can assign overflow to the area for whom auto was assigned. The track that has a size value of auto will then shrink and grow with the window size as seen in Figure 2.5.