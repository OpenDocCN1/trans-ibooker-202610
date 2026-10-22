
## 6.6.4 Flex-direction

In our original design, we have the description details (the numbers) above the description terms. To flip them visually we are going to use the `flex-direction` property. We asserted that flexbox can place elements across a singular axis. So far we have done our work across the horizontal or x axis.

To move the details above the terms we are going to use flexbox on the vertical or y axis. To change which axis we want flexbox to operate on we use the `flex-direction` property. By default it has a value of `row`, which makes flexbox operate on the x axis. By changing the value to `column`, we make it operate on the y axis.

Furthermore, the `flex-direction` property also allows us to dictate how the elements should be ordered. Setting the value to `column-reverse` tells the browser that we want to operate on the y axis, and we want the elements to be placed in reverse HTML order, making the description details (`dd`) appear first and the description term (`dt`) second.

As before, we want to set the behavior on the parent, in this case the `div`. We will therefore add to our previously created `div` rule as seen in Listing 6.14 to reorder the elements. We also decrease the size of the description term (`dt`) to emphasize the number over its term.

```
Listing 6.14 Reversing content order

dl > div {
  flex-basis: 33%;
  flex-shrink: 1;
  display: flex;
  flex-direction: column-reverse;
}
dt { font-size: .75rem; }
```

> NOTE: CONTENT ORDER For accessibility reasons we want to make sure that the order in which our HTML is written follows the order in which it is displayed on the screen. A user who has their computer read the contents of the page to them as they are visually following along would be easily disoriented or confused if the content that is being read to them does not match what they are looking at. Caution must therefore be used when using properties such as flex-direction to reorder content.

Figure 6.24 shows our styled description list (`dl`).

Figure 6.24 Styled description List

Continuing down the card, let’s turn our attention to the summary paragraph below the profile stats.

## 6.6.5 Styling the paragraph

The paragraph already looks pretty good. The only thing we are going to do to it is add some vertical margin for breathing room and increase the line height for better legibility as seen in listing 6.15.

Notice that the line height does not take a unit. By not setting a unit we allow the line height to scale with the font size. If we had set it to a 12 pixel value for example, the line height would remain 12 pixels no matter the font size. So if the font size was radically increased, our letters would vertically overlap. It is always safest not to declare a unit.

### Listing 6.15 Paragraph rule

```
p.summary {
  margin: 24px 0;
  line-height: 1.5;
}
```

With our paragraph taken care of (Figure 6.25), let's style the list of technologies.

Figure 6.25 Styled summary paragraph

## 6.6.6 Flex-wrap

The first thing we are going to do is style the list elements themselves. We will use a design pattern sometimes referred to as a pill or chip where the element has a background color and rounded edges. Our CSS will look as seen in listing 6.16. We also include some padding in order for the text not to come right up against the edge of the chip.

### Listing 6.16 Styling the list elements

```
ul.technologies li {
  padding: 12px 24px;
  border-radius: 24px;
  background: #ffdadd;
}
```

With the individual elements styled (Figure 6.26) we can now focus on the list’s layout.

![](img/b56145aa9b751926a7aedbd7172bc818_183_0.png)

Figure 6.26 Styled List Items

The first thing we are going to do is remove the bullets using list-style: none. We will then remove all padding and set the margins to 24px vertically and 0 horizontally.

To position the items we will use flexbox, add a gap of 12px and set the justify-content property value to space-between. space-between works very similarly to space-around except that it does not add space to the beginning and end of the container as seen in Figure 6.27.

![](img/b56145aa9b751926a7aedbd7172bc818_183_1.png)

Figure 6.27 space-around versus space-between

Our rule to layout our chips will looks as follows (Listing 6.17):

### Listing 6.17 Styling the list of technologies

```
ul.technologies {
  list-style: none;
  padding: 0;
  margin: 24px 0;
  display: flex;
  justify-content: space-between;
  gap: 12px;
}
```

However, we notice that when we reduce the screen width (Figure 6.28), our last chip extends beyond our card.

![](img/b56145aa9b751926a7aedbd7172bc818_184_0.png)

Figure 6.28 Chip extending beyond width of the card

On narrow screens our list is wider than our card. To prevent the content from overflowing the card, we can use the flex-wrap property.

By default, flexed items will display in a straight line even if the container is too small as we are experiencing with our list of technologies. To force the last element onto a new line when we run out of room, we can set the flex-wrap property to wrap. This will tell the browser to start a new line of items below when it runs out of room.

Like flex-direction, flex-wrap can also change the order in which the elements are displayed, but we won’t need to change that here. Our updated rule is shown in Listing 6.18.

### Listing 6.18 Adding flex-wrap

```
ul.technologies {
  list-style: none;
  padding: 0;
  margin: 24px 0;
  display: flex;
  justify-content: space-between;
  gap: 12px;
  flex-wrap: wrap;
}
```

Notice the gap between the CSS and Accessibility chips in Figure 6.29 even though our list element does not have any margin. Our list has a gap property value of 12px, this means that not only will it ensure we have a minimum of 12 pixels horizontally between our items, when we wrap, it will also add a 12 pixel gap between them vertically.

![](img/b56145aa9b751926a7aedbd7172bc818_185_0.png)

Figure 6.29 Wrapping the chips on narrow screens

## 6.6.7 Styling the actions

The last thing we need to style in our profile card are the 2 actions the user can take at the bottom of the card: to message or to follow the profile owner.

Even though they are semantically different, one is a link and the other is a button, we are going to style both of them to look like buttons. Let’s start with some basics that will apply to both elements. We create one rule with selectors for both elements in order to ensure that both element types are visually consistent. We then create individual rules for where they diverge.

We also create a focus-visible rule which will be applied to all elements using the universal selector (*) and the pseudo-class :focus-visible so that when a user navigates to our links and buttons via keyboard, they are presented with a dotted outline around the element and can clearly see what they are about to select.

### Listing 6.18 Adding flex-wrap

```
.actions a, .actions button {
  padding: 12px 24px;
  border-radius: 4px;
  text-decoration: none; #B
  border: solid 1px var(--primary);
  font-size: 1rem;
  cursor: pointer;
}

.follow {
  background: var(--primary);
  color: var(--primary-contrast);
}

.message {
  background: var(--primary-contrast);
  color: var(--primary);
}

*:focus-visible {
  outline: dotted 1px var(--primary);
  outline-offset: 3px;
}
```

#A applies to both the link and the button
#B removes the underline

Figure 6.30 shows our styled link and button. However, as they are quite close together, we are going to want to add some space between the 2 buttons.

![](img/b56145aa9b751926a7aedbd7172bc818_186_0.png)

Figure 6.30 Styled actions

We want to keep the link and button centered but add some space between them. Let’s use flex and gap one last time to position our action elements.

We are going to give the list a display property value of flex and add a gap of 16px. To keep the 2 elements centered, we will use the justify-content property with a value of center. Finally, we will add some space between the list of technologies and our actions by giving the list a margin-top value of 36 pixels as seen in Listing 6.19.
