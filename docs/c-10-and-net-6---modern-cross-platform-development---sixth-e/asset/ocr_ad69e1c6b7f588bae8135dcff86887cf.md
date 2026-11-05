1. In FilterAndSort, add a statement to extend the LINQ query to use the Select method to return only the three properties (that is, table columns) that we need, and modify the foreach statement to use the var keyword and the projection LINQ expression, as shown highlighted in the following code:

```
IOrderedQueryable<Product> sortedAndFilteredProducts =
    filteredProducts.OrderByDescending(product => product.UnitPrice);

var projectedProducts = sortedAndFilteredProducts
    .Select(product => new // anonymous type
    {
        product.ProductId,
        product.ProductName,
        product.UnitPrice
    });

WriteLine("Products that cost less than $10:");
foreach (var p in projectedProducts)
{
```

2. Hover your mouse over the new keyword in the Select method call and the var keyword in the foreach statement and note that it is an anonymous type, as shown in Figure 11.4:

![](bbox=[0.13, 0.49, 0.89, 0.7])

Figure 11.4: An anonymous type used during LINQ projection

3. Run the code and confirm that the output is the same as before.

# Joining and grouping sequences

There are two extension methods for joining and grouping:

+   - **Join**: This method has four parameters: the sequence that you want to join with, the property or properties on the left sequence to match on, the property or properties on the right sequence to match on, and a projection.