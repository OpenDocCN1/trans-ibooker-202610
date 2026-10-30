```
entity.Property(e => e.TimeStamp)
    .IsRowVersion()
    .IsConcurrencyToken();
});
```

If you were to create and run a migration now, you would find that nothing has changed since the commands in the Fluent API match the current configuration defined by the conventions and data annotations.

# Default Values

The Fluent API provides methods to set default values for columns. The default value can be a value type or a SQL string. For example, to set the default Color for a new Car to Black, use the following:

```
modelBuilder.Entity<Car>(entity =>
{
    ...
    entity.Property(e => e.Color)
        .HasColumnName("CarColor")
        .IsRequired()
        .HasMaxLength(50)
        .HasDefaultValue("Black");
});
```

To set the value to a database function (like getdate()), use the HasDefaultValueSql() method. Presume that a DateTime property named DateBuilt has been added to the Car class and the default value should be the current date using the SQL Server getdate() method. The columns gets configured like this:

```
modelBuilder.Entity<Car>(entity =>
{
    ...
    entity.Property(e => e.DateBuilt)
        .HasDefaultValueSql("getdate()");
});
```

Just like using SQL to insert a record, if a property that maps to a column with a default value has a value when EF Core inserts the record, the property’s value is used instead of the default. If the property’s value is null, the column’s default value is used.

A problem exists when the property’s data type has a default value. Recall that numbers default to zero and Booleans default to false. If you set the value of numeric properties to zero or Boolean properties to false and then insert that entity, EF Core will treat that property as not having a value set. If that property maps to a column with a default value, the default value in the column definition is used.

For example, add a bool property named IsDrivable to the Car class. Set the default for the property’s column mapping to true.

```
//Car.cs
public class Car : BaseEntity
{
    ...
    public bool IsDrivable { get; set; }
}
```