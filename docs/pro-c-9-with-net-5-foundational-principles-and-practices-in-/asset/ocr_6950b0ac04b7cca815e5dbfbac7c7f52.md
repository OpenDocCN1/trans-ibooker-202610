> Note Records can also be added using the derived DbContext as well. These examples will all use the DbSet<T> collection properties to add the records. Both DbSet<T> and DbContext have async versions of Add()/AddRange(). Only the synchronous versions are shown.

# Entity State

When an entity is created through code but not yet added to a DbSet<T>, the EntityState is Detached. Once a new entity is added to a DbSet<T>, the EntityState is set to Added. After SaveChanges() executes successfully, the EntityState is set to Unchanged.

# Add a Single Record

The following test demonstrates how to add a single record to the Inventory table:

```
[Fact]
public void ShouldAddACar()
{
    ExecuteInATransaction(RunTheTest);

    void RunTheTest()
    {
        var car = new Car
        {
            Color = "Yellow",
            MakeId = 1,
            PetName = "Herbie"
        };
        var carCount = Context.Cars.Count();
        Context.Cars.Add(car);
        Context.SaveChanges();
        var newCarCount = Context.Cars.Count();
        Assert.Equal(carCount+1,newCarCount);
    }
}
```

The executed SQL statement is shown here. Notice that the recently added entity is queried for the database-generated properties (Id and TimeStamp). When the results of the query come to EF Core, the entity is updated with the server-side values.

```
exec sp_executesql N'SET NOCOUNT ON;
INSERT INTO [dbo].[Inventory] ([Color], [MakeId], [PetName])
VALUES (@p0, @p1, @p2);

SELECT [Id], [IsDrivable], [TimeStamp]
FROM [dbo].[Inventory]
WHERE @@ROWCOUNT = 1 AND [Id] = scope_identity();

',N'@p0 nvarchar(50),@p1 int,@p2 nvarchar(50)',@p0=N'Yellow',@p1=1,@p2=N'Herbie'
```