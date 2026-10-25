function does the same thing as the code in that section but
does it many times as fast.

> Performance Tip
> You should observe through this chapter and Chapter 12 that many numpy functions
> echo actions that can be performed in Chapter 11, “The Random and Math
> Packages,” by importing random and math. But the numpy versions, especially with
> large data sets (such as the 2,000,000 random numbers generated for the most
> recent examples) will be many times as fast.
> So when you have a choice, prefer to use numpy, including its random sub-
> package, for large numeric operations.

Some of the other arguments are occasionally useful. To learn
about all of them, you can get help from within IDLE.

```
>>> np.info(np.histogram)
```

# 13.7 CIRCLES AND THE ASPECT
RATIO

Sometimes you’ll want to adjust the relative size of the X and Y
axes; this is especially true when you draw a geometric shape. In
this section, we’ll show how to draw a circle. Usually, you don’t
want it to look like an ellipse, so we’ll adjust the aspect ratio
between X and Y to be equal before showing the graph.

There’s more than one way to draw a circle; the other way is
left as an exercise for the end of the chapter. In this section,
we’ll use an approach utilizing trig functions. Imagine a little
bug traveling around the outside of a circle, starting at o