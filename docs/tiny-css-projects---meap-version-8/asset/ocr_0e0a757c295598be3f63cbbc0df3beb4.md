We have used the CSS grid module to create a layout that is responsive depending on the browser width. Many aspects of the grid are still being developed and iterated on, most notably subgrids, which would allow for grids within grids. Although you can set a grid within a grid now subgrids have the benefit of being more closely related to their parent grid. To keep an eye out for future enhancements and development check out the grid specification at https://www.w3.org/TR/css-grid-2/

# 2.7 Summary

+   - A grid, in this sense, is a network of lines that cross each other to form a series of squares or rectangles.
+   - The `display` property with a value of `grid` allows us to place items on a grid layout.
+   - The `display` property is applied to the parent item that contains the child elements that are to be placed in the grid
+   - `grid-template-columns` and `grid-template-rows` are used to explicitly define the quantity and size of the columns and rows the grid should contain
+   - The flexible length (`fr`) unit is a unit of measurement that was formed as part of CSS grids as an alternative way to set the dimension of items
+   - We can use the `repeat()` function as a way to improve code efficiency where one or more rows or columns are the same size
+   - The `minmax()` function allows us to set two arguments, the minimum width a column should be and the maximum width a column should be.
+   - `grid-template-areas` allows us to define what each grid area is called, and then we can use the `grid-area` property on the child items to assign them to those named locations
+   - The `gap` property is a way to add spacing (create gutters) between grid cells
+   - Remember to make sure the source code and the visual experience are in the same logical order. When in doubt, use the browser developer tools to check the tabbing order