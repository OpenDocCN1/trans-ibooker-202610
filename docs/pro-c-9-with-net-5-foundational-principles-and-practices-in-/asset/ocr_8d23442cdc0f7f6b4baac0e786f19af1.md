Next, inherit from BaseEntity, and remove the Id and TimeStamp properties, the constructor, and the pragma #nullable disable. This is the code for the class after these changes:

```
namespace AutoLot.Models.Entities
{
    [Table("Inventory", Schema = "dbo")]
    [Index(nameof(MakeId), Name = "IX_Inventory_MakeId")]
    public partial class Car : BaseEntity
    {
        public int MakeId { get; set; }
        [Required]
        [StringLength(50)]
        public string Color { get; set; }
        [Required]
        [StringLength(50)]
        public string PetName { get; set; }
        [ForeignKey(nameof(MakeId))]
        [InverseProperty("Inventories")]
        public virtual Make Make { get; set; }
        [InverseProperty(nameof(Order.Car))]
        public virtual ICollection<Order> Orders { get; set; }
    }
}
```

There are still some issues with this code that need to be fixed, and there are new properties to add.
The Color and PetName properties are set to be non-nullable, but values aren’t being set in the constructor or initialized with the property definition. This is resolved by assigning each property an initializer. Add the DisplayName attribute to the PetName property for a better, human-readable name. Update the properties to match the following (changes in bold):

```
[Required]
[StringLength(50)]
public string Color { get; set; } = "Gold";

[Required]
[StringLength(50)]
[DisplayName("Pet Name")]
public string PetName { get; set; } = "My Precious";
```

> Note The DisplayName attribute is used by ASP.NET Core and will be covered in Part 8.

The Make navigation property needs to be renamed to MakeNavigation and made nullable, and the inverse property is using a magic string instead of the C# nameof() method. The final change is to remove the virtual modifier. Here is the updated property:

```
[ForeignKey(nameof(MakeId))]
[InverseProperty(nameof(Make.Cars))]
public Make? MakeNavigation { get; set; }
```