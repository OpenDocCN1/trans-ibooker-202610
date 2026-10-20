# Calculation Groups

Using calculation groups is a great way to reduce the number of DAX measures in your reports. Instead of repeating similar logic for each measure, you can group that logic into a calculation group and reuse it. A common use case is time intelligence calculations, such as year-to-date (YTD), month-to-date (MTD), and last-year-to-date calculations. If you have multiple base measures like sales, quantity, and profit, you’d normally create separate YTD, MTD, and last-year-to-date versions for each one. But with a calculation group, you can apply those calculations across all relevant measures without duplicating the logic.

Before we get into how to create calculation groups, let’s look at where to find them. In Power BI Desktop, go to the Model view (where you can see your tables and relationships). At the top, you’ll see an icon for adding a calculation group (see Figure 5-10).

![](bbox=[0.145, 0.525, 0.855, 0.69])

Figure 5-10. Calculation group button in the Model view in Power BI

Once we create a calculation group, it appears as a table with one column. This column represents the calculation item(s) that we can apply to our existing measures, like sales or quantity. Figure 5-11 shows how a calculation group is displayed in the report view.