# 2

# Designing a layout using CSS grids

# This chapter covers

+   - Exploring grid tracks and how we can arrange our grid
+   - Using the minimax and repeat functions in CSS grids
+   - Working with the fractions unit and how its unique to CSS grids
+   - Creating template areas and placing the items on the areas
+   - Accessibility considerations when using grid
+   - Creating gutters between columns and rows within our grid

Now that we have a basic understanding of how CSS works, we can begin exploring our options for laying out HTML content, including grids, flexboxes and columns. First, we will focus on layout using grids.

A grid, in this sense, is a network of lines that cross each other to form a series of squares or rectangles. Now supported by all major browsers, grid has become a very popular layout technique.

Essentially a grid is made up of columns and rows, like a table. We first create our grid and then assign positions to our items, or elements, to that grid similarly to how we place boats on a grid when playing the game battleship.

Although sometimes compared to the concept of the table, grid layouts and tables have very different uses and fulfil very different needs. Grids are for layouts while tables are for tabular data. If the content being styled would be appropriate for an Excel sheet, then it's tabular data and should be placed in a table.

Historically we have used tables for layouts, and sometimes still need to (emails for example only support a small subset of CSS), but on the web it is considered bad practice. We can now use a grid instead.

CSS grid empowers us to be creative, and produce a range of layouts and adapt those layouts for different conditions in conjunction with media queries, as we will see. We will use