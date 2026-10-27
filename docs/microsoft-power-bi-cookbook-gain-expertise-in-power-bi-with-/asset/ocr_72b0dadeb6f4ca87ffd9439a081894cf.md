+   6. Choose **Local File**.
+   7. Select the CH5_R1_SalesAndFinanceDataset.pbix file and click **Open**.
+   8. Open the CH5_R1_RegionMonthlyReport.pbix file in Power BI Desktop.
+   9. Sign in to the Power BI service if necessary, and in the **Unable to connect** dialog, choose **Edit**. If you receive an error, contact your administrator to allow a connection to live datasets.
+   10. On the **Select a dataset to create a report** page, choose the **CH5_R1_SalesAndFinanceDataset**, and then click the **Create** button.
+   11. **Save** and **Publish** the report to the service in the **Workspace** created in *step 3*.
+   12. Repeat *steps 8, 9, 10, and 11* for CH5_R1_CountryMonthlyReport.pbix, and CH5_R1_MarginAnalysisReport.pbix.

If you are unable to create a **Workspace** in the Power BI service, you can use **My workspace** instead. After completing these steps, the reports all have a live connection back to the dataset, CH5_R1_SalesAndFinanceDataset.

# How to Build a Dashboard

To implement this recipe, use the following steps:

+   1. In the Power BI service, select the workspace created in the *Getting started* section of this recipe.
+   2. In the central area, click **New** and then choose **Dashboard**.

![](bbox=[0.2, 0.54, 0.87, 0.73])

Figure 5.4: Create a new dashboard

+   3. In the **Create dashboard** dialog, enter **North American Sales** for **Dashboard name** and then click **Create**.
+   4. A blank dashboard is created; click on the workspace in the left navigation pane and note that the **North American Sales** dashboard is shown in the list.
+   5. In the service, open the **CH5_R1_RegionMonthlyReport** report and navigate to the **North America** page.