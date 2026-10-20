
known values to new values. For example, this can include mapping *MSFT* (the stock market abbreviation for Microsoft) *Microsoft* (the company’s full name).

## Switching from Visual Query Editor to T-SQL Statements

> **NOTE**
> If you want to follow along with script development from a T-SQL perspective, you can click View SQL in the top menu bar found in Figure 4-2.

In Figure 4-9, you’ll see an example taken from the query merge in Figure 4-6, with all columns expanded from the join.

## View SQL

Copy to clipboard

```
select [$Outer].[DateID],
[$Outer].[MedallionID],
[$Outer].[HackneyLicenseID],
[$Outer].[PickupTimeID],
[$Outer].[DropoffTimeID],
[$Outer].[PickupGeographyID],
[$Outer].[DropoffGeographyID],
[$Outer].[PickupLatitude],
[$Outer].[PickupLongitude],
[$Outer].[PickupLatLong],
[$Outer].[DropoffLatitude],
[$Outer].[DropoffLongitude],
[$Outer].[DropoffLatLong],
[$Outer].[PassengerCount],
[$Outer].[TripDurationSeconds],
[$Outer].[TripDistanceMiles],
[$Outer].[PaymentType],
[$Outer].[FareAmount],
[$Outer].[SurchargeAmount],
[$Outer].[TaxAmount],
[$Outer].[TipAmount],
[$Outer].[TollsAmount],
[$Outer].[TotalAmount],
[$Inner].[GeographyID],
[$Inner].[ZipCodeBKKey],
[$Inner].[County],
[$Inner].[City],
[$Inner].[State],
[$Inner].[Country],
[$Inner].[ZipCode]
from [Warehouse].[dbo].[Trip] as [$Outer]
left outer join [Warehouse].[dbo].[Geography] as [$Inner] on ([ $Outer].[PickupGeographyID] = [$Inner].[GeographyID])
```

Edit SQL script Cancel

Figure 4-9. T-SQL statement from the visual query editor

This is a T-SQL statement from the visual query editor, and you can clearly see that the two table names are automatically renamed to [$Inner] and [$Outer], based on the merge operation we did earlier.

There are also two buttons at the bottom of the new window: “Edit SQL script” and “Cancel.”

Clicking the “Edit SQL script” button doesn’t directly open an edit mode for the provided T-SQL script for the visual query editor. Instead, it opens a new query in the warehouse or lakehouse, based on the shown T-SQL script.

From there, you can then modify or change the script to fit your needs.

Figure 4-10 shows an example of this new query.

![](img/8e59e0a64e4cc0c26d8e7f463c62aaad_198_0.png)

Figure 4-10. T-SQL script in edit mode from the visual query editor

There is no standard way of switching from T-SQL to visual query editor in Microsoft Fabric.

## Cross-Lakehouse and Cross-Warehouse Queries

> TIP
> This feature is based on the cross-database query capability from Microsoft Fabric. Cross-database queries and analytics are heavily implemented in Microsoft Fabric and continue to be developed.

The visual query editor also supports the use of tables and views from databases in other warehouses and lakehouses. You can do this by adding a warehouse or lakehouse in the Explorer area on the left side of the visual query editor window. Click the + Warehouses button to open a new window, where you can add reference to a different warehouse. Thereafter, you can work with that warehouse just like any other object.

## Saving a Query or a Query’s Output

When you’re done manipulating data to fit your needs, you have two options:

- Save the query as a view
- Save the output of the query to a table

### Saving a query as a view

To save a query as a view, you start by selecting the “Save as view” option from the top menu bar. That will bring up the pop-up window shown in Figure 4-11.

## Save as view

This will save the text of your SQL query as a view. Make sure the SQL syntax for the view is correct below.

Warehouse

Warehouse

Schema

dbo

View name *

TripWithGeography

SQL statement

```
CREATE VIEW [dbo].[TripWithGeography]
AS
select [$Outer].[DateID],
    [$Outer].[MedallionID],
    [$Outer].[HackneyLicenseID],
    [$Outer].[PickupTimeID],
    [$Outer].[DropoffTimeID],
    [$Outer].[PickupGeographyID],
    [$Outer].[DropoffGeographyID],
    [$Outer].[PickupLatitude],
    [$Outer].[PickupLongitude]
```

Copy to clipboard

OK

Cancel

Figure 4-11. “Save as view” pop-up window from the visual query editor

Then, fill in the “View name” field (in our example, we’ve filled it in with the name TripWithGeography).

Schemas are used to separate logical layers, business areas, and other groupings, based on the technical aspects and implementation rules you may have in your organization. You can select a different schema if you need to (for example, if you have an implemented naming convention in your lakehouse or warehouse).

From this window, you also have the option to copy the view statement to the clipboard and paste it to a new query window and continue your work. Click the “Copy to clipboard” button to do so.

When you’re done, click the OK button and your new view will be populated and ready to use.

### Saving the output of a query to a table

You also have the option to save the output of your visual query to a new table in your warehouse. To do so, click the “Save as table” button at the top of the results area and give your table a new name and schema in the pop-up window that appears (see Figure 4-12).

Notice the change in the SQL statement. In Figure 4-11, the SQL statement begins with CREATE VIEW...AS, but in Figure 4-12, it begins with CREATE TABLE...AS.

Also note that the same “Copy to clipboard” button is available here as in “Save as view” pop-up window.

## Save as table

This will create a table from the results of the visual query that has Enable load applied. To use a different visual query, cancel this dialog and apply Enable load to the visual query you want to use.

Warehouse

Warehouse

Schema

dbo

Table name *

TripsWithGeography

SQL statement

```
CREATE TABLE [Warehouse].[dbo].[TripsWithGeography] AS
    select [$Outer].[DateID],
    [$Outer].[MedallionID],
    [$Outer].[HackneyLicenseID],
    [$Outer].[PickupTimeID],
    [$Outer].[DropoffTimeID],
    [$Outer].[PickupGeographyID],
    [$Outer].[DropoffGeographyID],
    [$Outer].[PickupLatitude],
    [$Outer].[PickupLongitude],
    [$Outer].[PickupLatLong]
```

Copy to clipboard

OK

Cancel

Figure 4-12. “Save as table” pop-up window from the visual query editor

## Limitations

When working with the visual query editor, you can only create data query language (DQL) scripts from the T-SQL package of options. This means that you can’t do data manipulation (insertions, updates, and deletes) on existing tables in the database with the visual query editor. Also, you can only create data definition language (DDL) scripts by using the “Save as table” options from the results area of the visual query editor.

> **NOTE**
> There are some functions and features of the visual query editor that are only available in the visual query editor experience and can’t be translated into T-SQL. Though they’re outside the scope of the DP-600 exam, you might find it interesting to read more about these functions.

## Lineage and Debugging

If you encounter errors or other problems when working with the visual query editor, you have several options for determining what may have gone wrong each step of the way.

### Clicking on Each Box with Steps to View Query Results

When you click each main box of steps (which is also named in the UI as a query) in the visual query editor, the results pane changes to show you the results set from the last step in that query.

### Clicking on Each Step in Each Query to View Results of That Step

You can click on each step inside the queries to see that step’s specific results set.

### Viewing the Data Source Query

You can right-click on each of the steps within a query to view the data source query. For example, Figure 4-13 shows the results of the TripWithGeography visual query’s last step.

## Data source query

```
select [$Outer].[DateID],
    [$Outer].[MedallionID],
    [$Outer].[HackneyLicenseID],
    [$Outer].[PickupTimeID],
    [$Outer].[DropoffTimeID],
    [$Outer].[PickupGeographyID],
    [$Outer].[DropoffGeographyID],
    [$Outer].[PickupLatitude],
    [$Outer].[PickupLongitude],
    [$Outer].[PickupLatLong],
    [$Outer].[DropoffLatitude],
    [$Outer].[DropoffLongitude],
    [$Outer].[DropoffLatLong],
    [$Outer].[PassengerCount],
    [$Outer].[TripDurationSeconds],
    [$Outer].[TripDistanceMiles],
    [$Outer].[PaymentType],
    [$Outer].[FareAmount],
    [$Outer].[SurchargeAmount],
    [$Outer].[TaxAmount],
    [$Outer].[TipAmount],
    [$Outer].[TollsAmount],
    [$Outer].[TotalAmount],
    [$Inner].[GeographyID],
    [$Inner].[ZipCodeBKey],
    [$Inner].[County],
    [$Inner].[City],
    [$Inner].[State],
    [$Inner].[Country],
    [$Inner].[ZipCode]
from [Warehouse].[dbo].[Trip] as [$Outer]
left outer join [Warehouse].[dbo].[Geography] as [$Inner] on ([$Outer].[PickupGeographyID] = [$Inner].[GeographyID])
```

Figure 4-13. View of data source query
