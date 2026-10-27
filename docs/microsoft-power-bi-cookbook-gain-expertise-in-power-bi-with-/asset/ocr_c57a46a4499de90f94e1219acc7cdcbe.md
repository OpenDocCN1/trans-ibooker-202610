```
,vProducts.EnglishProductCategoryName
,FactInternetSales.OrderDate
FROM
    vProducts
    INNER JOIN FactInternetSales
        ON vProducts.ProductKey = FactInternetSales.ProductKey
```

+   9. Click the **OK** button to exit **Query Designer** and then click the **Next>** button.
+   10. Choose **Tabular** and click the **Next>** button.
+   11. Click the **Finish>>** button.
+   12. Click the **Finish** button.
+   13. Remove the table created in the central design panel (design surface).
+   14. Use the dataset to create SSRS report visuals for pinning. Charts, gauge panels, maps, and images can be pinned from SSRS to Power BI dashboards.
+   15. Deploy the SSRS report to a report folder in the SSRS portal and confirm that it renders properly:

![](bbox=[0.2, 0.41, 0.87, 0.53])

Figure 13.3: Pin SSRS report visual to Power BI Dashboard

+   16. Click on the Power BI icon and then click on the chart in the report you wish to pin.
+   17. Choose the **Workspace**, the **Dashboard**, and the **Frequency of updates**:

![](bbox=[0.2, 0.62, 0.87, 0.87])

Figure 13.4: Pin to Power BI from SSRS