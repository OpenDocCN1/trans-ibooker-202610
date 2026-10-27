+   6. Repeat steps 3, 4, and 5 for the **France**, **Germany**, and **United Kingdom** pages.
+   7. Select any visual and click **Edit interactions** in the ribbon of the **Format** tab to toggle off editing interactions.
+   8. Save your work.

# How it works

In this example, it was determined that the KPI visualizations should always show the last full month's statistics for all products and all countries, states/provinces, and cities. To ensure this is the case, interactivity with all other visualizations on the pages is set to **None** instead of the default **Filter**, using the **Edit interactions** feature on Power BI Desktop.

It is important to realize that DAX measures can override cross-filtering behavior by using filter functions such as ALL, ALL EXCEPT, and REMOVEFILTERS. In fact, there actually was no need to set the interaction between the Calendar Month slicer and the KPI visuals to **None** because the DAX measures that drive the KPI visuals include the use of the ALL function for the **Dates** table as part of their calculations.

# There's more...

The cross-filtering interactions between visualizations have the options of **Filter**, **Highlight**, and **None**—some visualizations, such as KPI visualizations, only include **Filter** and **None** options, whereas other visualizations, such as bar and column charts, include a **Highlight** option as well. To see the **Highlight** option, do the following:

+   1. From the **Visualizations** pane, create a **Clustered column chart**.
+   2. From the **Fields** list, add the **Year Month** column from the **Dates** table as the Axis, and add the **Internet Net Sales** and **Internet Plan** measures from the **Internet Sales** table as **Values**.
+   3. In the **Format** pane, expand **Legend** and change the **Position** to **Bottom**, turn off the **Title** on the **X axis**, turn the **Y axis** completely off, expand **Data labels**, and change the **Display units** to **Thousands**, the **Value decimal places** to **0**, and **Orientation** to **Horizontal**; expand the **Title** section and change the **Title text** to **Internet Sales to Plan** with an **Alignment** of **Center**, and turn the **Background** to **Off**.
+   4. In the **General** section, set the **X Position** to **449**, **Y Position** to **47**, **Width** to **831**, and **Height** to **241**.
+   5. Create a **Stacked bar chart** with the **Sales Territory Country** column from the **Sales Territories** table as the **Axis**, and in the **Values** field well, place the **Internet Net Sales** measure from the **Internet Sales** table, and the **Reseller Net Sales** measure from the **Reseller Sales** table.