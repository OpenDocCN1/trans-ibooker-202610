# Viewing the Data Source Query

You can right-click on each of the steps within a query to view the data source query. For example, Figure 4-13 shows the results of the TripWithGeography visual query’s last step.

# Data source query

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