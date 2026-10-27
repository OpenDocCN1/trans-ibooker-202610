+   5. From the **Report** view, create card visuals to display the **Last Month Internet Sales**, **Last Month Predicted Internet Sales**, **Mkt-Sales Correl**, **Mkt-Sales R Squared**, and **Regression Line Message** measures
+   6. Create a scatter chart visualization using the **Marketing Amount** column (sum) from the **MktSalesRegression** table as the **X Axis**, the **Internet Sales** column (sum) from the **MktSalesRegression** table as the **Y Axis**, and the **YearMonth** column from the **MktSalesRegression** table as **Details**
+   7. Add a **Trend** line from the **Analytics** pane
+   8. Create a second scatter chart visualization using the **Predicted Internet Sales** column (sum) from the **Residuals** table as the **X Axis**, the **Residuals Z Score** column (sum) from the **Residuals** table as the **Y Axis**, and the **YearMonth** column from the **Residuals** table as **Details**

![](bbox=[0.2, 0.32, 0.88, 0.6])

Figure 9.23: Regression report page

With this report design, the user can instantly perceive the strength of the relationship via the **Marketing Amount and Internet Sales by YearMonth** scatter chart and the high values for the correlation and R squared cards shown in *Figure 9.23*. The **Predicted Internet Sales and Residuals Z Score by YearMonth** scatter chart helps to identify the months with relatively large variations.

# How it works

In this example, we have obviously engineered a monthly marketing spend metric to have a high correlation with internet sales. Had we used the actual **Sales and Marketing** expenditure numbers from the general ledger (**FactFinance**) fact table, we would have found no correlation at all.