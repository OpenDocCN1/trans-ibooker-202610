Rather than dealing with numbers, we can instead use named areas to be referenced when explicitly placing elements on the grid. To do this we use the `grid-template-areas` property. This allows us to define the guide for how we want the web page to be laid out.

The `grid-template-areas` property takes multiple strings, each composed of the names of the areas they describe. Each string represents a row in the layout as seen in Figure 2.8. Each name represents a column within the row. If 2 adjacent cells have the same name (horizontally or vertically) the two cells are treated as one area.

```
grid-template-areas:
"header  header  header"
"content content author"
"content content aside "
"plays   .       aside "
"footer  footer  footer";
```

![](bbox=[0.5, 0.27, 0.88, 0.52])

Figure 2.8 Syntax of the `grid-template-areas` property

The benefit of named areas is in the visualization of the final outcome. We will define our `grid-template-areas` as follows (Listing 2.6). Notice the dot (.) in the 4th row. The dot is used to define a cell that we intend to keep empty. Since it does not have a name, content cannot be assigned to it.

# Listing 2.5 Creating our template areas

```
main {
  display: grid;
  grid-template-columns: repeat(2, minmax(auto, 2fr)) 250px;
  grid-template-areas:
  "header header header"
  "content content author"
  "content content aside "
  "plays   .       aside "
  "footer  footer  footer";
}
```

Although we have defined areas, the content is still implicitly positioning itself in each available cell, ignoring the areas we have defined (Figure 2.9). We now need to assign our content to each of these areas.