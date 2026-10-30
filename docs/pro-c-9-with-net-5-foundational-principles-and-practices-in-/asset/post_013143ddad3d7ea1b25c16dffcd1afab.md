
## Create the DbContext Design-Time Factory

As you recall from Chapter 22, IDesignTimeDbContextFactory is used by the EF Core command-line tooling to create an instance of the derived DbContext class. Create a new class file named ApplicationDbContextFactory.cs in the AutoLot.Dal project in the EfStructures directory. Add the following namespaces to the class:

```csharp
using System;
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Design;
```

The details of the factory were covered in the previous chapter, so I’m just going to list the code here. The additional call to Console.WriteLine() outputs the connection string to the console. This is just used for informational purposes. Make sure to update your connection string to match your environment.

```csharp
namespace AutoLot.Dal.EfStructures
{
    public class ApplicationDbContextFactory : IDesignTimeDbContextFactory<ApplicationDbContext>
    {
        public ApplicationDbContext CreateDbContext(string[] args)
        {
            var optionsBuilder = new DbContextOptionsBuilder<ApplicationDbContext>();
            var connectionString = @"server=.,5433;Database=AutoLot;User Id=sa;Password=P@ssw0rd;";
            optionsBuilder.UseSqlServer(connectionString);
            Console.WriteLine(connectionString);
            return new ApplicationDbContext(optionsBuilder.Options);
        }
    }
}
```

## Create the Initial Migration

Recall that the first migration will create three files: the two files for the migration partial class and the third file is the complete model snapshot. Enter the following in a command prompt in the AutoLot.Dal directory to create a new migration named Initial (using the ApplicationDbContext instance that was just scaffolded) and placing the migration files in the EfStructures\Migrations folder of the AutoLot.Dal project:

```bash
dotnet ef migrations add Initial -o EfStructures\Migrations -c AutoLot.Dal.EfStructures.ApplicationDbContext
```

> **Note** It is important to make sure no changes are applied to the generated files or the database until this first migration is created and applied. Changes on either side will cause the code and database to become out of sync. Once applied, all changes to the database need to be completed through EF Core migrations.

To confirm that the migration was created and is waiting to be applied, execute the list command.

```bash
dotnet ef migrations list -c AutoLot.Dal.EfStructures.ApplicationDbContext
```

The result will show the Initial migration pending (your timestamp will be different). The connection string is shown in the output due to Console.WriteLine() in the CreateDbContext() method.

```text
Build started...
Build succeeded.
server=.,5433;Database=AutoLot;User Id=sa;Password=P@ssw0rd;
20201231203939_Initial (Pending)
```

## Applying the Migration

The easiest method of applying the migration to the database is to drop the database and re-create it. If that is an option, you can enter the following commands and move on to the next section:

```bash
dotnet ef database drop -f
dotnet ef database update Initial -c AutoLot.Dal.EfStructures.ApplicationDbContext
```

If dropping and re-creating the database is not an option (e.g., it is an Azure SQL database), then EF Core needs to believe that the migration has been applied. Fortunately, this is straightforward with EF Core doing all of the work. Start by creating a SQL script from the migration by using the following command:

```bash
dotnet ef migrations script --idempotent -o FirstMigration.sql
```

The relevant portions of this script are the parts that create the `__EFMigrationsHistory` table and then add the migration record into the table to indicate that it was applied. Copy those pieces to a new query in either Azure Data Studio or SQL Server Management Studio. Here is the SQL code that you need (your timestamp will be different):

```sql
IF OBJECT_ID(N'[__EFMigrationsHistory]') IS NULL
BEGIN
    CREATE TABLE [__EFMigrationsHistory] (
        [MigrationId] nvarchar(150) NOT NULL,
        [ProductVersion] nvarchar(32) NOT NULL,
        CONSTRAINT [PK___EFMigrationsHistory] PRIMARY KEY ([MigrationId])
    );
END;
GO

INSERT INTO [__EFMigrationsHistory] ([MigrationId], [ProductVersion])
VALUES (N'20201231203939_Initial', N'5.0.1');
```

Now if you run the list command, it will no longer show the Initial migration as pending. With the initial migration applied, the project and database are in sync, and the development will continue code first.

## Update the Model

This section updates all the current entities to their final version and adds a logging entity. Note that your projects will not compile until this section is complete.

## The Entities

In the Entities directory of the AutoLot.Models project, you will find five files, one for each table in the database. Note that the names are singular and not plural (as they are in the database). This is a change in EF Core 5 where the pluralizer is on by default when scaffolding entities from the database.

The changes you will make to the entities include adding a base class, creating an owned Person entity, fixing navigation property names, and adding some additional properties. You will also add a new entity for logging (that will be used by the ASP.NET Core chapters). The previous chapter covered the EF Core conventions, data annotations, and the Fluent API in depth, so most of this section will be code listings with brief descriptions.

## The BaseEntity Class

The BaseEntity class will hold the Id and TimeStamp columns that are on every entity. Create a new directory named Base in the Entities directory of the AutoLot.Models project. In this directory, create a new file named BaseEntity.cs. Update the code to match the following:

```csharp
using System.ComponentModel.DataAnnotations;
using System.ComponentModel.DataAnnotations.Schema;

namespace AutoLot.Models.Entities.Base
{
    public abstract class BaseEntity
    {
        [Key, DatabaseGenerated(DatabaseGeneratedOption.Identity)]
        public int Id { get; set; }

        [Timestamp]
        public byte[]? TimeStamp { get; set; }
    }
}
```

All of the entities scaffolded from the AutoLot database will be updated to use this base class.

## The Owned Person Entity

The Customer and CreditRisk entities both have FirstName and LastName properties. Entities that have the exact same properties in each can benefit from moving those properties to owned classes. While two properties is a trivial example, owned entities help to reduce code duplication and increase consistency. In addition to the two properties in the classes, a new property is added that will be mapped to a SQL Server computed column.

Create a new directory named Owned in the Entities directory of the AutoLot.Models project. In this new directory, create a new file named Person.cs. Update the code to match the following:

```csharp
using System.ComponentModel.DataAnnotations;
using System.ComponentModel.DataAnnotations.Schema;
using Microsoft.EntityFrameworkCore;

namespace AutoLot.Models.Entities.Owned
{
    [Owned]
    public class Person
    {
        [Required, StringLength(50)]
        public string FirstName { get; set; } = "New";

        [Required, StringLength(50)]
        public string LastName { get; set; } = "Customer";

        [DatabaseGenerated(DatabaseGeneratedOption.Computed)]
        public string? FullName { get; set; }
    }
}
```

The FullName property is nullable since new entities will not have the value set until saved to the database. The final configuration of the FullName property will be added using the Fluent API.

## The Car (Inventory) Entity

The Inventory table was scaffolded to an entity class named Inventory. We prefer to use the name Car. This is easy to fix: change the name of the file to Car.cs and the name of the class to Car. The Table attribute is already applied correctly, so just add the dbo schema. Note that the schema parameter is optional because SQL Server defaults to dbo, but I include it for completeness.

```csharp
[Table("Inventory", Schema = "dbo")]
[Index(nameof(MakeId), Name = "IX_Inventory_MakeId")]
public partial class Car : BaseEntity
{
    ...
}
```

Update the using statements to match the following:

```csharp
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.ComponentModel.DataAnnotations;
using System.ComponentModel.DataAnnotations.Schema;
using System.Text.Json.Serialization;
using AutoLot.Models.Entities.Base;
using Microsoft.EntityFrameworkCore;
```
