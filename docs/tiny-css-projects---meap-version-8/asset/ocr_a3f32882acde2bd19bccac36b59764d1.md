# 6.6.4 Flex-direction

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