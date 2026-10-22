

the container. classList.toggle() will add the class if it's not currently present on the element and remove it if it already is.

## Listing 11.13 Opening and closing the menu (JS)

```
(() => {
  ...
  let expanded = false; #A
  const container = document.getElementById('share'); #B
  ...
  function addButtonListeners() {          #C
    shareButton.addEventListener('click', toggleMenu);   #C
    ...                                     #C
  }                                         #C

  function toggleMenu(event) {
    expanded = !expanded; #D
    ...
    container.classList.toggle('share_expanded'); #E
    ...
    ...
  }
}
```

- #A defines a variable to hold our current state
- #B defines a variable for our HTML container element
- #C defines what happens when the button is clicked
- #D toggles the expanded variable value
- #E handles adding and removing the share_expanded

NOTE JS IS ALREADY IN THE STARTER CODE Since this is a CSS book, the JS has already been included in the starter code. If you are following along, you don't need to make any edits to the JS, it should just work.

All put together this code makes it so that when the button is clicked we add the share_expanded class to the container. If it is already open we remove it. We had hidden our menu items, we will now show them when the share_expanded class is present.

NOTE CLASSE NAME SHARE_EXPANDED Remember that we decided we were going to use BEM for our class name convention. Notice that our class name only has one underscore. This is because expanded is our modifier. We use a modifier because we are changing (modifying) the style based on the state (open / close). We have the block (share) and the modifier (expanded); therefore our class name is block_modifier or share_expanded.

To hide our menu we used the width property with a value of 0 and hide the overflow (Listing 11.14).

## Listing 11.14 Hiding the menu

```
.share__menu {
  display: flex;
  margin: 0;
  padding: 0;
  background: inherit;
  border-radius: inherit;
  width: 0;          #A
  overflow: hidden;  #B
}
```

#A makes the width of the menu equal to 0
#B hides the overflow so that the links within are also hidden

To show the links when the component is marked as expanded we must increase our width of the menu as seen in Listing 11.15. We also add a little bit of horizontal padding to add some room round the menu.

To calculate the width of the menu we take the number of links and multiply by their width. The link’s width is 48 pixels (that we hard set) plus the border (1 pixel on each side). Therefore the menu’s width is width = 4×(48 + 2) = 200px.

## Listing 11.15 Showing the menu

```
.share_expanded .share__menu {
  width: 200px;
  padding: 0 2rem 0 1rem;
}
```

Running the code, after clicking the button and hovering over the link we can see that our link no longer expands outside of our menu (Figure 11.14).

![](img/b56145aa9b751926a7aedbd7172bc818_323_0.png)

![](img/b56145aa9b751926a7aedbd7172bc818_323_1.png)

Figure 11.14: Expanded component on click

The reason we no longer expand outside of the background like we did initially is because we added overflow: hidden to our menu in order to hide the links when the menu was given a width of 0. We must therefore set the overflow back to visible once we are expanded. Listing 11.16 shows our updated rule.

## Listing 11.16 Showing the menu

```
.share_expanded .share__menu {
  width: 200px;
  padding: 0 2rem 0 1rem;
  overflow: visible;
}
```

With the overflow reinstated, our magnification effect is restored (Figure 11.15). The next piece is to maintain the button outline that is usually present on hover, when the component is open.

![](img/b56145aa9b751926a7aedbd7172bc818_324_0.png)

# Figure 11.15: Open component with the overflow reinstated

Since we already have a rule written on hover and focus to add the border, we are going to append the rule to also trigger when the component is open. By reusing the same rule we ensure that the styles will be consistent when being hovered, focused, or when the list is visible. To add the condition, we add the .share_expanded .share__button selector to the existing rule (Listing 11.17).

## Listing 11.17 Adding the button border to share button when list is showing

```
.share__button:hover,
.share__button:focus-visible,
.share_expanded .share__button {
  cursor: pointer;
  outline: solid 1px black;
  outline-offset: -5px;
}
```

With the selector added, our button now keeps its border once the component is expanded (Figure 11.16) and when closed and not focused or hovered, we see that the border stays absent.

![](img/b56145aa9b751926a7aedbd7172bc818_324_1.png)

# Figure 11.16: Maintaining the share button border when list is showing

## 11.6.3 Animating the menu

Now that we have our styles set for both open and closed, let’s animate the showing and hiding of the menu. We want the link list to expand from the left as depicted in Figure 11.17.

![](img/b56145aa9b751926a7aedbd7172bc818_325_0.png)

Figure 11.17: Breakdown of opening animation

When the menu closes we will want to perform the inverse of the opening animation, retracting the menu hiding the link as we go.

Same as for the magnification effect we added to the links, we are going to use a transition. We don’t need to use keyframes because the animation is only going to be performed once when the button is clicked, and we already have the 2 states defined.

We will add the transition declaration to the menu as follows: `transition: width 250ms ease-in-out`. Again we want to keep the transition snappy so we give it a 250 millisecond duration.

Once we add the transition we realize that icons are becoming visible before they should. Figure 11.18 breaks down the effect.

![](img/b56145aa9b751926a7aedbd7172bc818_326_0.png)

Figure 11.18: Icons displaying too soon

Even if we change the transition to transition all properties instead of just width, the same issue occurs. The root cause is overflow. When the menu is closed, we want the menu’s overflow to be hidden. When it is open, we want it to be visible. However, overflow is not something that can be gradually changed like width. It is either visible or it’s not—there is no in between.

When opening the menu, we want to wait until the transition is done before we change overflow to visible. When we close, we want the overflow to be hidden immediately. This is where we turn to JavaScript to support our CSS.

We will remove overflow: visible from our .share_expanded .share__menu class and handle adding it via JavaScript.

Listing 11.18 singles out the relevant JavaScript for handling the overflow. The magic lies in the transitionstart and transitionend event listeners. Attached to the menu, they listen for when the transition is triggered and when it is done performing the change. Upon the event happening, they trigger their respective functions which handle the overflow for the menu.

## Listing 11.18 JavaScript for handling overflow

```
(() => {
'use strict';

let expanded = false;
const container = document.getElementById('share');
const menu = container.querySelector('menu');
...
addTransitionListeners();
...
function addTransitionListeners() {
  menu.addEventListener('transitionstart', handleAnimationStart);
  menu.addEventListener('transitionend', handleAnimationEnd);
}
...
function handleAnimationStart() { #A
  if (!expanded) { menu.style.overflow = 'hidden'; } #B
}
...
function handleAnimationEnd() { #C
  if (expanded) { menu.style.overflow = 'visible'; } #D
}
})()
```

#A Is triggered when the transition starts
#B If in the process of closing, we hide the menu's overflow
#C Is triggered when the transition ends
#D If just opened, we show the overflow

NOTE JS IS ALREADY IN THE STARTER CODE Same as earlier in the chapter, since this is a CSS book, the JS has already been included in the starter code. If you are following along, you don't need to make any edits to the JS, it should just work.

All put together the CSS needed to make the animation work is found in Listing 11.19:

