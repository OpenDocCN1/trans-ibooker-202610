```
// a query to get all categories and their related products
IQueryable<Category>? categories = db.Categories?
    .Include(c => c.Products);

if (categories is null)
{
    WriteLine("No categories found.");
    return;
}

// execute query and enumerate results
foreach (Category c in categories)
{
    WriteLine($"{c.CategoryName} has {c.Products.Count} products.");
}
}
```

3. At the top of Program.cs, after outputting the database provider name, call the QueryingCategories method, as shown highlighted in the following code:

```
WriteLine($"Using {ProjectConstants.DatabaseProvider} database provider.");
QueryingCategories();
```

4. Run the code and view the result (if run with Visual Studio 2022 for Windows using the SQLite database provider), as shown in the following output:

```
Using SQLite database provider.
Categories and how many products they have:
Using C:\Code\Chapter10\WorkingWithEFCore\bin\Debug\net6.0\Northwind.db database file.
Beverages has 12 products.
Condiments has 12 products.
Confections has 13 products.
Dairy Products has 10 products.
Grains/Cereals has 7 products.
Meat/Poultry has 6 products.
Produce has 5 products.
Seafood has 12 products.
```

![](bbox=[0.12, 0.8, 0.2, 0.88])

If you run with Visual Studio Code using the SQLite database provider, then the path will be the WorkingWithEFCore folder. If you run using the SQL Server database provider, then there is no database file path output.