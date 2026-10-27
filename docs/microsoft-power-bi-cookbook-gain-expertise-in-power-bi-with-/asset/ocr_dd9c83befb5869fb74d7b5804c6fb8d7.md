21. Click the column expansion icon in the DimProductSubcategory column header, deselect all of the columns, select the Product Subcategory and ProductCategoryKey columns, and uncheck the Use original column name as prefix checkbox; click OK.

![](bbox=[0.2, 0.142, 0.868, 0.33])

Figure 5.40: Expand column

22. Click Merge queries in the ribbon of the Home tab.

23. Select DimProductCategory as the Right table for merge, select the two ProductCategoryKey columns and click OK to accept the default Left outer Join kind.

24. Click the column expansion icon in the DimProductCategory column header, deselect all of the columns, select the Product Category column, uncheck the Use original column name as prefix checkbox, and click OK.

25. Select the ProductCategoryKey column and while holding down the Ctrl key, select the ProductSubcategoryKey column.

26. Right-click the ProductSubcategoryKey column header and choose Remove columns.

27. Right-click the DimProductCategory query in the Queries pane and deselect Enable load.

28. Disable load on the DimProductSubcategory query as well.

29. In the lower-right corner, click the Save & Close button.

30. Name the dataflow Products and click the Save button.

![](bbox=[0.2, 0.66, 0.868, 0.782])

Figure 5.41: Save your dataflow