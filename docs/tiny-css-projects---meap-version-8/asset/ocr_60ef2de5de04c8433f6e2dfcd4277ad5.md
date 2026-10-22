# Listing 11.18 JavaScript for handling overflow

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