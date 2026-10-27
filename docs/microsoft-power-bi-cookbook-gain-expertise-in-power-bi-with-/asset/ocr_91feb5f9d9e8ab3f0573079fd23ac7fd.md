This recipe provides data model design and metadata tips to prepare a dataset for Q&A. Additionally, synonyms are added to a Power BI Desktop data model to improve the accuracy of natural language queries.

# Getting ready

To prepare for this recipe:

+   - Complete the first recipe in this chapter, *Building a Dashboard*.

# How to Prepare for Q&A

To implement this recipe, use the following steps:

+   1. Open the **North American Sales** dashboard.
+   2. At the upper left of the dashboard, click **Ask a question about your data**.

![](bbox=[0.2, 0.407, 0.866, 0.44])

Figure 5.13: Power BI Q&A

+   3. Type the following: `wholesaler net sales by country.`
+   4. Notice that the word **wholesaler** is double-underlined in red, **net sales** is underlined in blue, and **country** is underlined with a dashed red line.

![](bbox=[0.2, 0.556, 0.866, 0.622])

Figure 5.14: Showing results for net sales by reseller country

+   5. Hover over one of the map bubbles displayed and notice that the tooltip reads **Total Net Sales**, and not **Reseller Net Sales**.
+   6. Click the thumbs-down icon in the lower-right corner of the screen.
+   7. Open the file `CH5_R1_SalesAndFinanceDataset.pbix`.
+   8. Change to the **Model** view.
+   9. Click **Q&A setup** in the ribbon of the **Home** tab.