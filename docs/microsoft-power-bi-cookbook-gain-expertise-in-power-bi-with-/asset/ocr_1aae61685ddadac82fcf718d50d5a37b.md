+   7. Select the **Table visualization**, and in the **Filters** pane, expand the **Internet Gross Sales** area in the **Filters on this visual** section.
+   
+   8. Select **is greater than** from the drop-down under **Show items when the value**, and in the field below the drop-down type `3000000`.
+   
+   9. Click **Apply filter** to apply the filter to the visual.

![](bbox=[0.2, 0.192, 0.866, 0.438])

Figure 4.10: Visual-level filter

# How it works

Report-level filters affect all pages in a report; thus, when filters are applied to the **Filters on all pages** area of the **Filters** pane, all visuals on all pages of the report respect this filter if a relationship to the dimension exists in the model and if DAX does not override the filter. For the report described in the previous recipe, it is determined that the European team does not need to view other sales groups or countries, and thus we filtered the entire report (all pages) for a **Sales Territory Group of Europe**. Similarly, page-level filters only affect the visuals on the page to which they are applied. Thus, we filtered each country page to the appropriate **Sales Territory Country**.

Report- and page-level filters are most commonly implemented at different levels of a dimension hierarchy that is relatively static and has few unique values, such as the example in this recipe. Date range filters should generally be applied either in the data retrieval queries or the report canvas, with visuals such as the date slicer. As these filters are applied to all visuals in the report or page, respectively, try to avoid filters against high-cardinality columns such as **ProductKey** or **Sales Order Number**. DAX measures cannot be used to define report- and page-level filters.

Visual-level filters can be based on both measures and columns from the data model. These measures and columns can be present in the visualization or not present.