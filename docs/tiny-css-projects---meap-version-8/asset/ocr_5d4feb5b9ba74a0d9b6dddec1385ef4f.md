![](bbox=[0.22, 0.15, 0.88, 0.52])

Figure 2.4 Grid structure based on writing mode being English with the direction set to left-to-right

The grid-template-columns and the grid-template-rows properties can be used to lay out your grid.

DEFINITION These properties specify, as a space-separated track list, the line names and track sizing functions of the grid. The grid-template-columns property specifies the track list for the grid's columns, while grid-template-rows specifies the track list for the grid's rows.

Before we set our columns we first need to understand a few concepts that are specific to CSS Grids.

# 2.2.1 Repeating columns

To save repetition in your code you can use the repeat() function to specify how many columns you need.

The repeat() function needs two values that are comma separated:

+   1. the first value indicates how many columns to create
+   2. the second value is the sizing of each column