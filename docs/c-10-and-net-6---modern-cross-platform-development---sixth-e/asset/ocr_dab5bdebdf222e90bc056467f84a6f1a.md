Let's configure those ports:

1. In the Northwind.WebApi project, in Program.cs, add an extension method call to UseUrls to specify port 5002 for HTTPS, as shown highlighted in the following code:

```
var builder = WebApplication.CreateBuilder(args);
builder.WebHost.UseUrls("https://localhost:5002/");
```

2. In the Northwind.Mvc project, open Program.cs and import the namespace for working with HTTP client factory, as shown in the following code:

```
using System.Net.Http.Headers; // MediaTypeWithQualityHeaderValue
```

3. Add a statement to enable HttpClientFactory with a named client to make calls to the Northwind Web API service using HTTPS on port 5002 and request JSON as the default response format, as shown in the following code:

```
builder.Services.AddHttpClient(name: "Northwind.WebApi",
    configureClient: options =>
    {
        options.BaseAddress = new Uri("https://localhost:5002/");
        options.DefaultRequestHeaders.Accept.Add(
            new MediaTypeWithQualityHeaderValue(
            "application/json", 1.0));
    });
```

# Getting customers as JSON in the controller

We can now create an MVC controller action method that uses the factory to create an HTTP client, makes a GET request for customers, and deserializes the JSON response using convenience extension methods introduced with .NET 5 in the System.Net.Http.Json assembly and namespace:

1. Open Controllers/HomeController.cs and declare a field to store the HTTP client factory, as shown in the following code:

```
private readonly IHttpClientFactory clientFactory;
```

2. Set the field in the constructor, as shown highlighted in the following code:

```
public HomeController(
    ILogger<HomeController> logger,
    NorthwindContext injectedContext,
    IHttpClientFactory httpClientFactory)
{
    _logger = logger;
    db = injectedContext;
    clientFactory = httpClientFactory;
}
```