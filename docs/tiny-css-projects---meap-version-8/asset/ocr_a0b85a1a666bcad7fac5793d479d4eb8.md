![](bbox=[0.22, 0.14, 0.88, 0.31])

Figure 8.24 Effects of margin and margin collapse

To add space between the edges of the card and the header, we replace the card header’s margin with padding. Then we will add margin to the card in order to regain the lost vertical space between them. Lastly we add padding to the body so that the cards are not stuck right up against the left and right edges of the screen. Listing 8.18 shows how this is done.

# Listing 8.18 Section Margin and Header Padding

```
body { padding: 1rem; }
section { margin: 1rem 0; }
section h2 {
  padding: 1rem;
  margin: 0;
}
```

With the mobile layout now finished, let’s increase the width of the screen and consider tablet and laptop sized screens.

# 8.4 Medium screen size layout

Most of what we did for mobile will look great on medium sized screens. Since we used a media query to restrict table layout changes to screens less than to 459 pixels, the styles we wrote to edit the table will not apply to any screen 500 pixels and up. Figure 8.25 shows the table when the viewport is 449 pixels wide and when it is 550 pixels wide. At 550 pixels of width, we are back to a standard table layout.