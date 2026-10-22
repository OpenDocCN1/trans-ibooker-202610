
## A Parent Item (.list)
## B Children of .list
## C Grand child of .list
## D Great grandchildren of .list

We are going to style only the list items (the children) in a crimson color, without affecting the nested list items (the great grandchildren). So the browser will find the `list` class and target just the child list items (`li`) directly under the `list` class and change the `color` to `crimson`. We will use the following CSS: `.list > li { color: crimson; }`

As we can see in Figure 1.17, the list items are now in crimson while the nested list items are still black. This is one of the benefits of using the child combinator. It allows us to style just the child elements but not the grandchildren or anything further down the hierarchy.

![](img/b56145aa9b751926a7aedbd7172bc818_30_0.png)

Figure 1.17 Child combinator applied to list items

*So we can do this in reverse and select the parent of the child element, right?* The short answer is no. There is currently no way to select the parent item in CSS, so the following example would not work: `article < p { color: blue; }`.

## 3. ADJACENT SIBLING COMBINATOR (+)

When we need to style an element that is at the same level as another (like how your brother or sister is on the same level of the family tree as you) we can use the adjacent sibling combinator.

If we want to target the element that is directly after another we can use the adjacent sibling selector.

In this example (Listing 1.7), the browser will find any uses of the `header` tag and then target the first paragraph (`p`) immediately after (or adjacent) to the `header` tag and change the `font-size` to `1.5rem` and the `font-weight` to `bold`.

```
Listing 1.8 Adjacent sibling combinator

header + p {
  font-size: 1.25rem;
  font-weight: bold;
}
```

Figure 1.18 shows the code applied to our article.

# Title of our article (heading 1)

Posted on May 16 by Lisa.

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Pellentesque tincidunt dapibus eleifend. Nam eu urna ipsum. Etiam consequat ac dolor et dapibus. Duis eros arcu, interdum eu volutpat ac, lacinia a tortor. Vivamus justo tortor, porttitor in arcu nec, pretium viverra ipsum. Nam sit amet nibh magna. Sed ut imperdiet orci, id finibus justo. Maecenas magna mauris, tempor nec tempor id, aliquam et nibh. Nunc elementum ut purus id eleifend. Phasellus pulvinar dui orci, sed eleifend magna ullamcorper sit amet. Proin iaculis lacus congue aliquam sodales.

- 1. List item 1
  - Nested item 1
  - Nested item 2
- 2. List item 2
- 3. List item 3
- 4. List item 4

![](img/b56145aa9b751926a7aedbd7172bc818_31_0.png)

Curabitur id augue nulla. Aliquam purus urna, aliquam eu ornare id, maximus et tellus. Aliquam eleifend sem vitae una blandit, non bibendum tellus dignissim. Aliquam imperdiet imperdiet sapien sit amet consectetur. Nam convallis turpis felis, sed vulputate lacus eleifend a. Mauris pharetra imperdiet lacinia. Sed sit amet feugiat lectus, in consectetur magna. Vestibulum accumsan porta enim ut ultricies. Vestibulum vitae massa quis massa dignissim imperdiet.

> Nunc eleifend nulla lobortis porta rhoncus. Vivamus feugiat, sem vitae feugiat aliquam, orci nulla venenatis libero, vitae rhoncus nibh neque ac velit.

Etiam tempor vulputate varius. Duis at metus ut eros ultrices facilisis. Donec ut est finibus, egestas nisl eu, placerat neque. Pellentesque cursus, turpis nec sollicitudin sodales, nisi

Figure 1.18 Adjacent sibling combinator applied to paragraph immediately after header

This could be useful if we are trying to style the first element differently to any others to make it stand out. We might see this when looking at a newspaper. The first paragraph might be made to look more prominent than the rest to catch our attention. Another use case is for error handling in forms. Adjacent sibling combinations allow us to display an error message to the user immediately following an invalid value in a form control.

It is worth noting that similar to the child combinator, we cannot find a previous sibling. `header + p { ... }` to find a paragraph immediately before the header will not work.

## 4. GENERAL SIBLING COMBINATOR (~)

The general sibling combinator is more open-ended than the previous methods, as it allows us to target all elements that are siblings after the element targeted by the selector.

In our example, we will style all images that come after the element `header`. We will notice that we have three placeholder images. The first one is smaller and could be a logo or an author photo and resides above the `header`. We do not want to style it. The other two images are further down in the article. We want to apply a border around them to keep the color theme consistent with the rest of the article.

Our rule will look as follows: `header ~ img { border: 4px solid crimson; }`. The browser will find the `header` element and target all the sibling images (`img`) after that

element and add a border that's 4px in thickness, a solid line (as opposed to a dotted, dashed, or double line) and is colored crimson.
We can see the code applied to our article in Figure 1.19.

![](img/b56145aa9b751926a7aedbd7172bc818_32_0.png)

Figure 1.19 General Sibling Combinator targeting sibling images of header

## 1.6.3 Pseudo-class and pseudo-element selectors

### PSEUDO-CLASS

A pseudo-class is added to a selector to target a specific state of the element. They are especially useful for elements the user will interact with such as links, buttons, and form fields. Pseudo classes use a single colon (:) followed by the state of the element. Within our article there are a few links. We have not styled them in any way, their styles therefore come from the user agent stylesheet. For most browsers, they have an underline and will vary in color based upon whether the link has been previously visited (the url is found in the browser's history).

With links, there are a few states to consider. The most common are:

- link: an anchor tag that contains an href attribute and a url that is not found in the user's browser history
- visited: an anchor tag that contains an href, and a url that is found in the user's browser history
- hover: the user has their cursor over the link, but has not pressed it
- `active`: the user is clicking the link and holding the click down
- `focus`: a focused element is an element that will receive keyboard events by default. When a user clicks on an element it automatically gains focus (unless there is JavaScript altering this behavior). Using the keyboard to navigate between form fields, links, and buttons will also change the element that is in focus.
- `focus-within`: is applied to a parent element. If a child of the parent has focus, focus-within styles will be applied.
- `focus-visible`: applies only when the focus has been gained via keyboard navigation or the user is interacting with the element via the keyboard.

There is also the `:target` pseudo-class which we will explore later in this book when we look at `text-fragments`.

In our current article project, we will first create an `a:link` rule to changing the color of anchor tags that contain and href element and have not been visited to a light blue using its Hex color code (#1D70B8).

The `:visited` state should be different from the `:link` state because it should indicate to the user that they have visited that page before. Often we see websites not differing between the two states however, differentiating the two can provide a better experience.

In our example, we will change the `:visited` state to a purple color using the Hex code value #4C2C92.

We will then handle the `:hover` state. The hover state does not apply to mobile users as there is no way to recognize a user hovering over a link. In our article, we will change the `:hover` state text color to a dark blue using the hex code value #003078.

Finally we will handle the `:focus` state. The focus state can be used on links, buttons, and form inputs. The focus state is shown when the user clicks or taps on an element. When the element is focused we will add a 1 pixel crimson outline to the element.

All put together our link rules are as follows (Listing 1.8):

## Listing 1.9 Styling links using pseudo elements

```
a:link {
  color: #1D70B8;
}
a:visited {
  color: #4C2C92
}
a:hover {
  color: #003078;
}
a:focus {
  outline: solid 1px crimson;
}
```

We can view the different element states by going into our browser, right clicking and selecting "inspect". Then we will typically have a view of the HTML with the CSS on the side. By clicking the `:hov` button in the Styles section we can see a panel which may say something like "force element state" and then we can toggle different pseudo classes on and off. Figure 1.20 shows the Chrome developer tools with the :hov panel open.
