
## 6
## Creating a profile card
## This chapter covers
- CSS custom properties
- Creating a background using radial-gradient
- Setting image size
- Positioning elements using flexbox

In this chapter we will create a profile card. In web designs, cards are a visual element that contains information on a single topic. We are going to apply this concept to someone’s profile information, essentially creating a digital business card. This type of layout can often be seen on social media or blog sites to give readers a quick overview of who authored the content. It will sometimes have links to a detailed profile page or opportunities to interact with the person the profile belongs to.

By the end of the chapter, our profile card will look as follows (Figure 6.1):

![](img/b56145aa9b751926a7aedbd7172bc818_158_0.png)

Figure 6.1 Final Output

To create this layout, we will do a lot of work revolving around positioning, specifically leveraging flexbox to help us align and center elements. We will also look at how to make a rectangular image fit into a circle without distorting the image.

Let’s dive right in and take a look at our starting HTML (Listing 6.1) which can be found in the github repo at https://github.com/michaelgearon/Tiny-CSS-Projects/tree/main/chapter-06.

We have a div with a class of card which contains all of the elements being presented in the profile card. To set our blog post information we use a description list.

> **Description list**
> A description list contains groups of terms including a description term (dt) and any number of descriptions (dd). Description lists are often used for glossaries or to display metadata. Since we are pairing terms (posts, likes, and followers) with their counts (the number), this a great use case for using a description list.

Our technologies (CSS, HTML...) are presented in a list.

### Listing 6.1 Project HTML

```
<body>
  <div class="card">  #A
    <img class="portrait" src="./img/portrait.jpg" alt=""> #B
    <h1>Annabelle Erickson</h1> #C
    <div class="title">Software Developer</div> #D
    <dl>          #E
      <div>        #E
        <dt>Posts</dt>      #E
        <dd>856</dd>       #E
      </div>        #E
      <div>        #E
        <dt>Likes</dt>      #E
        <dd>1358</dd>       #E
      </div>        #E
      <div>        #E
        <dt>Followers</dt>  #E
        <dd>1257</dd>       #E
      </div>        #E
    </dl>          #E
    <p class="summary">      #F
      I specialize in UX / UI design and development. I love coffee, chocolate, and my
      betta fish. #F
    </p>      #F
    <ul class="technologies">    #G
      <li>CSS</li>          #G
      <li>HTML</li>         #G
      <li>JavaScript</li>   #G
      <li>Accessibility</li> #G
    </ul>          #G
    <div class="actions">      #H
      <button type="button" class="follow">Follow</button>  #H
      <a href="" class="message">Message</a>          #H
    </div>          #H
  </div> #I
</body>
```

#A Start of the card
#B Profile Image
#C Profile holder's name
#D Profile holder's job title
#E Post information
#F Personal summary / about
#G Technologies
#H Actions

As we begin styling our card, our page looks as follows (Figure 6.2).

![](img/b56145aa9b751926a7aedbd7172bc818_160_0.png)

Figure 6.2 Starting point

### 6.1 CSS custom properties

In our layout, specifically when we style the profile image and colored portion at the top of the card under the image, we are going to need to use the image size value for a number of calculations. In other languages like JavaScript, when we have a value we are going to be referencing multiple times we use variables. In CSS we have custom properties also sometimes referred to as CSS variables.

To create a custom property we prefix the variable name with 2 dashes (--) immediately followed by the variable name. We assign the value in the same way we do any other property with a colon (:) followed by the value. A CSS variable declaration would therefore looks as follows: `--myVariableName: myValue;`

Like any other declaration, we need to define our variables inside of a rule. For our project we are going to define our colors and image size and declare them inside of a body rule as seen in listing 6.2. Since we define our variables on the body, the body element and any its descendants will have access to the variables.

Notice that we can assign different types of values to our variables. We assign colors, such as in our --primary variable (probably one of the most common uses for CSS custom properties), but we also define a size (--imageSize), a font family, and a gradient (--page-background).

To reference the variable and use it as part of a declaration, we use the following syntax: `var(--variableName)`, therefore to assign our text color for example would declare: `color: var(--text-color);`

### Listing 6.2 Defining CSS custom properties

```
body {
  --primary: #de3c4b; #A
  --primary-contrast: white;
  --secondary: #717777; #B
  --font: Helvetica, Arial, sans-serif;
  --text-color: #2D3142; #C
  --card-background: white;
  --page-background: linear-gradient(#4F5D75, #2D3142);
  --imageSize: 200px;

  background: var(--page-background);
  font-family: var(--font);
  color: var(--text-color);
}
```

#A dark salmon
#B gray
#C very dark blue-gray

NOTE: LINEAR GRADIENT Our linear gradient will go from top to bottom fading from a dark blue to very dark blue. For a more in depth explanation of linear gradients checkout Chapter 3 Section 3.7.2.

With our background and font color and family applied (Figure 6.3) we notice that our background repeats at the bottom of the page.

![](img/b56145aa9b751926a7aedbd7172bc818_161_0.png)

Figure 6.3 Adding the background to the body

### 6.2 Full height backgrounds

A linear gradient is a type of image. When we apply an image as a background to an element in CSS, if the image is smaller than the element, the image will repeat or tile. In this particular case we don’t want the image to repeat.

We have 2 ways we can fix this:

- We can tell the background we don’t want it to repeat using `background-repeat: no-repeat;` However since our body element is only as tall as its contents, if the window is taller than the content we will be left with an unsightly white bar at the bottom of the page which is not ideal.
- Our second option (the one we will use) is to make both the html and body take the full height of the screen rather than have it size itself to contents within.

We therefore add the rule found in Listing 6.3 to our stylesheet.

We reset to margin and padding to 0 because we want to ensure we go edge to edge inside of the window.

To set the height, we actually use `min-height` because should the content length be greater than the height of the window, we still want the user to have access to the content and for the background to be behind it. By using `min-height` we instruct the browser to make the `body` and `html` elements at a minimum the height of the window. If the contents would force the elements to be taller, then the browser will use the height of the content.

The value we set for `min-height` is `100vh`. Viewport height (vh) is a unit that is based on the height of the viewport itself. It is percentage based so assigning a value of `100vh` to `min-height` means we want the element to have, at minimum, a height equal to 100% of the viewport height.

### Listing 6.3 Making the background full height

```
html, body {
  margin: 0;
  padding: 0;
  min-height: 100vh;
}
```

Now that we have our background set (Figure 6.4), let’s style the card.

![](img/b56145aa9b751926a7aedbd7172bc818_163_0.png)

Figure 6.4 Full screen gradient background

### 6.3 Styling and centering the card using flexbox

Let’s start with styling the card itself. We will give it a white background and shadow to give our layout some depth. Notice that instead of using the color value for the background, we use our background variable.

We are also going to set the width of the card to 75vw. Viewport Width (vw) is the horizontal counterpart to the viewport height (vh) unit we used earlier. It is also percentage based, so by setting our width to 75vw we are setting the width of the card to be 75% of the total width of the browser window.

We are then going to further constrain the width of the card to a maximum of 500 pixels wide. By using both the width and max-width properties, we allow for the card to shrink when the screen size is narrow but constrain the card from becoming too wide and unruly on larger screens.

Lastly we curve the corners of the card using border radius to soften the design. Listing 6.4 shows our card rule.

### Listing 6.4 Styling the card

```
.card {
  background-color: var(--card-background);
  box-shadow: 0px 0px 55px rgba(38, 40, 45, .75);
  width: 75vw;
  max-width: 500px;
  border-radius: 4px;
}
```
