# Save as table

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

# Limitations

When working with the visual query editor, you can only create data query language (DQL) scripts from the T-SQL package of options. This means that you can’t do data