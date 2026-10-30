If you did not drop your database for the first migration, the procedure already exists and can’t be created. The simple fix is to comment out the call to create the stored procedure in the Up() method, like this:

```
protected override void Up(MigrationBuilder migrationBuilder)
{
// MigrationHelpers.CreateSproc(migrationBuilder);
MigrationHelpers.CreateCustomerOrderView(migrationBuilder);
}
```

After you apply this migration the first time, uncomment that line, and everything will process normally. Of course, another option is to delete the stored procedure from the database and then apply the migration. This does break the “one place for updates” paradigm but is part of the transition from database first to code first.

> Note You could also write code that will first check for an object's existence and drop it if it already exists, but I find that overkill for an issue that might never happen.

# Add the ViewModel

Now that the SQL Server view is in place, it’s time to create the ViewModel that will be used to display the data from the view. The view model will be added as a Keyless DbSet<T>. The advantage to this is that data can be queried using the normal LINQ process common to all DbSet<T> collections.

# Add the ViewModel

Add a new folder named ViewModels in the AutoLot.Models project. In this folder, add a class named CustomerOrderViewModel.cs, and add the following using statements to the file:

```
using System.ComponentModel.DataAnnotations.Schema;
using Microsoft.EntityFrameworkCore;
```

Next, update the code to the following:

```
namespace AutoLot.Models.ViewModels
{
[Keyless]
public class CustomerOrderViewModel
{
public string? FirstName { get; set; }
public string? LastName { get; set; }
public string? Color { get; set; }
public string? PetName { get; set; }
public string? Make { get; set; }
public bool? IsDrivable { get;set; }
[NotMapped]
public string FullDetail =>
$"{FirstName} {LastName} ordered a {Color} {Make} named {PetName}";
```