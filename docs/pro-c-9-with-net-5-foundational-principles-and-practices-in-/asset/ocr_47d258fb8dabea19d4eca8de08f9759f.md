The easiest way to resolve this is to comment out the TimeStamp property on the base entity, create and apply a migration, and then uncomment the TimeStamp and create and apply another migration.

Comment out the TimeStamp property and data annotation and execute these commands:

```
dotnet ef migrations add RemoveTimeStamp -o Migrations -c AutoLot.Samples.ApplicationDbContext
dotnet ef database update RemoveTimeStamp  -c AutoLot.Samples.ApplicationDbContext
```

Uncomment the TimeStamp property and data annotation and run these commands to add the property to each table as a timestamp column:

```
dotnet ef migrations add ReplaceTimeStamp -o Migrations -c AutoLot.Samples.ApplicationDbContext
dotnet ef database update ReplaceTimeStamp  -c AutoLot.Samples.ApplicationDbContext
```

Now your database matches your model.

# Annotations and Navigation Properties

The ForeignKey annotation lets EF Core know which property is the backing field for the navigation property. By convention, <TypeName>Id would automatically be set as the foreign key property, but in the previous example it is explicitly set. This supports different naming styles as well as having more than one foreign key to the same table. It also (in my honest opinion) increases the readability of the code.

InverseProperty informs EF Core of how the tables relate by indicating what the navigation property is on the other entities that navigates back to this entity. InverseProperty is required when an entity relates to another entity more than once and also (again, in my honest opinion) makes the code more readable.

# The Fluent API

The Fluent API configures the application entities through C# code. The methods are exposed by the ModelBuilder instance available in the DbContext OnModelCreating() method. The Fluent API is the most powerful of the configuration methods and overrides any conventions or data annotations that are in conflict. Some configuration options are only available using the Fluent API, such as setting default values and cascade behavior for navigation properties.

# Class and Property Mapping

The following code shows the previous Car example with the Fluent API equivalent to the data annotations used (omitting the navigation properties, which will be covered next).

```
modelBuilder.Entity<Car>(entity =>
{
    entity.ToTable("Inventory", "dbo");
    entity.HasKey(e=>e.Id);
    entity.HasIndex(e => e.MakeId, "IX_Inventory_MakeId");
    entity.Property(e => e.Color)
        .IsRequired()
        .HasMaxLength(50);
    entity.Property(e => e.PetName)
        .IsRequired()
        .HasMaxLength(50);
}
```