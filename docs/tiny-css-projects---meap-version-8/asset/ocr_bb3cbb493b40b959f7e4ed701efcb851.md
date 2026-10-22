# Listing 11.12 JavaScript File

```
(() => {
'use strict';

let expanded = false;
const container = document.getElementById('share');
const shareButton = document.getElementById('shareButton');
const menuItems = Array.from(container.querySelectorAll('li'));
const menu = container.querySelector('menu');

addButtonListeners();
addListListeners();
addTransitionListeners();

function addButtonListeners() {                          #A
  shareButton.addEventListener('click', toggleMenu);     #A
  shareButton.addEventListener('keyup', handleToggleButtonKeypress);  #A
}                                                         #A

function addListListeners() {                            #B
  menuItems.forEach(li => {                              #B
    const link = li.querySelector('a');                  #B
    link.addEventListener('keyup', handleMenuItemKeypress);  #B
    link.addEventListener('keydown', handleTab);         #B
    link.addEventListener('click', toggleMenu);          #B
  })                                                     #B
}                                                         #B

function addTransitionListeners() {                      #C
  menu.addEventListener('transitionstart', handleAnimationStart);  #C
  menu.addEventListener('transitionend', handleAnimationEnd);      #C
}                                                         #C

function handleToggleButtonKeypress(event) {             #D
  switch(event.key) {                                    #D
    case 'ArrowDown':                                    #D
      if (!expanded) { toggleMenu(); }                   #D
      moveToNext();                                      #D
      break;                                             #D
    case 'ArrowUp':                                      #D
      if (expanded) { toggleMenu(); }                    #D
      break;                                             #D
  }                                                      #D
}                                                         #D

function handleMenuItemKeypress(event) {                 #E
  switch(event.key) {                                    #E
    case 'ArrowDown':                                    #E
      moveToNext();                                      #E
      break;                                             #E
    case 'ArrowUp':                                      #E
      if (event.altKey === true) {                       #E
        navigate(event);                                  #E
        toggleMenu();                                     #E
      } else {                                           #E
        moveToPrevious();                                 #E
      }                                                  #E
      break;                                             #E
```