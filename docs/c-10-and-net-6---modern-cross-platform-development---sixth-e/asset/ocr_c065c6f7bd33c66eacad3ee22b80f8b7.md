```
{
  WriteLine("Add product successful.");
}

ListProducts();
```

4. Run the code, view the result, and note the new product has been added, as shown in the following partial output:

```
Add product successful.
Id  Product Name          Cost Stock Disc.
078 Bob's Burgers         $500.00       False
038 Côte de Blaye         $263.50 17 False
020 Sir Rodney's Marmalade $81.00 40 False
...
```

# Updating entities

Now, let's modify an existing row in a table:

+   1. In Program.cs, add a method to increase the price of the first product with a name that begins with a specified value (we'll use Bob in our example) by a specified amount like $20, as shown in the following code:

```
static bool IncreaseProductPrice(
    string productNameStartsWith, decimal amount)
{
    using (Northwind db = new())
    {
        // get first product whose name starts with name
        Product updateProduct = db.Products.First(
            p => p.ProductName.StartsWith(productNameStartsWith));

        updateProduct.Cost += amount;

        int affected = db.SaveChanges();
        return (affected == 1);
    }
}
```

+   2. In Program.cs, comment out the whole if block that calls AddProduct, and add a call to IncreaseProductPrice before the call to list products, as shown highlighted in the following code:

```
/*
if (AddProduct(categoryId: 6,
    productName: "Bob's Burgers", price: 500M))
{
```