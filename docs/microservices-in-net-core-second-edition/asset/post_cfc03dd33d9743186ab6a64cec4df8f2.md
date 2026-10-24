
```
[
    {
        "productId": "1",
        "productName": "Basic t-shirt",
        "productDescription": "a quiet t-shirt",
        "price": { "amount": 40, "currency": "eur" },
        "attributes": [
            {
                "sizes": ["s", "m", "l"],
                "colors": ["red", "blue", "green"]
            }
        ]
    },
    {
        "productId": "2",
        "productName": "Fancy shirt",
        "productDescription": "a loud t-shirt",
        "price": { "amount": 50, "currency": "eur" },
        "attributes": [
            {
                "sizes": ["s", "m", "l", "xl"],
                "colors": ["ALL", "Batique"]
            }
        ]
    }
]
```

This JSON must be deserialized, and the information required to create a list of ShoppingCart items needs to be read from it. The array returned from product catalog is formatted by the microservice’s API. To avoid tight coupling between microservices, only the ProductCatalogClient class knows anything about the API of the product catalog microservice. That means the ProductCatalogClient is responsible for translating the data received from the microservice into types for the ShoppingCart project. In this case, you need a list of ShoppingCartItem objects. The following listing shows the code for deserializing and translating the response data.

### Listing 2.11 Extracting data from the response

```csharp
private static async Task<IEnumerable<ShoppingCartItem>>
    ConvertToShoppingCartItems(HttpResponseMessage response)
{
    response.EnsureSuccessStatusCode();
    var products = await
        JsonSerializer.DeserializeAsync<List<ProductCatalogProduct>>(
            await response.Content.ReadAsStreamAsync(),
            new JsonSerializerOptions
            {
                PropertyNameCaseInsensitive = true
            }) ?? new();
    return products
        .Select(p =>
            new ShoppingCartItem(
                p.ProductId,
                p.ProductName,
                p.ProductDescription,
                p.Price));
}

private record ProductCatalogProduct(
    int ProductId,
    string ProductName,
    string ProductDescription,
    Money Price);
```

- Uses System.Text.Json to deserialize the JSON from the product catalog microservice.
- Creates a ShoppingCartItem for each product in the response.
- Uses a private record to represent the product data.

If you compare listings 2.10 and 2.11, you may notice that there are more properties in the response than in the ProductCatalogProduct class. This is because the Shopping Cart microservice doesn’t need all the information, so there’s no reason to read the remaining properties. Doing so would only introduce unnecessary coupling. I’ll return to this topic in chapters 5, 6, and 8.

The following listing combines the code that requests the product information and the code that parses the response. This method makes the HTTP GET request and translates the response to the domain of the shopping cart.

### Listing 2.12 Fetching products and converting them to shopping cart items

```csharp
public async Task<IEnumerable<ShoppingCartItem>>
    GetShoppingCartItems(int[] productCatalogIds)
{
    using var response =
        await RequestProductFromProductCatalogue(productCatalogIds);
    return await ConvertToShoppingCartItems(response);
}
```

The ProductCatalogClient is almost finished. The only part missing is the code that handles an HTTP request failure.

## 2.2.5 Adding a failure-handling policy

Remote calls can fail. Not only can they fail, but when running a distributed system at scale, remote calls often do fail. You may not expect the call from shopping cart to product catalog to fail often, but in an entire system of microservices there will often be a failing remote call somewhere in the system.

Remote calls fail for many reasons: the network can fail, the call could be malformed, the remote microservice might have a bug, the server where the call is handled may fail during processing, or the remote microservice might be in the middle of a redeploy. In a system of microservices, you must expect failures and design a level of resilience around every place remote calls are made. This is an important topic, and I’ll go into more detail in chapter 7.

The level of resilience needed around a particular remote call depends on the business requirements for the microservice making the call. The call to the product catalog microservice from the shopping cart microservice is important; without the product information, the user can’t add items to their shopping cart, which means the e-commerce site can’t sell the items to the user. On the other hand, product information doesn’t change often, so you could store a copy of it in the shopping cart and only request it from the product catalog when the copy doesn’t already contain the information. One way to populate such a copy is by caching responses from the product catalog. Caching product information has some significant advantages:

- It makes the shopping cart more resilient to failures in product catalog.
- The shopping cart microservice will perform better when the product information is present in the cache.
- Fewer calls made from the shopping cart microservice mean less stress is put on the product catalog microservice.

For now, you won’t implement caching; we’ll return to the subject of caching for the sake of robustness in chapter 7.

Even with caching in place, some calls from the shopping cart to the product catalog are still made. For these calls, you may decide that the best strategy for handling failed calls is to retry the call a couple of times and then give up and fail to add any items to the shopping cart. For this chapter, you’ll implement a simple retry policy for handling failing requests. You’ll use the Polly library and the Microsoft.Extensions.Http.Polly extension to Polly, both of which you’ll install in the ShoppingCart project as a NuGet package.

> **NOTE** Failure-handling strategies and Polly are described in much more detail in chapter 7.

Using a Polly policy involves these two steps:

1. Declaring the policy.
2. Using the policy to execute the remote call.

As you can see in the following listing, Polly’s API and integration with ASP.NET makes both these steps easy. Replace the current registration of ProductCatalogClient in Startup with this:

### Listing 2.13 Microservice error-handling policy

```csharp
services.AddHttpClient<IProductCatalogClient, ProductCatalogClient>()
    .AddTransientHttpErrorPolicy(p =>
        p.WaitAndRetryAsync(
            3,
            attempt => TimeSpan.FromMilliseconds(100*Math.Pow(2, attempt))));
```

- Wraps http calls made in ProductCatalogClient in a Polly policy.
- Uses Polly’s fluent API to set up a retry policy with an exponential back-off.

The HttpClient injected in ProductCatalogClient will use this policy around the call to the product catalog microservice: in case of failure, retry the call at most three times. And for each failure, double the amount of waiting time before making the next attempt.

This completes the implementation of `ProductCatalogClient`. Even though it has fewer than *70* lines of code, it does a lot: it builds up the HTTP GET request and executes it. It parses the response from product catalog and translates it into the shopping cart domain. And it uses the retry policy used for these calls. Next, let's tackle the event feed.

## 2.2.6 Implementing a basic event feed

The shopping cart microservice can now store shopping carts and add items to them. The items include product information from the product catalog microservice. The shopping cart also has an API for other microservices that allows them to add or delete items from shopping carts and read the contents of a shopping cart.

The piece missing is the event feed. The shopping cart needs to publish events about changes to shopping carts, and other microservices can subscribe to these events and react to them as required. In the case of items being added to a shopping cart, figure 2.9 (repeated from Chapter 1) illustrates how the recommendations microservice and the shopper tracking microservice base part of their functionality on events from the shopping cart microservice.

![](img/608267d7803dc57a1a896ad121f37868_69_0.png)

Figure 2.9 The shopping cart microservice publishes events about changes to shopping carts to an event feed. The recommendations and shopper tracking microservices subscribe to these events and react as events arrive.

In this section, you'll implement the `EventFeed` and shopping cart domain model components highlighted in figure 2.10. (Chapter 5 returns to the implementation of event feeds and event subscribers.) The domain model is responsible for raising events, and `EventFeed` allows other microservices to read the events that the shopping cart microservice has published.
