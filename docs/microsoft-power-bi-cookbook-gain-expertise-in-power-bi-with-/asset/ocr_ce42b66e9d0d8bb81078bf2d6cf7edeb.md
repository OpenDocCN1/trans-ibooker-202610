EffectiveUserName connection property. This allows RLS roles defined in the SSAS database to be applied to Power BI users.

+   6. Under **Advanced Settings**, check that the source uses the appropriate privacy level, such as organizational or private.

+   7. Click on **Add** and then, via the **Users** tab, add users authorized to use this gateway for this data source.

![](bbox=[0.2, 0.21, 0.868, 0.317])

Figure 12.23: Successful setup of a data source for the on-premises data gateway

+   8. Add an additional data source for the Excel file using the file data source type.

+   9. Like the SQL Server data source, authorize users for this gateway and this data source via the **Users** page.

+   10. Publish the import mode Power BI Desktop file (dataset) to a workspace in the Power BI service.

+   11. Access the workspace and from the **Datasets + dataflows** list, click on the **Schedule refresh** icon for the dataset.

![](bbox=[0.2, 0.518, 0.869, 0.677])

Figure 12.24: Actions available to a published dataset in the Power BI service

+   12. Expand the **Gateway connection** section.

The gateway will appear as an option for data refresh if the following three criteria are met:

+   - ☐ The user is listed on the **Users** page of the data source(s) within the gateway
+   - ☐ The server and database names configured in the Power BI Service for the gateway match the names used in the Power BI Desktop file
+   - ☐ Each data source used by the dataset is configured as a data source for the gateway