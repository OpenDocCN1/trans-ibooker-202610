
## See also

- Visualization types in Power BI: http://bit.ly/3cHg2vh

## Creating Table and Matrix visuals

Table and Matrix visuals are appropriate when visibility of precise, individual values are needed, or when data is viewed at a detailed level, such as in individual transactions. Table visuals in Power BI conform to the classic "list" report format of columns and rows but support powerful cross-highlighting and formatting options, including conditional formatting. Matrix visuals include Table visual features and correspond to the layout and general functionality of pivot tables in Excel: two-dimensional representations of measures with the ability to drill up and down the row and column axes.

In this recipe, the various capabilities of Table and Matrix visualizations are demonstrated, including conditional formatting, drill-down, and cross-highlighting.

### Getting ready

To prepare for this recipe, follow these steps:

- 1. Open the CH4_R6.pbix file and save the file as CH4_R7.pbix.
- 2. Switch to the Scratch page and use Ctrl-A and then Delete to remove any visuals.
- 3. Check the Filters pane and ensure that only the Europe filter is active.

### How to Create Table and Matrix visuals

To implement this recipe, use the following steps:

- 1. Select the France page.
- 2. From the Visualizations pane, select the Table visualization.
- 3. Add the Product Category column from the Products table, the Internet Net Sales, Internet Margin %, and Internet Customer Count measures from the Internet Sales table, and the Reseller Net Sales and Reseller Margin % measures from the Reseller Sales table.
- 4. Rearrange the measures as necessary in the Values area for the visualization.
- 5. In the Values well for the visualization, double-click the Internet Customer Count measure and rename it to Internet Customers.
- 6. In the General section of the Format pane, set the X Position to 448, Y Position to 526, Width to 832, and Height to 194.
- 7. Expand the Style section and choose Contrast alternating rows.
- 8. Copy this visual to the Germany, United Kingdom, and Scratch pages.
- 9. On the Scratch page, use the drop-down for the Reseller Margin % measure in the Values well to apply Conditional formatting to the Background color.

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_229_0.png)

Figure 4.33: Conditional formatting

- 10. In the conditional formatting dialog, check the Diverging checkbox, and then click the OK button.

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_229_1.png)

Figure 4.34: Diverging colors for conditional formatting

- 11. Conditionally format the background for **Internet Net Sales**, but this time change **Format by** to **Rules** and in the **Rules** change the drop-down to **is blank**.

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_230_0.png)

Figure 4.35: Format by rules

- 12. Conditionally format the **Reseller Net Sales** measure, but this time choose **Data bars** and click **OK** in the **Data bars** dialog to accept the default settings.

| Product Category | Internet Net Sales | Reseller Net Sales | Internet Margin % | Reseller Margin % | Internet Customers |
|---|---:|---:|---:|---:|---:|
| Accessories | $202,269 | $125,708 | 62.6% | 33.9% | 4,717 |
| Bikes | $8,644,933 | $8,509,428 | 40.6% | -4.1% | 2,752 |
| Clothing | $82,840 | $318,540 | 34.9% | 8.8% | 2,034 |
| Components | — | $1,916,858 | — | 7.2% | — |
| **Total** | **$8,930,042** | **$10,870,535** | **41.0%** | **-1.3%** | **5,503** |

Figure 4.36: Table with conditional formatting applied

- 13. Copy and paste the **Table visual** on the **Scratch** page and reposition.
- 14. Remove the **Reseller Net Sales** and **Reseller Margin %** measures from this new visual and note that the **Components** row disappears from the table.
- 15. In the **Values** well for the visual, expand the menu for **Product Category**, choose **Show items with no data**, and note that the **Components** row reappears along with an entirely blank row.
- 16. In the **Values** well for the visual, expand the menu for **Internet Customers**, choose **Show value as** and then **Percent of grand total**, and note that percentages are now displayed in the **Internet Customers** column, and the blank row and the **Components** row disappear.
- 17. Add a **Matrix visualization** to the **Scratch** page.
- 18. Use the Sales Territory Hierarchy from the Sales Territories table for the rows, the Product Category Hierarchy from the Products table for Columns, and the Internet Net Sales measure from the Internet Sales table for Values.
- 19. Expand **Europe** using the **+** icon for the row.
- 20. Switch **Drill** on to **Columns**, choose the down arrow to turn on *drill* mode and click on the **Bikes** column.

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_231_0.png)

Figure 4.37: Matrix visualization

- 21. Save your work.

### How it works

Table and Matrix visualizations automatically filter out rows and columns for which there is no data. In effect, if the aggregations or measures in the **Values** area all evaluate to blank, then the row is not displayed in the table. The **Show items with no data** feature overrides this default behavior but is not foolproof, as shown in the case of using the **Show value as** feature. An alternative approach is to construct measures in such a way that they return zero instead of a blank, as in the examples below:

```
Measure =
    VAR __Sum = SUM('Internet Sales'[Sales Amount])
RETURN
    IF(ISBLANK(__Sum),0,__Sum)
```

or

```
Measure = SUM('Internet Sales'[Sales Amount]) + 0
```

or

```
Measure =
    COALESCE(
        SUM('Internet Sales'[Sales Amount]),
        0
    )
```

Data bars can be displayed with the measure values, as per this example, or as standalone bars. Specific minimum and maximum threshold values can also be entered to drive the conditional formatting of the bars.

In addition to conditionally formatting background colors and data bars, conditional formatting is also available for fonts as well as icons. If the same formatting logic and colors are used for both font color scales and background color scales, only the color of the value will be displayed, such as in a heat map.

A **Show values as** option is available for measures added to Table and Matrix visuals to display the measure value as a percentage of the row, column, or grand total. However, measures in Table and Matrix visuals cannot currently be formatted to display units, nor decimal places, such as thousands or millions. For Table visuals, only **Percent of grand total** is available, given the single dimension of the visual.

With Matrix visuals, the rows hierarchy can be navigated via the **+** and **-** icons at the start of each row. Alternatively, selecting **Rows** for the **Drill on** drop-down allows the drill-down icons to be used to navigate to the row hierarchy. Similarly, selecting **Columns** for the **Drill on** drop-down allows the columns to be navigated to via the same drill-down icons. There are four drill-down icons: the up arrow icon drills up one layer in the hierarchy; the down arrow icon turns on drill mode. When drill mode is active (solid circle), then clicking on a row or column drills one layer deeper into the hierarchy for that particular column, or row, only. The double down arrow icon drills one layer deeper into the hierarchy for all rows or columns, but the parent hierarchy context is lost in this case. To drill one layer deeper into the hierarchy for all rows or columns and preserve the parent hierarchy context, use the forked down arrow icon.

### There's more...

An additional conditional formatting option for Table and Matrix visualizations is **Web URL**. Using the **Web URL** conditional formatting makes the displayed value into a live link. To demonstrate how this works, do the following:

- On the **Scratch** page, create a **Table visualization** using the **Customer Name**, **Email**, and **Service URL** columns from the **Customers** table.
- In the **Values** well for the **Table visual**, access the **Conditional formatting** for the **Customer Name** measure and choose **Web URL**.
- In the **Web URL** dialog box, change **Based on field** to the **Email** column in the **Customers** table.

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_232_0.png)

Figure 4.38: Web URL conditional formatting
