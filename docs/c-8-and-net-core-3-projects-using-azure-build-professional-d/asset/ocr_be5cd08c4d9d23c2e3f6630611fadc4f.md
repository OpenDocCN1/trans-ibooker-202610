```
return salesOrder;
}
```

There's a bit more to this method, but essentially all we're doing is calling the product service to retrieve a list of products and then picking one at random. Finally, we're creating a sales order with a random quantity.

The product service reads a list of products from a product repository; for the sake of completeness, I'll list both the product service and its interface here. However, since it only calls through to the product repository, I won't go into any further explanation of the code:

```
public interface IProductService
{
	IEnumerable<Models.Product> GetProductData();
}
```

The implementation is as follows:

```
public class ProductService : IProductService
{
	private readonly IProductRepository _productRepository;
	public ProductService(IProductRepository productRepository)
	{
		_productRepository = productRepository;
	}

	public IEnumerable<Models.Product> GetProductData()
	{
		return _productRepository.GetProductData();
	}
}
```

Again, I'll list the repository interface, but it doesn't warrant any explanation:

```
public interface IProductRepository
{
	IList<SalesOrder.Models.Product> GetProductData();
}
```