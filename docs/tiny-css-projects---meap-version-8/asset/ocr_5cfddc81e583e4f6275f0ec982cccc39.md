# Listing 5.15 Hiding the non header content

```
@media (hover: hover) and (min-width: 700px) and (prefers-reduced-motion: no-preference) {
    #A
    main > section > div {
        transform: translateY(calc(350px - 8rem));
        height: 5rem;
        overflow: hidden;
    }
    main > section > div > *:not(h2) {
        opacity: 0;
        transform: translateY(1rem);
    }
}
```

#A The media query
#B Moving and shortening the inner content container
#C Hiding all of the non H2 content

The not() pseudo-class allows us to filter selectors. In this case, we want to target anything that is not an h2. Figure 5.18 diagrams the process.

![](bbox=[0.25, 0.48, 0.85, 0.8])

# Figure 5.18 Selecting anything that is not an h2 inside of the inner container

Now that the content is hidden (Figure 5.19) we can focus on showing it again.