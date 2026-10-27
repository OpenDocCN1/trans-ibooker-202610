# How it works...

A separate workspace is not technically necessary for **publish to web**, but this isolation is recommended for manageability and limiting the risk of publishing confidential or proprietary information. Similarly, Premium capacity is not required but could be appropriate for larger datasets or when more frequent data refreshes and consistent performance are important.

The HTML code provided can be edited manually to improve the fit of the report on the destination for embedding. Adding 56 pixels to the height dimension can adjust the size of the bottom bar. Setting the page size in Power BI Desktop, the view mode in the Power BI service (the **View** icon in the report header at the upper right), and manually adjusting the iFrame height and width values may be necessary for a perfect fit.

Per *Figure 12.39*, Power BI reports can also be embedded in SharePoint Online. Clicking on **SharePoint Online** provides a URL that can be used with a Power BI web part in SharePoint online. Users accessing the SharePoint online page must also have access to the report in the Power BI service.

Power BI caches the report definition and the results of the queries required to view the report as users view the report. Given the cache, it can take approximately one hour before changes to the report definition or the impact of dataset refreshes are reflected in the version of the report viewed by users.

# There's more...

All **Publish to web** and other embed codes can be managed by members of the workspace where the embed code was created. In addition, all embed codes from all workspaces can be managed by administrators in a central location. To see how this is accomplished, do the following:

+   1. In the Power BI service, open the workspace where the **Publish to web** embed code was created.
+   2. Click the gear icon in the upper-right corner and select **Manage embed codes**.
+   3. All embed codes created for the workspace are displayed and these codes can be retrieved or deleted using the ellipsis (**...**).

![](bbox=[0.202, 0.729, 0.865, 0.853])

Figure 12.41: Manage embed codes interface for a workspace