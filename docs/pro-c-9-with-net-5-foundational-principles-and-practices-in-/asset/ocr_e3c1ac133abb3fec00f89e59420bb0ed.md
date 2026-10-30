The executed SQL statement is listed here:

```
exec sp_executesql N'SET NOCOUNT ON;
UPDATE [dbo].[Inventory] SET [Color] = @p0
WHERE [Id] = @p1 AND [TimeStamp] = @p2;
SELECT [TimeStamp]
FROM [dbo].[Inventory]
WHERE @@ROWCOUNT = 1 AND [Id] = @p1;
',N'@p1 int,@p0 nvarchar(50),@p2 varbinary(8)',@p1=1,@p0=N'White',@p2=0x0000000000000862D
```

# Concurrency Checking

The previous chapter covered concurrency checking in great detail. As a reminder, when an entity has a TimeStamp property defined, the value of that property is used in the where clause when changes (updates or deletes) are being persisted to the database. Instead of just searching for the primary key, the TimeStamp value is added to the query, like this example:

```
UPDATE [dbo].[Inventory] SET [PetName] = @p0
WHERE [Id] = @p1 AND [TimeStamp] = @p2;
```

The following test shows an example of creating a concurrency exception, catching it, and using the Entries to get the original values, current values, and the values that are currently stored in the database. Getting the current values requires another database call.

```
[Fact]
public void ShouldThrowConcurrencyException()
{
    ExecuteInATransaction(RunTheTest);

    void RunTheTest()
    {
        var car = Context.Cars.First();
        //Update the database outside of the context
        Context.Database.ExecuteSqlInterpolated(
            $"Update dbo.Inventory set Color='Pink' where Id = {car.Id}");
        car.Color = "Yellow";
        var ex = Assert.Throws<CustomConcurrencyException>(
            () => Context.SaveChanges());
        var entry = ((DbUpdateConcurrencyException) ex.InnerException)?.Entries[0];
        PropertyValues originalProps = entry.OriginalValues;
        PropertyValues currentProps = entry.CurrentValues;
        //This needs another database call
        PropertyValues databaseProps = entry.GetDatabaseValues();
    }
}
```

The executed SQL calls are listed here. The first is the update statement, and the second is for the call to get the database values.

```
exec sp_executesql N'SET NOCOUNT ON;
UPDATE [dbo].[Inventory] SET [Color] = @p0
WHERE [Id] = @p1 AND [TimeStamp] = @p2;
```