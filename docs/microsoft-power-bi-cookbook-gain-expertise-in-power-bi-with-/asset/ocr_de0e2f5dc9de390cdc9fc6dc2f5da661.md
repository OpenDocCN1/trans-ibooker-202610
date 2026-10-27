```
foreach ($Report in $_.Reports) {
[PSCustomObject]@{
Workspace = $Workspace
Id = $WorkspaceId
ReportID = $Report.Id
Report = $Report.Name}} | Export-Csv "c:\temp\wsreports.csv" -NoTypeInformation
```

+   8. Click **Close & Apply** from the ribbon of the **Home** tab.
+   9. Create a relationship between the **ID** columns in the **Workspaces** and **WorkspaceUsers** tables.
+   10. Optionally, create additional relationships between the **ID** columns in the **Workspaces** table and other tables for Reports, Dashboards, Datasets, Dataflows, and Workbooks.
+   11. Create a measure to concatenate the users:
+       **Users = CONCATENATEX('WorkspaceUsers',[User],", ")**
+   12. Create a matrix visualization using the **Name** column from the **Workspaces** table as **Rows**, the **Role** column from the **WorkspaceUsers** table as **Column**, and the **Users** measure as **Values**.

# How it works...

Classic Power BI workspaces have less functionality than newer ones. With Classic workspaces, the list of user permissions cannot be retrieved through the Power BI API because access is granted through Office 365 groups. In addition, PSCustomObject is not included in cmdlet calls that support classic workspaces. As a Power BI Admin, you can upgrade workspaces by accessing the Power BI **Admin portal** and selecting **Workspaces**. In the **Workspaces** listed, select one or more workspaces, and then click **Upgrade** from the toolbar.

With the matrix visualization created in this recipe, it is easy to find classic workspaces by clicking on the dropdown for the **Name** column in the **Rows** field and selecting **Show items with no data**. Non-orphaned workspaces with no users listed, such as **Admin**, **Contributor**, **Member**, or **Viewer**, are almost certainly classic workspaces that should be upgraded.

# There's more...

There are many PowerShell cmdlets available for Power BI, including the following:

+   1. Get a list of all deleted workspaces:
+       **Get-PowerBIWorkspace -Scope Organization -Deleted -All**