
To implement this UI, you'll perform the following steps:

+   1. Reuse the product catalog and shopping cart microservices from chapter 5.
+   2. Create an API Gateway.
+   3. Create the product list from figure 12.9:
+      - Create an endpoint in the API Gateway for fetching a page with the product list.
+      - Make the new endpoint read the list of products from the product catalog.
+      - Create a view with the list of products and return it from the new endpoint.
+   4. Add the shopping cart from figure 12.9 to the web page:
+      - Make the API Gateway get the current state of the shopping cart from the shopping cart.
+      - Add the shopping cart to the web page.
+   5. Create an endpoint in the API Gateway for adding products to the shopping cart:
+      - Add a POST endpoint for adding products to the shopping cart in the API Gateway.
+      - Call the shopping cart from the API Gateway to add the new product.
+      - Update the view the user sees to reflect that a product was added to the shopping cart.
+   6. Create an endpoint in the API Gateway to remove products from the shopping cart.

You’ll use the product catalog microservice and the shopping cart microservice from chapter 5 just as they are in the new API Gateway. You could, at this point, add the microservice platform from chapter 11 to these microservices, but for the sake of brevity you’ll skip that step. The only thing you’ll do in these microservices is run them on different ports when you run them at the same time on your development machine. You specify these ports in command line.

> NOTE To keep the scope of this example tenable, I’ve cut some corners. Most notably, you won’t implement a login system. Instead, you’ll trust the user ID in the URL. Never do that in production. In chapter 10, we discussed how to deal with security in general, including authenticating end users, and how you should obtain the user ID.

## 12.3.1 Creating an API Gateway

Create a new project for the API Gateway the same way you’ve created projects many times before. Call the new project ApiGateway. Then, add the microservice platform you developed in chapter 11 as a NuGet package to the new project. The project file should look like the following.

```
services.AddControllers();
}

public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    app.UseRouting();
    app.UseKubernetesHealthChecks();
    app.UseEndpoints(endpoints => endpoints.MapControllers());
}
}
```

Now you have an empty project, ready for the implementation of the API Gateway.

## 12.3.2 Creating the product list GUI

The next step is to create the part of the application that lists products. Add a new MVC controller called ProductListController to a folder called ProductList in the API Gateway, and add a /productlist endpoint to that module. The ProductListController will contain all the endpoints that serve the application to end users: it will have endpoints that give end users a web GUI and also endpoints used by the JavaScript in that web GUI.

To keep it simple at first, let’s begin with an endpoint that does nothing. Then you’ll add a GUI based on a hardcoded list of products, and finally you’ll retrieve the real list of products from the product catalog microservice.

The ProductListController will serve the web frontend to end users, but start with the following endpoint that always responds with an empty 501 Not Implemented.

### Listing 12.4 Placeholder endpoint implementation in the API Gateway

```
namespace ApiGateway.ProductList
{
    using Microsoft.AspNetCore.Mvc;
    using System.Threading.Tasks;

    public class ProductListController : Controller
    {
        [HttpGet("/productlist")]
        public async Task<IActionResult> ProductList([FromQuery] int userId)
            => StatusCode(501);
    }
}
```

The next step is to add a GUI that shows the product list. So far, you’ve returned data—for example, in the form of JSON—from all endpoints. Now, you’ll return a GUI, in the form of server side-generated HTML using the Razor view engine, which allows us to dynamically generate HTML on the server side. To use Razor, you need to change the way you configure MVC slightly in the ConfigureServices method in the Startup class.

```
```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddBasicHealthChecks();
    services.AddControllersWithViews();
}
```
```

To make the `/productlist` endpoint return a view, change it, as shown next. It returns a view called productlist and passes a hardcoded list of products into the view as a model object.

```
```csharp
namespace ApiGateway.ProductList
{
    using Microsoft.AspNetCore.Mvc;
    using System.Threading.Tasks;

    public class ProductListController : Controller
    {
        [HttpGet("/productlist")]
        public async Task<IActionResult> ProductList([FromQuery] int userId)
        {
            var products = new[]
            {
                new Product(1, "T-shirt", "Really nice t-shirt"),
                new Product(2, "Hoodie", "The coolest hoodie ever"),
                new Product(3, "Jeans", "Perfect jeans"),
            };
            return View(new ProductListViewModel(products));
        }
    }

    public record Product(int ProductId,
        string ProductName, string Description);
    public record ProductListViewModel(Product[] Products);
}
```
```

Listing 12.6 uses an MVC feature you haven’t used before: `View(new ProductListViewModel(products));`. This is how you return a view from an MVC controller. The argument is a model object that will be passed to the view and that can be used while rendering the view. By convention the name of the view is equal to the name of the action method—ProductList in this case. This convention can be overridden by passing in a view name as the first parameter to `View(...)`, but here the convention is fine.

To implement the ProductList view, create a new file called ProductList.cshtml next to ProductListController.cs in the ProductList folder. The file extension .cshtml tells ASP.NET that this is a Razor view. The ProductList.cshtml file contains a simple view that iterates over the list of products in the model object and builds an HTML list

from the products. To give the page just a little bit of structure, you import the Bootstrap CSS framework (http://getbootstrap.com), and add a few Bootstrap CSS classes here and there.

### Listing 12.7 Simple product list view

```
```html
@model ApiGateway.ProductList.ProductListViewModel

<!DOCTYPE html>
<html>

<head>
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@4.6.0/dist/css/bootstrap.min.css" integrity="sha384-B0vP5XAtW+1K9RQLQERJvTumQW0nPEZvF6L/Z6nronJ3O0UFPpCjEUQouq2+1" crossorigin="anonymous">
    <title>MicroCommerce.NET</title>
</head>
<body>
<div class="container">
    <div class="page-header">
        <h1>MicroCommerce <small>– Product List</small></h1>
    </div>
    <div class="row">
        <div class="col-md-8">
            @foreach (var product in Model.Products)
            {
                <div class="row" style="border-bottom-style: solid">
                    <div class="col-md-8">
                        <h4>@product.ProductName</h4>
                        <p>@product.Description</p>
                    </div>
                    <div class="col-md-4">
                        <p></p>
                        <button class="btn btn-primary" type="button">
                            BUY!
                        </button>
                    </div>
                </div>
            }
        </div>
    </div>
</div>
</body>
</html>
```
```

If you run this, it will fail with a runtime error because ASP.NET cannot locate the ProductList.cshtml file. This is because ASP.NET expects view files to be in certain locations by default, but we put it right next to the controller, which is not one of the places ASP.NET expects. But, as I have mentioned in previous chapters, I like to group files by what they are about in terms of the domain—the product list in this case. To fix this, we need to add a bit of code to the ConfigureService in the Startup class.

![](img/608267d7803dc57a1a896ad121f37868_304_0.png)
