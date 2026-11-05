```
[Required]
[StringLength(40)]
public string ProductName { get; set; } = null!;

public int? SupplierId { get; set; }
public int? CategoryId { get; set; }

[StringLength(20)]
public string? QuantityPerUnit { get; set; }

[Column(TypeName = "money")] // required for SQL Server provider
public decimal? UnitPrice { get; set; }
public short? UnitsInStock { get; set; }
public short? UnitsOnOrder { get; set; }
public short? ReorderLevel { get; set; }
public bool Discontinued { get; set; }
}
```

+   13. Build the project and fix any compiler errors.
+   If you are using Visual Studio 2022 for Windows, then the compiled application executes in the LinqWithEFcore\bin\Debug\net6.0 folder so it will not find the database file unless we indicate that it should always be copied to the output directory.
+   14. In Solution Explorer, right-click the Northwind.db file and select Properties.
+   15. In Properties, set Copy to Output Directory to Copy always.

# Filtering and sorting sequences

Now let's write statements to filter and sort sequences of rows from the tables:

+   1. In Program.cs, statically import the Console type and namespaces for working with EF Core and your entity model using LINQ, as shown in the following code:
+   2. At the bottom of Program.cs, write a method to filter and sort products, as shown in the following code:

```
using Packt.Shared; // Northwind, Category, Product
using Microsoft.EntityFrameworkCore; // DbSet<T>

using static System.Console;
```

```
static void FilterAndSort()
{
    using (Northwind db = new())
    {
        DbSet<Product> allProducts = db.Products;

        IQueryable<Product> filteredProducts =
```