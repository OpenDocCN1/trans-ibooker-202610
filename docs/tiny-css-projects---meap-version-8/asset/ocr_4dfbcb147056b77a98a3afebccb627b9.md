In HTML, the initial value for this property is 50% 50% 0 so this is center, center, flat. However, for SVG elements the initial value is 0, 0 which would place it in the top left hand corner.

For our animation we want the rectangle transform origin to be at the center. We want the top and bottom of the rectangles to shrink rather than the top staying fixed in place and expanding and retracting from that point. To do this we can either apply the keyword value `center` or set a percentage value of `50%` to the `transform-origin` property for our rectangles.

Either way we are saying we want the point of origin to happen from the center of the rectangle. For our project, we will use the keyword value. Listing 3.9 shows our updated `rect` rule.

We mentioned earlier that when working with 2D animations, the property would take 2 values, but we only passed one. When only one value is passed, it will be applied to both the vertical and horizontal position, therefore `transform-origin: center;` is equivalent to `transform-origin: center center;`.

# Listing 3.11 Updated rect rule with transform-origin property

```
rect {
  animation: scale 2.2s infinite;
  transform-origin: center;
}
```

We have finished our loader animation (Figure 3.10) however we still need to consider how accessible our design is.

![](bbox=[0.18, 0.57, 0.88, 0.83])

Figure 3.10 Finished Loader Animation

The next section will dive into some of the ways we can make sure to provide a positive experience for all of our users.