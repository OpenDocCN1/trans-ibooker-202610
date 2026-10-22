# align-items

container

children

# Stretch (default)

# flex-start

# flex-end

# baseline

# center

Figure 6.7 align-items property values

Listing 6.5 shows our updated body rule. Remember the parent of the element being positioned is the one on which we apply flexbox related declarations.

# Listing 6.4 Centering the card

```
body {
    ...
    display: flex;
    justify-content: center; #A
    align-items: center; #B
}
```

#A centers the card horizontally

#B centers the card vertically

Now that our card is centered (Figure 6.8), let’s focus on the contents of the card starting with the profile picture.