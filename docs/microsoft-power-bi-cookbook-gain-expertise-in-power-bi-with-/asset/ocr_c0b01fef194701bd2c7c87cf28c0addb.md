+   6. Connect to Power BI:
+       Login-PowerBI
+   7. Verify access:
+       Get-PowerBIWorkspace | Format-Table

# How to leverage the Power BI PowerShell Module

To implement this recipe, perform the following steps:

+   1. In PowerShell, get a list of all new workspaces, including personal workspaces, and export this as a CSV file:
+      Get-PowerBIWorkspace -Scope Organization -All | Export-Csv c:\temp\workspaces.csv

+   2. Get a list of all workspaces, including classic workspaces, but excluding personal workspaces, and export this to a CSV file:
+      Get-PowerBIWorkspace -All | Export-Csv c:\temp\workspaces2.csv

+   3. Get a list of all workspace users for all new workspaces:
+      Get-PowerBIWorkspace -Scope Organization -Include All -All |
+      ForEach-Object {
+      $Workspace = $_.name
+      $WorkspaceId = $_.Id
+      foreach ($User in $_.Users) {
+      [PSCustomObject]@{
+      Workspace = $Workspace
+      Id = $WorkspaceId
+      Role = $User.accessright
+      User = $user.Identifier}} | Export-Csv "c:\temp\workspaceusers.csv" -NoTypeInformation

+   4. Create a query called Workspaces2 and disable the load:

```
let
    Source = Csv.Document(File.Contents("C:\temp\workspaces2.csv"),
        [Delimiter=",", Columns=15, Encoding=1252,
        QuoteStyle=QuoteStyle.None]),
    RemoveRows = Table.Skip(Source,1),
    Headers = Table.PromoteHeaders(RemoveRows, [PromoteAllScalars=true]),
    ChangeTypes = Table.TransformColumnTypes(Headers,{"Id", type text},
        {"Name", type text}, {"IsReadOnly", type logical},
        {"IsOnDedicatedCapacity", type logical}, {"CapacityId", type text},
        {"Description", type text}, {"Type", type text}, {"State", type text},
```