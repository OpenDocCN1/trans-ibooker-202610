Then the logger can be used in, for example, the endpoint for adding items to the shopping cart, which you can see in the following listing.

# Listing 9.6 Including structure in logs from controllers

```
[HttpPost("{userId:int}/items")]
public async Task<ShoppingCart> Post(int userId, [FromBody] int[] productIds)
{
    var shoppingCart = await shoppingCartStore.Get(userId);
    var shoppingCartItems = await this.productCatalog.GetShoppingCartItems(productIds);
    await shoppingCart.AddItems(shoppingCartItems, this.eventStore);
    await this.shoppingCartStore.Save(shoppingCart);

    this.logger.LogInformation(
        "Successfully added products to shopping cart {@productIds}, {@shoppingCart}", productIds, shoppingCart);

    return shoppingCart;
}
```

Log a structured message.

If we make an HTTP post to add items to the cart now, one of the log messages we will see in the console is as follows.

# Listing 9.7 Structured log message including a shopping cart

```
2020-12-03 11:37:08 INF Successfully added products to shopping cart [1, 2], {"userId":123,"items":[{"productCatalogueId":1,"productName":"Basic t-shirt","description":"a quiet t-shirt","price":{"currency":"eur","amount":40}},{ "productCatalogueId":2,"productName":"Fancy shirt","description":"a loud t-shirt","price":{"currency":"eur","amount":50}}]}
```

This log message includes the whole shopping cart as JSON, which provides a lot more context to the message than the simple “Successfully added products to shopping cart” message. This is the strength of structured logging.

The last step we need to make the log message a bit easier to consume in a log shipping pipeline. To that end, we format them as JSON logs. This makes the message a bit harder to read for humans, though, so I prefer to use the output we’ve seen so far in development and the JSON output in production, as shown in the following listing. This is done by telling Serilog to JSON format the output to the console when the environment is not “development.”

# Listing 9.8 Using JSON logs in production

```
private static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .UseSerilog((context, logger) =>
        {
            logger
```