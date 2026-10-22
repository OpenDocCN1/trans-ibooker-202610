We first give our background a height and width of 1rem, creating a square. We then set the background image to our three gradients. The first value will be on top, the last on the bottom. Working from the bottom to the top, we start with a gradient that goes from accent color to accent color. In other words, we create a blue box. We then create a second gradient that is placed on top of the square. Our second gradient splits the box from bottom left to top right, where the top is transparent and bottom is our background color. This creates a triangle obfuscating the bottom right of the box. Our third will be the opposing diagonal, from top left to bottom right with again, the top transparent, and the bottom our background color. This creates another triangle which obfuscates the bottom bottom left of the box. This leaves an inverted blue triangle 1rem wide and half a rem tall exposed. Figure 10.26 illustrates the process.

![](bbox=[0.18, 0.36, 0.85, 0.46])

Figure 10.26 Creating an arrow from superimposed gradients

All that is left is to position our background and prevent it from repeating. We want the background to be 1rem from the right side of our control and centered. We can use the calc() function to do the math for us. To calculate the horizontal position we take the full width (100%) and subtract 1rem. For the vertical positioning, we know that we have 1rem of padding top and bottom plus our text which also has a size of 1rem making our control 3rem in height. We can therefore position our arrow at 50% below the top. Since the height of the arrow is only half a rem, we then add ⅓rem to the top position.

All put together we get the following rule for our select (Listing 10.18):

# Listing 10.18 Styled Select

```
select {
  appearance: none;
  width: 100%;

  background-image:
    linear-gradient(225deg, transparent 50%, var(--background) 50%),
    linear-gradient(135deg, transparent 50%, var(--background) 50%),
    linear-gradient(var(--accent), var(--accent))
  ;

  background-size: 1rem 1rem;
  background-position: calc(100% - 1rem) calc(50% + .33rem);
  background-repeat: no-repeat;
}
```