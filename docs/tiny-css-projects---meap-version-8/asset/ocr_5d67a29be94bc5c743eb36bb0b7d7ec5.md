# Explicit vs implicit grid

When we use either `grid-template-columns` or `grid-template-rows` we create an explicit grid, we are clearly stating to the browser the exact amount of columns and rows this grid should have. The implicit parts (both for the rows and columns) are those that are automatically created by the browser, this can happen when there are more child items than grid cells, the browser will then implicitly extra cells to. We can control implicit behavior through `grid-auto-flow`, `grid-auto-columns`, and `grid-auto-rows`.

![](bbox=[0.185, 0.22, 0.9, 0.463])

Figure 2.7 Firefox browser preview showing the grid lines and the associated numbers for each line

At this juncture, we have created a grid containing 3 columns. Two of those columns use `minmax()` and then our third column has a fixed value of 250px. This will allow us to have a three column layout. We will want to distribute the main content in the first 2 columns and use the 3rd one as an aside to put other less important content hence it being given less visual real estate (on most screens it will be narrower than the first 2 columns).

# 2.3 Grid template areas

If we want to explicitly set an element on a particular row and column of our grid, we have 2 different options. First, we can use the line numbers and dictate the position of the child as follows: `grid-column: 1 / 4`. This would place the element in the first column spanning through the second and third. Remember than grid is all based on lines, and the first line is the far left edge so the 4th line is the right edge of the grid. In our example, the element would be full width. To define the row we would use the same syntax as for columns with the `grid-row` property. To define that an element should start on the 3rd row and span 2 rows we would write: `grid-row: 3 / 5`.