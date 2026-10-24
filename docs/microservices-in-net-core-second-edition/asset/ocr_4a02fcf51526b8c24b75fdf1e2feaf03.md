# 12.3.3 Creating the shopping cart GUI

The next bit of GUI you want to add is the contents of the shopping cart. To do this, first extend the /productlist endpoint to call not only the product catalog microservice but also the shopping cart microservice, which means first we add another HttpClient to the IServiceCollection, assuming the shopping cart runs on localhost port 5201.

# Listing 12.11 Adding shopping cart HttpClient to the service collection

```
public void ConfigureServices(IServiceCollection services)
{
    services.AddBasicHealthChecks();
    services.AddControllersWithViews();
    services.Configure<RazorViewEngineOptions>(x =>
        x.ViewLocationFormats.Add("{1}/{0}.cshtml"));
    services.AddHttpClient(
        "ProductCatalogClient",
        client => client.BaseAddress = new Uri("https://localhost:5001"))
        .AddTransientHttpErrorPolicy(p =>
            p.WaitAndRetryAsync(
                3,
                attempt => TimeSpan.FromMilliseconds(100 * Math.Pow(2, attempt))));
    services.AddHttpClient(
        "ShoppingCartClient",
        client => client.BaseAddress = new Uri("https://localhost:5201"));
}
```

Use the new HttpClient to get the contents of the shopping cart.

# Listing 12.12 Extending /productlist to fetch the shopping cart

```
namespace ApiGateway.ProductList
{
    using System.Net.Http;
    using System.Net.Http.Json;
    using System.Text.Json;
    using System.Threading.Tasks;
    using Microsoft.AspNetCore.Mvc;

    public class ProductListController : Controller
    {
        private readonly HttpClient productCatalogClient;
        private readonly HttpClient shoppingCartClient;

        public ProductListController(IHttpClientFactory httpClientFactory)
        {
            this.productCatalogClient =
                httpClientFactory.CreateClient("ProductCatalogClient");
            this.shoppingCartClient =
                httpClientFactory.CreateClient("ShoppingCartClient");
        }
    }
```