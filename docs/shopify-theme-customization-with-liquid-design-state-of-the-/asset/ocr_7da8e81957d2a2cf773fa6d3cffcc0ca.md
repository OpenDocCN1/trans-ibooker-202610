3. For example, let's say that we were looking to update the total price on the cart page every time we update the product quantity. To start, we will use the fetch method to retrieve the total price value:

```
fetch('/cart.js')
.then(response => response.json())
.then(data => {
    console.log(data.total_price);
});
```

While we successfully retrieved the price, all that we have received is an unformatted string value, which is not that useful to us:

```
78594
```

4. The easiest way to resolve this would be to look into how the theme developer has defined the currency formatting helper function throughout the theme. We can usually find it inside the theme master js file. In our case, this will be theme.js.

After identifying the keywords that we need, we simply need to apply the formatting to the value that we are looking to format:

```
fetch('/cart.js')
.then(response => response.json())
.then(data => {
    console.log(theme.Currency.formatMoney(
        cart.total_price, theme.moneyFormat));
});
```

Note that the formatting we have used in the previous example will work without any modifications in most cases. However, we might need to make some adjustments to specific themes – it all depends on how the theme developer defined the function.

Previously, we learned how to retrieve information about the current cart session and any data about any product inside the cart. However, what if we wanted to retrieve product information more directly?