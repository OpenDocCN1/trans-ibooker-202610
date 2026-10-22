# Listing 5.16 Showing content on hover and on focus-within

```
@media (hover: hover) and (min-width: 700px) and (prefers-reduced-motion: no-preference) {
  main > section > div {
    transform: translateY(calc(350px - 8rem));
    height: 5rem;
    overflow: hidden;
    transition: all 700ms ease-in-out; #E
  }

  div > *:not(h2) {
    opacity: 0;
    transform: translateY(1rem);
    transition: all 700ms ease-in-out; #E
  }

  section:hover div,      #A
  section:focus-within div {      #B
    transform: translateY(0);
    height: calc(350px - 4rem);
  }

  section:hover div > *:not(h2),      #C
  section:focus-within div > *:not(h2){      #D
    opacity: 1;
    transform: translateY(0);
  }
}
```

#A on section hover move container back into place

#B on section focus-within move container back into place

#C on hover move all non h2 elements inside the container back into place with full opacity

#D on section focus-within move all non h2 elements inside the container back into place with full opacity

#E Animating the changes

With these changes applied (Figure 5.20) all that is left is to set the background on our page and our project will be complete.