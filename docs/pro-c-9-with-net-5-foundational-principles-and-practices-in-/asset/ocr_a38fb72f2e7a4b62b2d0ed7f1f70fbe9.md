# Creating the CreditRisks Table

You will use your final table, CreditRisks, to represent the customers that are considered a credit risk. Create a new query, enter the following code, and click Run (or press F5):

```
USE [AutoLot]
GO
CREATE TABLE [dbo].[CreditRisks](
    [Id] [int] IDENTITY(1,1) NOT NULL,
    [FirstName] [nvarchar](50) NOT NULL,
    [LastName] [nvarchar](50) NOT NULL,
    [CustomerId] [int] NOT NULL,
    [TimeStamp] [timestamp] NULL,
    CONSTRAINT [PK_CreditRisks] PRIMARY KEY CLUSTERED
(
    [Id] ASC
) ON [PRIMARY]
) ON [PRIMARY]
GO
```

# Creating the Table Relationships

This next section will add in the foreign key relationships between the interrelated tables.

# Creating the Inventory to Makes Relationship

Open a new query, enter the following SQL, and click Run (or press F5):

```
USE [AutoLot]
GO
CREATE NONCLUSTERED INDEX [IX_Inventory_MakeId] ON [dbo].[Inventory]
(
    [MakeId] ASC
) ON [PRIMARY]
GO
ALTER TABLE [dbo].[Inventory] WITH CHECK ADD CONSTRAINT [FK_Make_Inventory] FOREIGN KEY([MakeId])
REFERENCES [dbo].[Makes] ([Id])
GO
ALTER TABLE [dbo].[Inventory] CHECK CONSTRAINT [FK_Make_Inventory]
GO
```

# Creating the Inventory to Orders Relationship

Open a new query, enter the following SQL, and click Run (or press F5):

```
USE [AutoLot]
GO
CREATE NONCLUSTERED INDEX [IX_Orders_CarId] ON [dbo].[Orders]
```