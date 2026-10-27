# The /cart.js endpoint

The `/cart.js` endpoint, as its name suggests, allows us to access the current cart session and retrieve all the information about the cart, as well as products inside the cart. We can use it to dynamically update the cart page or even create a cart drawer for the store and improve the purchase flow significantly. Let's take a look:

+   1. We can retrieve information about the cart using the following `fetch` method:

```
fetch('/cart.js')
  .then(response => response.json())
  .then(data => {
    console.log(data);
  });
```

Note that the response of a successful GET request is a JSON object. The following example shows the response we will receive using the previous code to fetch the cart data:

```
{
  "token": "fd2d9bc86cfa72228b4de6bff52fe915",
  "note": null,
  "attributes": {},
  "original_total_price": 78594,
  "total_price": 78594,
  "total_discount": 0,
  "total_weight": 0,
  "item_count": 9,
  "items": [],
  "requires_shipping": true,
  "currency": "USD",
  "items_subtotal_price": 78594,
  "cart_level_discount_applications": []
}
```

As we can see, the JSON object provides us with all the necessary information about the cart and each product separately. Note that the variant information can be accessed through the items array object, which we have minified to keep everything concise.