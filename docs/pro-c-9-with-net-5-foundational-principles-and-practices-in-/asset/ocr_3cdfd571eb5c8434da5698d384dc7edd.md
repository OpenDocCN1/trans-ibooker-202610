```
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

# The Car (Inventory) Entity

The Inventory table was scaffolded to an entity class named Inventory. We prefer to use the name Car. This is easy to fix: change the name of the file to Car.cs and the name of the class to Car. The Table attribute is already applied correctly, so just add the dbo schema. Note that the schema parameter is optional because SQL Server defaults to dbo, but I include it for completeness.

```
[Table("Inventory", Schema = "dbo")]
[Index(nameof(MakeId), Name = "IX_Inventory_MakeId")]
public partial class Car : BaseEntity
{
    ...
}
```

Update the using statements to match the following:

```
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.ComponentModel.DataAnnotations;
using System.ComponentModel.DataAnnotations.Schema;
using System.Text.Json.Serialization;
using AutoLot.Models.Entities.Base;
using Microsoft.EntityFrameworkCore;
```