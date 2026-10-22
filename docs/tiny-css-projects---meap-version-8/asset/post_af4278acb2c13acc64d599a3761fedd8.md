
```
background: radial-gradient(black, white, black, white, black)
```

```
background: radial-gradient(circle at 20%, black, white);
```

```
background: radial-gradient(black, 30%, white, 30%, white);
```

```
background-image: radial-gradient(black, 30%, white, 30%, white);
background-size: 25px 25px;
```

![](img/b56145aa9b751926a7aedbd7172bc818_255_0.png)

![](img/b56145aa9b751926a7aedbd7172bc818_255_1.png)

![](img/b56145aa9b751926a7aedbd7172bc818_255_2.png)

![](img/b56145aa9b751926a7aedbd7172bc818_255_3.png)

Changes from back to white at equal interval starting from the center

Center is vertically centered but 20% from the edge and fades from black to white

Creates a start black until 30% of circle and immediately shifts immediately to white which creates a circle

Same as the previous but the background is given a size and allowed to repeat which created a polka dotted background.

## Figure 10.4 Radial Gradient²

We will create a 15px by 15px image with a very small dot in the center which we will let repeat over the body. By default, when using background images (and gradients are no exception) if the image is smaller than the container, the browser will repeat the image as many times as it can (both vertically and horizontally) to fill the container unless otherwise explicitly specified using the background-repeat property. Because the image we created is a 15px square and our page is much wider and taller than that, the background image will automatically tile or repeat to fill the entire element.

We will keep the gradient starting from the center from which we will apply our accent color. After 0.75 pixels, we will then immediately shift to being transparent to let our background color show through. If we switched from accent color to transparent at 7.5 pixels (half of 15) our circle would be exactly 15 pixels in diameter. Since we are doing the switch at 0.75 pixels, our circle will be 1.5 pixels in diameter. Our background-color property value will therefore look as follows: radial-gradient(var(--accent) .75px, transparent .75px);

In addition to adding the background to the body, we will also set our margins to 0 and add 2rem of padding to prevent content from coming all the way to the edge of the screen. Listing 10.3 shows the styles applied to the body thus far.

## Listing 10.3 Colors and Font Family Custom Properties

```
body {
  background-color: var(--background);
  background-image: radial-gradient(var(--accent) .75px, transparent .75px);  #A
```

² https://codepen.io/martine-dowden/pen/poajdeQ

```
background-size: 15px 15px;
margin: 0;
padding: 2rem;
}
#A
```

#A properties used to created the dot grid in the background

With the page background applied, shown in Figure 10.5, we will now style the <main> tag which is the container around the header and form.

![](img/b56145aa9b751926a7aedbd7172bc818_256_0.png)

Figure 10.5 Dot Grid Background Created using Radial Gradient

## 10.1.3 Emphasizing the form

By giving main a brighter solid background and some shadow, we make it stand out and it becomes the visual focus of the page. Adding a drop shadow will further emphasize its differentiation from the background. The shadow does not have to be particularly harsh, the one we will apply will be rather subtle but will still go a long way to really bringing out the content as the element to pay attention to.

We will also cap the width which the element can have to a maximum of to 600 pixels. Looking at Figure 10.4 we can see that block elements such as the fieldset around the radio buttons will expand as far as the screen will go. Much like text, if a control is too wide, eye tracking will become difficult and the user may lose their place while attempting to fill the form. Setting a maximum width prevents this issue.

We will then center the element by setting our right and left margins to auto. We will also set vertical margins of 1rem both top and bottom to move it away from the top and bottom edges of the screen.

The properties and values used to style the form container are found in Listing 10.4.

## Listing 10.4 Form Container Styles

```
main {
  background: var(--background-card);
  padding: 2rem;
  max-width: 600px;
  margin: 1rem auto;
  box-shadow: -2px 2px 15px 0 var(--border);
}
```

We gave our container a background, but we can now see in Figure 10.6 that our content is right up against its edge.

![](img/b56145aa9b751926a7aedbd7172bc818_257_0.png)

Figure 10.6 Form Container Background

To prevent the content from being so tightly fitted inside of the container, we will add 2rem of padding to the container.

We will also make our header ("Contact") more distinct by applying our accent color to it at which point we will be ready to start styling the form elements. The final touches for the container and header are shown in listing 10.5.

## Listing 10.5 Finishing Touches to the Container and Header Styles

```
main {
  ...
  padding: 2rem; #A
}
```

```
h1 { color: var(--accent); } #B
```

#A Adding padding to the previously created main rule
#B Making the header text blue (our accent color) by referencing the variable we set on the body

With our color palette, background, and page layout now set (Figure 10.7), we are now ready to style the elements in the form itself.

![](img/b56145aa9b751926a7aedbd7172bc818_258_0.png)

Figure 10.7 Form Container Background with padding and styled header

## 10.2 Resetting fieldset styles

Fieldsets are purpose built to group controls and labels. Radio groups are a perfect use case for them as it allows us to very effectively and explicitly identify them as belonging together. It also gives us a ready-built way of labeling the group via the legend. Stylistically however, I think we can agree that they are rather unsightly.

Let’s reset the styles on the group in order to make it visually disappear. Programmatically, we want to keep it as it is helpful for assistive technology users, but visually, we are going to make it blend in a little more.

To make the fieldset disappear will will need to reset the following properties:

- border
- margin
- padding
- min-width

Listing 10.6 shows our rule

## Listing 10.6 Resetting Fieldset Styles

```
fieldset {
  border: 0;
  padding: 0;
  min-width: 0;
  margin: 0;
}
```

With our fielset now no longer visible as seen in Figure 10.8, let’s turn our attention to our input fields.

![](img/b56145aa9b751926a7aedbd7172bc818_259_0.png)

Figure 10.8 Form Container Background

## 10.3 Styling input fields

We have 4 types of input fields in our form broken down in Table 10.1

## Table 10.1 Input field types in the form

| Field Label | Type |
|---|---|
| Your Name | text |
| Your Email Address | email |
| Yes
No | radio |
| Subscribe to our newsletter | checkbox |

HTML has many more types of fields including date, time, number, color, ect. each with their own semantic meaning and their own styling considerations. The sampling above was chosen because these are very commonly used types on the web today.

Their unstyled look dictates what we will do to style them. We will treat radios and checkboxes very differently than the text input for example but we can reuse code across multiple types. We will group them by how their unstyled control looks, so we will handle the type text and email together and then the radio and checkbox together. Let’s start with the text and email inputs.

### 10.3.1 Styling text and email inputs

The first thing we want to figure out is how to select just text and email input fields, or rather, all input fields that are not a radio or checkbox. One solution would be to add a class to each of the inputs we want to handle. This is rather hard to maintain and will get quite noisy especially in a form heavy application or complex form. We will therefore use the pseudo-class :not()³ in conjunction with a type selector selector[type="value"].

The :not() pseudo-class allows us to select elements that do not meet a particular criteria. In our case, we want to select all input fields that do not have a type of radio or checkbox. Our selector will therefore be input:not([type="radio"], [type="checkbox"]).

We can now start styling the input field. We can see from figure 10.8 that our inputs have a white background. We are going to change that to being the same background color we used on the body. We can also notice that the font size is smaller than the 12pt size we set on the body. Input fields do not inherit font styles. Small font sizes are difficult to read on mobile and for many users, especially young children and the elderly. If we want our form to be easily usable across a wide population and across devices, we will need to increase it. We will therefore set it to 1rem to match the rest of our application. As mentioned earlier, inputs do not inherit font styles by default, we will therefore also explicitly set the color to inherit. inherit⁴ is a very handy property value. It allows us to forcibly inherit a property value from the parent when inheritance does not happen by default.
