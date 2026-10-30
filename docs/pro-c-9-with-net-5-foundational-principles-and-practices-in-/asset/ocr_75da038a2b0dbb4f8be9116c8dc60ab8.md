```
using System.ComponentModel.DataAnnotations;
using System.ComponentModel.DataAnnotations.Schema;
public abstract class BaseEntity
{
    [Key, DatabaseGenerated(DatabaseGeneratedOption.Identity)]
    public int Id { get; set; }
    [Timestamp]
    public byte[] TimeStamp { get; set; }
}
```

Here is the Car class and the data annotations that shape it in the database:

```
using System.Collections.Generic;
using System.ComponentModel.DataAnnotations;
using System.ComponentModel.DataAnnotations.Schema;
using Microsoft.EntityFrameworkCore;

[Table("Inventory", Schema="dbo")]
[Index(nameof(MakeId), Name = "IX_Inventory_MakeId")]
public class Car : BaseEntity
{
    [Required, StringLength(50)]
    public string Color { get; set; }
    [Required, StringLength(50)]
    public string PetName { get; set; }
    public int MakeId { get; set; }
    [ForeignKey(nameof(MakeId))]
    public Make MakeNavigation { get; set; }
    [InverseProperty(nameof(Driver.Cars))]
    public IEnumerable<Driver> Drivers { get; set; }
}
```

The Table attribute maps the Car class to the Inventory table in the dbo schema (the Column attribute is used to change a column name or data type). The Index attribute creates an index on the foreign key MakeId. The two text fields are set to be Required and a max StringLength of 50 characters. The InverseProperty and ForeignKey attributes are explained in the next section.

The changes from the EF Core conventions are as follows:

+   - Renaming the table from Cars to Inventory
+   - Changing the TimeStamp column from varbinary(max) to the SQL Server timestamp data type
+   - Setting the datatype and nullability for the Color and PetName columns from nvarchar(max)/null to nvarchar(50)/not null
+   - Renaming the index on MakeId

The rest of the annotations used match the configuration defined by the EF Core conventions.

If you were to create a migration and try to apply it, the migration will fail. SQL Server doesn’t allow an existing column to be changed to a timestamp data type from another data type. The column has to be dropped and re-created. Unfortunately, the migration infrastructure doesn’t drop and re-create. It attempts to alter the column.