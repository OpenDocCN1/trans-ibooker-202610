![](bbox=[0.18, 0.13, 0.88, 0.25])

Figure 10.13 Accent Color Applied to Radios and Checkboxes

The elements have taken our set accent color instead of the light blue default color it was using beforehand. If I increase the font-size on the application however, we see in Figure 10.14 that the controls do not increase in size.

![](bbox=[0.18, 0.37, 0.88, 0.5])

Figure 10.14 Increasing Font Size on Radio Inputs and Checkboxes

Although we can change the color of the element (which is a very effective way to quickly and efficiently style the control to fit our styles better) if we want to allow the control to scale with our font size, or make any further customizations we will need to replace the controls styles with our own.

Since we want to keep the functionality of the control and only replace the visual aspect of it, our HTML stays the same. We are however going to hide the native control provided by the browser, and replace the visual portion with our own custom styles. To hide the native control we are going to use the appearance property and give it a value of none. This property allows us to control the native appearance of the control. By setting its property to none, we are saying that we do not want it to display the operating system provided styles. We will also set the background-color to our own background color because some operating systems include a background to the controls and then finally reset our margins. The completed reset is shown in Listing 10.9.

# Listing 10.9 Radio and Checkbox Reset

```
input[type="radio"],
input[type="checkbox"] {
  appearance: none;
```

8 https://developer.mozilla.org/en-US/docs/Web/CSS/appearance