# # Temporal constraints

Depending on the business problem you are solving, you might need to implement many temporal constraints. Remember that for application time, SQL Server does not help you much. You need to implement the constraints in your code, using SQL Server declarative constraints where possible. However, most of the constraints you need to implement through custom code, either in triggers or in stored procedures, or even in the application code.

Imagine the Suppliers table example. One supplier can appear multiple times in the table because the same supplier could be under contract for separate periods of time. For example, you could have two tuples like this in the relation with the shortened header Suppliers (supplierid, companyname, from, to):

```
```text
{2, Supplier VHQZD, d05, d07}
{2, Supplier VHQZD, d12, d27}
```
```

Here are some possible constraints you might need to implement:

+   - *To* should never be less than *from*
+   - Two contracts for the same supplier should not have overlapping time intervals
+   - Two contracts for the same supplier should not have abutting time intervals
+   - No supplier can be under two distinct contracts at the same point in time
+   - There should be no supplies from a supplier at a point in time when the supplier is not under contract

You might find even more constraints. Anyway, SQL Server 2016 brings support for system versioned tables only. To maintain application validity times, you need to develop the code by yourself.

# # Temporal data in SQL Server before 2016

As mentioned, in SQL Server versions before 2016, you need to take care of temporal data by yourself. Even in SQL Server 2016, you still need to take care of the human, or application, times. The following code shows an example of how to create a table with validity intervals expressed with the b and e columns, where the beginning and the end of an interval are represented as integers. The table is populated with demo data from the WideWorldImporters.Sales.OrderLines table:

```
```sql
USE tempdb;
GO
SELECT OrderLineID AS id,
```
```