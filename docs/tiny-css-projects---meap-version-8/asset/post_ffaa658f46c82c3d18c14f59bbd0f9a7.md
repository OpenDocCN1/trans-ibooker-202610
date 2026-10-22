
Next, we are going to give the inputs some padding, custom borders, and border-radius. In this particular case, these styles are going to be for stylistic purposes. Most applications will have a general style, or look and feel. The styles we choose to apply to our fields should be in the same vein as the rest of our application’s general theme. By doing this we help the form blend with the page and look like it belongs. From a marketing perspective, keeping with your theme will also help reinforce brand recognition. For this project we have chosen a very round theme so we will give our fields exaggerated round corners.

Lastly, we will also give our fields a display value of block (inputs have a display property value of inline by default) and make them take the full width of our container. We will use the calc() function to calculate the full width of the container (100%) minus the padding we have added to the input field (2rem on each side for a total of 4 in width). Remember the box model? Padding adds to the width of the element. Therefore, we must remove the total quantity of left and right padding from our width. A width of 100% would equate a width of 100% of the available width of the container, then the padding value would be added. We would therefore be 4rem larger than our allotted space if we did not subtract the padding.

Our code looks as follows (Listing 10.7).

## Listing 10.7 Styling input fields that are not of type radio or checkbox

```
input:not([type="radio"], [type="checkbox"]) {
  background: var(--background);
  font-size: 1rem;
  color: inherit;
  border: solid 1px var(--border);
  border-radius: 32px;
  padding: 1rem 2rem;

  display: block;
  width: calc(100% - 4rem); /*A
}
```

### A Calculating what the width of our element should be

We have some basic styles set for our text and email inputs, displayed in Figure 10.9. We have now started to develop our theme for our form controls.

![](img/b56145aa9b751926a7aedbd7172bc818_262_0.png)

Figure 10.9 Text and Email Input Styles

## 10.3.2 Making selects and textareas match the input styles

To make sure our look and feel are consistent across our controls, let’s apply the same styles we applied to the input field to the textarea and select elements. We are not going to create new rules, or copy and paste the code. To keep our styles consistent and maintainable, we will add select and textarea as selectors to our already written rules like so:

```
input:not([type="radio"], [type="checkbox"]), textarea, select { ... }.
```

Once applied, we notice that both fields still need a little bit of extra styling. Let’s focus on the textarea first. In Figure 10.10 we can see that the textarea has the wrong typeface. The same way we inherited color earlier, we are going to also need to inherit the font-family value. Although the very rounded corners work well on the slimmer input fields, it looks a bit off on the textarea due to its height, so we are going to reduce the border-radius on just the textarea.

Figure 10.10 shows our updated text area.

![](img/b56145aa9b751926a7aedbd7172bc818_263_0.png)

Figure 10.10 Updated Textarea Styles

By default on the web, textareas can be resized both in width and in height by the user by clicking and dragging the bottom right hand corner. In our layout, increasing or decreasing the height does not cause any layout issues, width however is problematic as we can observe in Figure 10.11.

![](img/b56145aa9b751926a7aedbd7172bc818_264_0.png)

Figure 10.11 Textarea Resize Issue

The textarea extends outside of the container in an unsightly fashion. When we resize vertically, the container resizes appropriately but this is not the case horizontally. By changing the textarea’s resize property’s value from its default of both to vertical, we can limit the user’s ability to resize the textarea. They will continue to be able to change its height but no longer the width. Visually the textbox still looks the same, and still has the control on the bottom right to resize it as demonstrated by Figure 10.12. When the user interacts with the resize control however, they will be constrained to only resizing vertically.

![](img/b56145aa9b751926a7aedbd7172bc818_264_1.png)

Figure 10.12 Textarea Vertical Resize Only

6 https://developer.mozilla.org/en-US/docs/Web/CSS/resize

The edits to our base style and extras added specifically to textarea are found below in Listing 10.8.

## Listing 10.8 Updated Styles for Textarea

```
input:not([type="radio"], [type="checkbox"]),
textarea,
select {
  background: var(--background);
  font-size: 1rem;
  font-family: inherit;
  color: inherit;
  border: solid 1px var(--border);
  border-radius: 32px;
  padding: 1rem 2rem;

  display: block;
  width: calc(100% - 4rem);
}

textarea {
  border-radius: 4px;
  resize: vertical;
}
```

### A Added textarea and select to our styles
### B Make the controls inherit font size from the parent container
### C Added rule specific to textarea for border radius and resizing

We still need to address the select, but this will be a bit more involved then the edits we made for textarea, so let’s finish styling the input fields first and then we will circle back and finish styling the select control.

## 10.3.3 Styling radio inputs and checkboxes

Earlier in the chapter we talked a little bit about how some form controls are notoriously difficult to style because our options for styles that can be applied to them are incredibly limited. Radios and checkboxes fall squarely into that category. Until recently there were no properties that affect the radio circle or checkbox square whatsoever. Our only option was to completely replace the native control styles with our own.

We now have the ability to change the native control’s color. The accent-color property allows us to replace the user agent’s chosen color with the color we specify. Applying accent-color: var(--accent); to our checkbox and radios will yield the following results (Figure 10.13).

![](img/b56145aa9b751926a7aedbd7172bc818_266_0.png)

Figure 10.13 Accent Color Applied to Radios and Checkboxes

The elements have taken our set accent color instead of the light blue default color it was using beforehand. If I increase the font-size on the application however, we see in Figure 10.14 that the controls do not increase in size.

![](img/b56145aa9b751926a7aedbd7172bc818_266_1.png)

Figure 10.14 Increasing Font Size on Radio Inputs and Checkboxes

Although we can change the color of the element (which is a very effective way to quickly and efficiently style the control to fit our styles better) if we want to allow the control to scale with our font size, or make any further customizations we will need to replace the controls styles with our own.

Since we want to keep the functionality of the control and only replace the visual aspect of it, our HTML stays the same. We are however going to hide the native control provided by the browser, and replace the visual portion with our own custom styles. To hide the native control we are going to use the appearance property and give it a value of none. This property allows us to control the native appearance of the control. By setting its property to none, we are saying that we do not want it to display the operating system provided styles. We will also set the background-color to our own background color because some operating systems include a background to the controls and then finally reset our margins. The completed reset is shown in Listing 10.9.

## Listing 10.9 Radio and Checkbox Reset

```
input[type="radio"],
input[type="checkbox"] {
  appearance: none;

```
```
background-color: var(--background);
margin: 0;
}
```

### A Styles are only being applied to inputs that have a type of radio or checkbox

Figure 10.15 shows us that our radio input and our buttons have now disappeared. We can therefore start creating our own styles for the controls.

![](img/b56145aa9b751926a7aedbd7172bc818_267_0.png)

Figure 10.15 Reset Radio and Checkbox Styles

What we want to do is create a box. For radio inputs we will give that box a border-radius in order to make it round, but at the core regardless of checkbox or radio, we need a box. We will therefore create one by giving the input a height and a width of 1.25em. We use em units because they are a percentage of the parent’s font size. By setting our height and width to 1.25rem, we are setting them to equal 1 and a quarter times the value of font size of the parent. If our label, the container and therefore parent of our input, has a font size or 16 pixels, our box will therefore have dimensions of 20 pixels wide by 20 pixels tall (16 × 1.25 =20).

We will then add a border to our box that inherits our label’s font color. I know this sounds a little weird, how are we going to make border color inherit from font color? We are going to use a keyword value currentColor which allows properties to inherit specifically font color when they otherwise could not. We are going to set the border color to currentColor which will make the border color match the font color. To set our border width we are also going to use em, same as with our height and width, which will allow the width of our border to also scale with the size of our radio button.

Because inputs are inline elements by default, in order for the height and width to be applied, we will also need to change the display property. We will set it to inline-grid because when we handle the checked state for our inputs, we will need to center the inner disk or check mark. Grid allows us to do so easily using the place-content property.

Inline-grid is to grid as inline-block is to block. inline-block has all the same properties as block, but places itself inline in the page flow. inline-grid works the same way. We have access to all of the features of grid, but the element will place itself inline in the page flow rather than below the previous content. For our purpose this means that the

9 https://developer.mozilla.org/en-US/docs/Web/CSS/color_value#currentcolor_keyword
10 https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Grid_Layout/Basic_Concepts_of_Grid_Layout
