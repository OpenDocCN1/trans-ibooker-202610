If we were fair we could say that our cake can be divided into 4 equal slices. In our CSS this would be the same as grid-template-columns: 1fr 1fr 1fr 1fr. We are telling the browser to give each HTML element an equal slice of the whole thing.

But what if we decided to be sneaky and have a larger size? After all, we baked the cake, right?

So we decide to take half of the cake for ourselves, and then divide the remaining half into three slices. To do this we need to have a total of 6 fractions, 3 fractions for our 50% of the cake and then one fraction three times to equally split the other 50% of the cake.

So in our CSS that would be grid-template-columns: 3fr 1fr 1fr 1fr, so we are saying there are 6 fractions in total, the first column gets 3 of them (or 50% of the total) and then the remaining 50% should be equally split between the other 3 columns.

![](bbox=[0.2, 0.36, 0.88, 0.52])

Figure 2.6 Diagram showing fractions value

For our project we will create our grid lines for the columns by adding the following to our main rule:

# Listing 2.4 Setting the amount of columns

```
main {
  display: grid;
  grid-template-columns: repeat(2, minmax(auto, 2fr)) 250px
}
```

When previewed in the browser (Figure 2.7) we can see that now our grid has numbers set across each line. What we can do with this information is explicitly choose where to place our HTML elements within the grid based on the grid line number.

We also notice that the browser has assumed that we want to place our HTML elements within each grid cell so rather than each element being stacked vertically the browser fills up each column cell until it runs out and then creates a new row and fills in those columns. This is also known as the implicit grid.