+   4. Double-click on it to open the **Foreach Loop Editor**, and assign the properties in the **Collection** pane, as shown in the following screenshot:

![](bbox=[0.105, 0.167, 0.895, 0.448])

+   5. Now go to the **Variable Mappings** pane and add a string variable called `User::AzureAggregatedData`. Make sure the scope is at the package level.

+   6. Drag a **Data Flow Task** into the `felc_StgCloudSales` and rename it `dft_StgCloudSales`.

+   7. Go into the **Data Flow Task** and drag an **Azure Blob Source** from the SSIS toolbox. Rename it `azure_blob_src_ExportBlob`.

+   8. Click anywhere on the background of the data flow and go to the **Properties** pane. Select **Expressions**. Click on the ellipsis button (`...`) and select `[azure_blob_src_ExportBlob].[Blob Name]` from the list. Assign `@[User::AzureAggregatedData]` as the value.