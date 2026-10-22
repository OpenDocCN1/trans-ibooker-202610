```
}
#A
button.secondary,
#B
.button.secondary {
#B
  border-color: #2c6c69;
#B
  color: #2c6c69;
#B
}
```

#A is applied to the "Proceed to checkout" link
#B is applied to the "Continue Shopping" link

Left to style is the remove item button found in the table for each of the cart items in the cart which have been given a class of destructive.

Same as for the previous 2 types we will want to change the colors, this time to red instead of teal, to emphasize that this is a destructive action. We will also need to make some adjustments to make the button circular and to center the image. To make the button look like a circle, we will increase the border radius to 25% but as we see in figure 8.8, that only gets us part way. Our button is now oval (vertically elongated).

![](bbox=[0.18, 0.395, 0.83, 0.5])

Figure 8.8 Oval remove item button

To ensure that our button is circular we are going to give it an equal height and width. The image being used for the remove icon is 24 pixels by 24 pixels. We are going to make our button size match our image size. We therefore need to add the dimensions of the image plus the border: width = 24 + 2 = 26. We add 2 because our border is 1 pixel wide, and there is a border on the right and the left. We then do the same thing for the height.

Since we want the button to be the same size as the image, we also need to get rid of the padding inside of the button.

We did not edit the hover and focus styles for the action buttons because the color used for the outline (teal - #2c6c69) in the base styles we set matched the colors we used for those links. This is not true of this button. Our button outline and image are red; we are therefore going to make the outline red on hover and focus.

Listing 8.6 shows the CSS for the listing button and Figure 8.9 shows the output for each state.

# Listing 8.6 Remove Button

```
button.destructive {
  border-color: #9d1616;
  color: #9d1616;
  border-radius: 50%;
  height: 26px;
  width: 26px;
  padding: 0;
}
```