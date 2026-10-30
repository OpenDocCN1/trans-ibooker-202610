How this situation is handled depends on the requirements for the application. Solutions range from doing nothing (second update overwrites the first) to using optimistic concurrency (the second update fails) to more complicated solutions such as checking individual fields. Except for the choice of doing nothing (universally considered a bad programming idea), developers need to know when concurrency issues arise so they can be handled appropriately.

Fortunately, many modern databases have tooling to help the development team handle concurrency issues. SQL Server has a built-in data type called timestamp, a synonym for rowversion. If a column is defined with a data type of timestamp, when a record is added to the database, the value for the column is created by SQL Server, and when a record is updated, the value for the column is updated as well. The value is virtually guaranteed to be unique and controlled by SQL Server.

EF Core can leverage the SQL Server timestamp data type by implementing a Timestamp property on an entity (represented as byte[] in C#). Entity properties defined with the Timestamp attribute or Fluent API designation are added to the where clause when updating or deleting records. Instead of just using the primary key value(s), the generated SQL adds the value of the timestamp property to the where clause. This limits the results to those records where the primary key and the timestamp values match. If another user (or the system) has updated the record, the timestamp values will not match, and the update or delete statement will not update the record. Here is an example of an update query using the Timestamp column:

```
UPDATE [Dbo].[Inventory] SET [Color] = N'Yellow'
WHERE [Id] = 1 AND [TimeStamp] = 0x000000000000081F;
```

When the data store reports a number of records affected that is different than the number of records the ChangeTracker expected to be changed, EF Core throws a DbUpdateConcurrencyException and rolls the entire transaction back. DbUpdateConcurrencyException contains information for all of the records that did not persist, including the original values (when the entity was loaded from the database) and the current values (as the user/system updated them). There is also a method to get the current database values (this requires another call to the server). With this wealth of information, the developer can then handle the concurrency error as the application requires. The following code shows this in action:

```
try
{
    //Get a car record (doesn't matter which one)
    var car = Context.Cars.First();
    //Update the database outside of the context
    Context.Database.ExecuteSqlInterpolated($"Update dbo.Inventory set Color='Pink' where Id = {car.Id}");
    //update the car record in the change tracker and then try and save changes
    car.Color = "Yellow";
    Context.SaveChanges();
}
catch (DbUpdateConcurrencyException ex)
{
    //Get the entity that failed to update
    var entry = ex.Entries[0];
    //Get the original values (when the entity was loaded)
    PropertyValues originalProps = entry.OriginalValues;
    //Get the current values (updated by this code path)
    PropertyValues currentProps = entry.CurrentValues;
    //get the current values from the data store -
    //Note: This needs another database call
    //PropertyValues databaseProps = entry.GetDatabaseValues();
}
```