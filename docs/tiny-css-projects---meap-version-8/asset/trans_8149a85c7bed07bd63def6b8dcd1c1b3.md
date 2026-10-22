

![](img/b56145aa9b751926a7aedbd7172bc818_250_0.png)

## Figure 10.1 Starting Point and Finished Product

Shown in listing 10.1, our starting HTML is fairly simple and contains our form inside of which our labels, fields, error messages, and buttons are placed.

The starting and finished code can both be found in github at https://github.com/michaelgearon/Tiny-CSS-Projects/tree/main/chapter-10.

## Listing 10.1 Starting HTML

```
<body>
<main>
<h1>Contact</h1>

<form>
<p>Your opinion is important to us. Please fill out all of the fields.</p>

<label for="name">Your Name</label> #A
<input type="text" id="name" name="name" maxlength="250" #A
required aria-describedby="nameError"> #A
<div class="error" id="nameError"> #A
<span role="alert"> #A
Please let us know who you are #A
</span> #A
</div> #A

<label for="email">Your Email Address</label> #B
<input type="email" id="email" name="email" maxlength="250" #B
required aria-describedby="emailError"> #B
<div class="error" id="emailError"> #B
<span role="alert"> #B
```
```
Please provide a valid email address #B
</span> #B
</div> #B

<label for="reasonForContact"> #C
Reason For Contact #C
</label> #C
<select id="reasonForContact" required> #C
<option value="sales" selected> Sales inquiry</option> #C
<option value="support"> Support request</option> #C
<option value="feedback"> Providing feedback</option> #C
<option value="other"> Other</option> #C
</select> #C

<fieldset> #D
<legend>Are you currently a subscriber?</legend> #D
<label> #D
<input type="radio" value="1" name="subscriber" checked required> #D
Yes #D
</label> #D
<label> #D
<input type="radio" value="0" name="subscriber" required>No</label> #D
</fieldset> #D

<label for="message">Message</label> #E
<textarea id="message" name="message" rows="5" required maxlength="500" aria-describedby="messageError"></textarea> #E
<div class="error" id="messageError"> #E
<span role="alert"> #E
Please let us know how we can help #E
</span> #E
</div> #E

<label> #F
<input type="checkbox" name="subscribe">Subscribe to our newsletter #F
</label> #F

<div class="actions">
<button type="submit" onclick="send(event)">Send</button>
<button type="reset" onclick="setAsPristine()">Reset</button>
</div>

</form>

<script src="./script.js"></script>

</main>

</body>
```

#A Name input with associated label and error message
#B Email input with associated label and error message
#C Reason for Contact Dropdown and associated label
#D Fieldset containing subscription radio buttons
#E Message textarea
#F Subscription Checkmark

## 10.1 Creating backgrounds using radial-gradient

Let’s start with some base typography styles, some colors, and the elements around the form such as the header, container holding our form, and most notably the background. As we can see in figure 10.2, our background has a dotted grid pattern. It is not a premade image, but a gradient that has been created and then repeated.

![](img/b56145aa9b751926a7aedbd7172bc818_252_0.png)

Figure 10.2 Dotted Background

We present this technique now because we will use this technique again later to style the arrow on the drop down. Some elements, <select> being one of them, cannot have ::before or ::after pseudo elements added to them. The select element, like the circles of the radio button, or the checkbox have very limited styling options. This is related to the fact that they rely on the underlying operating system for styles and controls. This is very visible on mobile when a number field automatically opens the number keypad instead of the standard keyboard for example, same for calendar or time controls. Furthermore, when looking at the same website or application on an Iphone versus Android phone, the controls will look drastically different. As a result however, the browser will only allow for a limited number of properties to be applied to the control. Because of our limited ability to style controls, as developers, we have found work arounds, using properties we can affect, the background property is often one that is editable.

Although we could simply use an image that we repeat across the screen to create our gradient, images have their drawbacks in terms of flexibility. For each shape or color we would have to import a new image. We would also need to acquire or create said images. Instead, we can create simple shapes on the fly using a gradient. Although the background is polka dotted and not a gradient, we will be using the radial-gradient function to create the dots.

### 10.1.1 Setting the color scheme using custom properties

Before we look at the gradient, let’s first establish our color palette. We add them as custom properties on the html tag so that we do not have to look up the color codes every time we want to color something and to ensure consistency in our application. An added benefit to using custom properties for our colors is that should we want to add multiple themes to our web page in the future, it could be accomplished with minimal work.

We also include our font family in our custom properties because we will need to apply it to several fields as most fields do not automatically inherit the font-family set on their parent or ancestors. Listing 10.2 shows our color palette and font choice.

## Listing 10.2 Colors and Font Family Custom Properties

```
html {
  --color: #333333;              #A
  --font-family: sans-serif;     #A
  font-size: 12pt;               #A
  color: var(--color);           #A
  font-family: var(--font-family); #A

  --background: #fafafa;         #B
  --background-card: #fff5dc;    #C
  --accent: #086788;             #D
  --accent-contrast: #ffffff;    #E
  --error: #dd1c1aff;            #F
  --border: #ddd;                #G
  --hover: #bee0eb;              #H

  margin: 0;
  padding: 0;
}
```

#A Setting our text default size, font family, and setting the color for the font to a dark gray almost black.
#B Page background color, light gray almost white
#C Background color for the form container, pale yellow
#D Our accent color, used for call to actions and highlighting elements we want to draw the user’s attention to. Blue
#E When the accent color is used as background color, text will be set to the contrast color. White
#F Used for errors. Red
#G Used anytime we have a default border color to set, gray
#H Used for hover effects, very pale blue

Our page, seen in Figure 10.3, looks very similar to how it looked before except for our text which is now a dark gray rather than black and uses my system’s default sans-serif font. Also of note, we notice that the text inside of the message textarea does not look the same as the rest of the page. It is a different font family. An issue we will need to address when we style it later in this chapter.

![](img/b56145aa9b751926a7aedbd7172bc818_253_0.png)

Figure 10.3 Updated Font Style and Color

## 10.1.2 Manipulating a radial gradient

With the colors set, we can start styling our background which we will apply to the <body>.

It is possible to set both a background color and a background image on an element. If instead of using the background property we use individually set values for background-color and background-image, the browser will render both. This is particularly useful when we have images that include transparency or that do not cover the entire element they are applied to. Using our custom property we set in the previous section, we are going to give the body a background-color of var(--background).

Next we will create our gradient. radial-gradient, as its name implies, manipulates colors circularly from the center of the element outwards. The radial-gradient() function allows us to dictate where the center point of the gradient will be located and then set an arbitrary number of colors and values for when the color should be applied relative to the center point as illustrated in the diagram found in Figure 10.4. We can move the center point both vertically and horizontally.

The values for color change can be either relative units (such as percentages) or fixed units (such as pixels). When no change value is passed, the browser will space the colors equally across the image (First example in Figure 10.4). If we do set a position at which the color should change and 2 subsequent values are set to change at the same distance from center, such as in the third example of Figure 10.4, the change will be immediate and abrupt. If the values are not identical the colors will fade from one to the other in the typical way in which we think of gradients such as in the second example of Figure 10.4.

