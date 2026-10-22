# Listing 11.11 Hiding the menu

```
.share_menu {
  display: flex;
  margin: 0;
  padding: 0;
  background: inherit;
  border-radius: inherit;
  width: 0;
  overflow: hidden;
}
```

With our menu hidden (Figure 11.13) we now need to toggle exposing and hiding the menu when the button is clicked.

![](bbox=[0.2, 0.36, 0.32, 0.4])

# Figure 11.13: Hidden menu

Our JavaScript handles part of the behavior for us. At the beginning of this chapter we mentioned we were going to need some JavaScript for this project (Listing 11.12). When we open the file we notice that there is a lot of code in there.