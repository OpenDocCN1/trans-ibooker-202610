The DAX queries created by Power BI reveal how the different layers of filters are implemented. Using DAX Studio, we can view the DAX variables used to store the report- and page-level filters (as well as slicer selections):

```
DEFINE
VAR __DS0FilterTable =
TREATAS({"Europe"}, 'Sales Territories'[Sales Territory Group])
```

Figure 4.11: DAX report-level filter variable in DAX Studio

Visual-level filters are also variables, but use the report- and page-level variables as inputs. The DAX queries created by Power BI take advantage of the latest functions and features, such as variables, VAR, and TREATAS.

# There's more...

There are more advanced filtering options than just Basic filtering. These include Advanced filtering and Top N filtering. Both text and numeric columns support AND conditions as well as OR conditions to create more complex filter conditions. To see how this works, follow these steps:

+   1. Select the Table visualization created on the Scratch page.
+   2. In the Filters pane, clear the visual-level filter from Internet Gross Sales.
+   3. Expand the Sales Territory Country area in the Filters on this visual section and switch the Filter type to Advanced filtering.
+   4. Select contains from the Show item when the value drop-down, type the letter a in the field below, and then click Apply filter. The United Kingdom disappears from the Table visualization.
+   5. Clear this filter and change the Filter type to Top N.
+   6. Under Show items, choose Top and type the number 2.
+   7. Drag the Reseller Net Sales measure from the Reseller Sales table to the By value area and click Apply filter. Germany now disappears from the table.

If complex filtering conditions are required, or if filters are needed against columns with many distinct values (for example, Sales Order Number), it may be beneficial to account for this logic in the data retrieval or model, thus simplifying the DAX queries generated.

Additionally, report-, page-, and visual-level filters can be overridden with DAX measures. To see how this works, create the following measure in the Sales Territories table:

```
Count Countries = COUNTROWS(ALL('Sales Territories'))
```