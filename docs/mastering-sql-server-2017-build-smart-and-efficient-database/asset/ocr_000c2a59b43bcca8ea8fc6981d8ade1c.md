```
```sql
GO
INSERT INTO dbo.T1 (id, c1, c2) VALUES
    (1, 'Benfica Lisbon','20180115'),
    (2, 'Manchester United','20180202'),
    (3, 'Rapid Vienna','20180128'),
    (4, 'Juventus Torino','20180225'),
    (5, 'Red Star Belgrade','20180225');
```
```

In the next sections, you will enable and use the Stretch DB feature for the T1 table. Assume that you want to move all rows from this table with a value in the c2 column that is older than *1st February 2018* to the cloud.

# Enabling Stretch DB for a table by using wizard

You can create a new table with the Stretch DB feature enabled or enable it for an existing table using the **Enable Table for Stretch** wizard. To launch it, you need to navigate to the T1 table under the database Mila in SSMS. Then, after right-clicking, you need to select the option **Tasks/Stretch/Enable** respectively. You should get a following screenshot:

![](bbox=[0.105, 0.456, 0.895, 0.765])