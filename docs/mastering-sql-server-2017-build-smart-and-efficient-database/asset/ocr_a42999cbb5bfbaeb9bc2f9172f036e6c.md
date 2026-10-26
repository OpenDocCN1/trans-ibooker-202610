```
```sql
GO
CREATE TABLE dbo.T1_Hist(
    Id INT NOT NULL,
    C1 INT,
    Vf DATETIME2 NOT NULL,
    Vt DATETIME2 NOT NULL
)
GO
--populate tables
INSERT INTO dbo.T1_Hist(Id, C1, Vf, Vt) VALUES
(1,1,'20171201','20171210'),
(1,2,'20171210','20171215');
GO
INSERT INTO dbo.T1 (Id, C1, Vf, Vt) VALUES
(1,3,'20171215','99991231 23:59:59.9999999');
GO
```
```

Here is the content of both tables after the script execution:

```text
Id    C1          Vf                         Vt
---   --------    -------------------------  ---------------------------
1     3           2017-12-15 00:00:00.0000000  9999-12-3 23:59:59.9999999
```

```text
Id    C1          Vf                         Vt
---   --------    -------------------------  ---------------------------
1     1           2017-12-01 00:00:00.0000000  2017-12-10 00:00:00.0000000
1     2           2017-12-10 00:00:00.0000000  2017-12-15 00:00:00.0000000
```

The actual value in the C1 column for the row with the ID of 1 is 3, and there are two historical values. You will now convert the T1 table into a temporal table and so define a retention policy that states historical entries should be retained for one day only. Here is the appropriate code:

```
```sql
ALTER TABLE dbo.T1 ADD PERIOD FOR SYSTEM_TIME (Vf, Vt);
GO
ALTER TABLE dbo.T1 SET
(
    SYSTEM_VERSIONING = ON
    (
        HISTORY_TABLE = dbo.T1_Hist,
        HISTORY_RETENTION_PERIOD = 3 DAYS
    )
);
```
```