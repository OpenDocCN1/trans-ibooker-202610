![](bbox=[0.18, 0.13, 0.9, 0.46])

Figure 6.4 Full screen gradient background

# 6.3 Styling and centering the card using flexbox

Let’s start with styling the card itself. We will give it a white background and shadow to give our layout some depth. Notice that instead of using the color value for the background, we use our background variable.

We are also going to set the width of the card to 75vw. Viewport Width (vw) is the horizontal counterpart to the viewport height (vh) unit we used earlier. It is also percentage based, so by setting our width to 75vw we are setting the width of the card to be 75% of the total width of the browser window.

We are then going to further constrain the width of the card to a maximum of 500 pixels wide. By using both the width and max-width properties, we allow for the card to shrink when the screen size is narrow but constrain the card from becoming too wide and unruly on larger screens.

Lastly we curve the corners of the card using border radius to soften the design. Listing 6.4 shows our card rule.

# Listing 6.4 Styling the card

```
.card {
  background-color: var(--card-background);
  box-shadow: 0px 0px 55px rgba(38, 40, 45, .75);
  width: 75vw;
  max-width: 500px;
  border-radius: 4px;
}
```